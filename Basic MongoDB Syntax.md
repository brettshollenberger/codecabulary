# Basic MongoDB Syntax

MongoDB CRUD operations are very similar to SQL operations. Let's take a look at performing some CRUD operations, indexing, and Mongo's SQL-like `explain` command:

#### Create a Database

Using a database is simple, just type, `use database`:

	use tutorial
	
If the tutorial database doesn't exist, it won't be created now. It will only be created once we've added a collection to it, so let's do that now: 

#### Insert Documents:

Collections in MongoDB are schemaless, meaning they can be created dynamically. All we have to do is specify a collection, and if it doesn't exist, Mongo will create one for us:

	db.users.insert({username: "brettshollenberger"});
	
Assuming the users collection doesn't exist, it will be created now for us, and then one entry will be added, with the username "brettshollenberger." If the collection does exist, Mongo will add the document for us. If the schemas don't match, that's okay, because Mongo doesn't require a strict schema, although in practice we should use one as much as possible. 

One major fear about dynamically created collections is typos and our ability to accidentally create new users instead of Mongo catching our error and yelling at us that the collection doesn't exist. We can ensure that we don't screw up by using strict mode:

	use strict
	
Simple enough. 

#### Find Documents:

Finding documents is as easy as you might expect. To find all documents:

	db.users.find();
	
Returns exactly what we're looking for. To add query selectors, just use JSON syntax:

	db.users.find({username: "brettshollenberger"});
	
#### Update Documents

Update syntax requires two arguments: a query selector that will find the appropriate documents, and what to update the documents to. If we use plain Javascript object syntax for the second argument, the documents will be updated to be _exactly_ what we've typed:

	db.users.update({username: "brettshollenberger"}, {"email: "brett.shollenberger@gmail.com"});
	
This update doesn't _add_ the email attribute; it replaces the entire document:

	db.users.find();
	> {"email": "brett.shollenberger@gmail.com"}
	
If we want to add attributes to the document, or update particular attributes, we can use `$set`:

	db.users.update({email: "brett.shollenberger@gmail.com"}, {$set: {username: "brettshollenberger"}});
	
Now this only updates the first document it finds that matches the query selector. What if we know that any currently active users are eligible for some promotion? We want to set promotionEligible to true for each of these users. The fourth parameter in the `update` method is `multi-update`:

	db.users.update({status: "active"}, {$set: {promotionEligible: true}}, false, true);
	
By setting the fourth parameter to true, we've now performed a multi-update. 

There are also special operators for working with arrays (since MongoDB is cool enough to provide such rich data types, we might as well work with them).

`$push` you're probably familiar with regardless of your language of expertise. It's an array method that tacks on an item to the end of an array. It works the same way here:

	db.users.update({username: "brettshollenberger"}, {$push: {"favorites.shows": "Breaking Bad"}});
	
Even if we hadn't yet created the favorites document or the shows array on the favorites document, Mongo is smart enough to create those for us now, _provided we use dot syntax in quotations_. Breaking Bad will now be pushed onto the end of my list of favorite shows.

Mongo also gives us the `$addToSet` method, which assumes the array is a mathematical _set_, aka a collection of unique objects. `$push` in any language will allow us to push duplicates onto the end of the array; `$addToSet` treats the array as a set, and won't push on duplicates:

	db.users.update({username: "brettshollenberger"}, {$addToSet: {"favorites.shows": "Breaking Bad"}});
	
	db.users.find();
	{ "_id" : ObjectId("523f57c5a99a00f5eb30c572"), "favorites" : { "shows" : [  "Breaking Bad" ] }, "username" : "brettshollenberger" }
	
As you see here, we don't add the redundant copy of _Breaking Bad_ to the favorite shows array.

#### Remove Documents and Dropping Collections

Remve all documents from a collection:

	db.users.remove();
	
Remove only documents matching the query selector: 

	db.users.remove({username: "brettshollenberger"});
	
Drop a collection:

	db.users.drop();
	
#### Diagnose & Improve Inefficient Queries

As in SQL, the explain method describes query paths and allows us to diagnose slow operations by determining which indices a query has used, if any. If we make a sufficiently large collection listing the numbers from 1 to 200,000, and find all numbers equal to 1, we'll obviously only return one document, but we'll see that we scan all 200,000:

	db.numbers.find({num: 1}).explain();
	{
		cursor: "BasicCursor", 	# no index used to return the result set
		nscanned: 200000, 		# all documents we scanned in the making of this movie
		n: 4,					# but only four results were returned. Ouch.
		millis: 63				# took 63 milliseconds on my machine
		...					# truncated 
	}
	
Obviously that's a bit dumb. Any time we have a wide disparity between the number of documents scanned and the number of documents returned, we're in prime territory to add an index.

We can add a secondary index to the `num` property to see how this improves. When we create an index, we specify the key:

	db.numbers.ensureIndex({num: 1});
	
We can now test the same query:

	db.numbers.find({num: 1}).explain();
	{
		cursor: "BtreeCursor num_1", 	# searches the index (in the form of a B-tree) to obtain the indices of the elements it needs
		nscanned: 1,				# only 1 document was scanned, but a number of node searches had to be performed in the B-tree that aren't documented here
		n: 1,						# 1 document was returned
		millis: 0					# the query took less than 0 milliseconds on my computer
		...
	}
	
It's important to note here that the B tree does need to be scanned, but the number of potential comparisons to be made is far fewer than 200,000 using the B tree data structure. 
