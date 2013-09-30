# Document-Oriented Databases

Developers tend to be familiar with relational databases--they're the ones we learn about in school and the ones that have powered our big projects. You've likely also heard about document-oriented databases or NoSQL databases like MongoDB and wondered what the hype was about and what the differences were between traditional relational databases and nonrel databases. Really, there are at least three distinct types of databases: relational, key-value store, and sophisticated key-value stores. Today, I'm looking at what sets MongoDB apart, and why it's the most popular NoSQL datastore, and competing well with the likes of Postgres and a number of other relational databases. 

#### 1) No Normalization / Richer Object Modelling

In our object-oriented languages, we have full, rich representations of objects that we serialize into storable data. Non-relational databases still serialize data, but single objects are represented as single complex data structures in NoSQL databases; in other words, we don't normalize the data.

Normalization is the process of decomposing objects into simpler data-types that have a fairly standardized structure. We can't know how many comments a blog post will have, and in relational databases, we can't store arrays of other documents, so we might break a comment out into its own table with a standardized set of properties (commenter and text), and we add on `post_id`, the primary key of the post on which the comment exists. This allows a comment to follow a predictable structure, and for it to still be assignable to a comment. 

In MongoDB we can store arrays of documents, so the important question to ask ourselves is "Would a comment ever operate as a self-contained entity?" Would we have a page listing all comments without the context of the post they belong to? If the answer is no, then in Mongo it really makes more sense for comments to be a property of a post document. 

	{ 
		_id: ObjectID('4bd9e8e17cefd644108961bb'),
		 title: "The Very Best Post",
		 content: "Super great content",
		 comments: [
		 	{ author: "DaveFinch",
		 	  text: "What a great post!"
		 	},
		 	{ author: "DeniseHarnett",
		 	  text: "I loved it!"
		 	}
		 ]
	}
	
Mongo falls into the category of rich key-value stores; we can use it to represent both primitive data types (Strings, Numbers, Dates), and complex data types (Arrays, other documents, arrays of other documents). Since normalization in and of itself requires assembly of complete objects in our applications, normalization can be costly--requiring complex joins and additional queries--some of the most time-consuming parts of working with an application. 

> The Document-Oriented Data model represents data in aggregate form. 

#### 2) Ad Hoc Queries

Relational databases support ad hoc queries; if it's valid SQL, you can write a query for it. It's hard not to take ad hoc queries for granted coming from a relational database background, but simple key-value stores can only be queried by key, not complex relationships. MongoDB, as a sophisticated key-value store does support ad hoc queries (granted, not in the SQL you'll be familiar with), but the syntax is easy to pick up on:

	db.posts.find({'comment_count': {'$gt': 10}});
	
Notice that of course no joins are required; Mongo expects comments to be a property of posts rather than another collection related to posts. The built-in `$gt` attribute allows us to search for posts with more than 10 comments. 

Although ad hoc queries are easy to take for granted, if any datastore is going to act as your primary database, and not some additional caching layer to improve read speed when 1,000 people are reading from the same information from your database at the same time, then ad hoc queries are a must. Simple key-value stores are only ever going to be a solution to niche problems like caching and web ops, but sophisticated key-value stores like Mongo, Cassandra, and Project Voldemort are as robust as SQL-based relational databases.

#### 3) Secondary Indices

Secondary indices are another must for fast databases. The common analogy for secondary indices is that of a book index: it's much faster to look for references to MongoDB in the book index then to scan each page for references. Similarly, you can declare a secondary index on any "column" in a MongoDB database (and relational database columns), that adds a minor decrease to write speed and a significant increase to read speed. MongoDB collections support up to 64 secondary indices.

#### 4) Horizontal Scaling

MongoDB is better suited for horizontal scaling (adding more devices) than vertical scaling (upgrading a single device). Horizontal scaling is often more-cost effective and protects you from losing data, as a single machine only represents a smaller part of your total stack. MongoDB will handle sharding for you, and provides automated failover, which relational databases do not do. Relational database stacks tend toward vertical scaling (unless someone in house knows how to scale horizontally).

#### 5 ) Transactions

MongoDB supports atomic updates, and thus a subset of what's possible with traditional transactions.

