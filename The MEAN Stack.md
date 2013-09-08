# The MEAN Stack

The MEAN Stack is a collection of Javascript-oriented tools that represents a common RESTful application setup. A RESTful application is one that deals with data manipulation--Facebook, Gmail, iCal, you name it; most applications we use day-to-day fall into this category with the exception of games, although something like Dungeons and Dragons could fall into the category as well. Since we need to store, visualize, and interact with data, the general requirements for a RESTful application are:

* A database: A means of persisting data from one use of the application to another and a means of separating my data from yours. A database allows us to save a representation of an object (like a Facebook profile) in a text orientation that we can reassemble later into the object it represents--a profile. Inherently, we must save our data somehow if people are going to reuse it later. Many stacks use relational databases like MySQL or PostgreSQL, while some newer stacks use Document-Oriented Databases (also called NoSQL databases) to persist data--but don't get hung up on the jargon. You can read books (or just Wiki articles) on the differences, but the MEAN stack uses MongoDB, a NoSQL datastore that saves documents as JSON objects (JSON stands for Javascript Object Notation, hence why it's very easy to use MongoDB in a Javascript-centric stack). JSON objects are much like hash tables, which are fairly easy to read at first glance even if the notation is a bit unfamiliar:

		{
			name: "Brett Shollenberger",
			email: "brett.shollenberger@gmail.com",
			favorite_show: "Breaking Bad"
		}
		
* A way to assemble and disassemble representations of the database-stored values. In object-oriented programming, we create objects with robust features for the user to interact with, but we don't need to store all of that information in the database. It's much more efficient to break down the objects into text-based representations of that data, a process known as serialization. Relational databases use tools called Object-Relational Mappers to do this work (map objects to relational tables), while Document-oriented databases use Object-Document Mappers to do the same thing (map objects to documents). Mongoose is a MongoDB Document-Relational Mapper that many programmers use in the MEAN stack, and it's the one we'll describe here in detail. 
		
* A server: A server is just a computer running our application that can respond to requests across a network (like the Internet). You request the page containing your Facebook profile, and the server asks the database for the right information, assembles it into the object representing your profile, and returns to you a representation that you see (the Facebook website). MEAN uses Node.js and its library Express to create servers. Node is an implementation of the Javascript language that can run on the server (or just your computer), which is essential for creating an application that can talk to a database. Most traditional implementations of Javascript have occurred in browsers like Chrome or Safari, but these implementations aren't capable of running server-side applications. Express is a library the simplifies creation of servers, and offers us some robust tools to work with.

* A client-side view: The site you end up seeing is the "view" layer of an MVC stack--it's one of many representations of the data you want to interact with, and the one the developers have chosen to create with their data, though there can be others that use the site's API--its "Application Programming Interface," or the means of retrieving, creating, updating, and deleting data. The MEAN stack uses Angular.js to create views; Angular is a browser-based implementation of Javascript that extends the functionality of HTML, allowing developers to create and describe new, reusable features beyond the scope of traditional HTML like navbars, image galleries, and sliders. Angular also provides a structured, opinionated means of writing AJAX-styled requests, which allow individual pieces of a page to update without requiring a full browser refresh, improving the perceived speed of an application and allowing web applications to become more like native desktop applications. AJAX stands for Asynchronous Javascript and XML (Asynchronous meaning the page can be updated in pieces rather than as a whole), and AJAX has traditionally represented a very messy set of requests that quickly grows unmanageable as an application scales. Angular's tools aim to simplify and standardize asynchronous page requests, and make working on the front-end a whole lot easier. 

## Building with the MEAN Stack

Using a slew of tools as opposed to a cohesive framework means developers have to truly understand the way different parts of an application work together, so let's build a useful MEAN application that delves into the details of each.

In this example, we'll build a fairly simple page with a content-management system that allows the user to live preview the page elements they're creating, which is one of the ways Angular will really shine with its asynchronous scripting. 

#### Setting Up the API

An API is a window for interacting with the database. As in Excel tables, we'll want to be able to create new rows of information, update our information, see information we've stored in the past, and delete information we no longer need. Excel tables are a fairly good analogy for relational databases, but a little off when we're talking about NoSQL databases, since each of our "rows" (documents) won't need to contain an entry for each "column" (property) in our "spreadsheet" (collection), but total newbies should appreciate that some "cells" will not contain _no_ value (as they would in a relational database or Excel sheet), they'll just simply not exist. 

The API will be a standardized, RESTful way of working with our database. This means that we'll set up commonly-recognized URLs for doing our work--if a user wants to view all pages in our collection, they'll visit `/api/pages`; if they want to view a specific page, they'll visit `/api/pages/1`. We'll see more of these "standard" routes as we go along. Those that have used Ruby on Rails before will recognize these standard routes from running `rake routes` in the command line, but this background knowledge isn't essential for getting to work. 

#### Starting a Server

The first piece in our API will be the server--the means our API will have of responding to URL requests appropriately. To begin, we'll require the library Express, which is used for creating server objects. We create a server object by calling the `express()` function, and we'll assign this function to an object called `app`. `app` will also be the "interface" other parts of our application use to communicate with the API server, so we write the line `module.exports` and assign it to our `app` object. Don't worry too much about the details here--a module is a cohesive set of code, in this case a server, and `exports` simply says these are the parts of the server that other objects in our application are allowed to "talk to." Anything not included in the `exports` statement will be `private`--only accessible to the code within the module. 

	// server/lib/api/index.js
	var express   = require('express');
	var app       = module.exports = express();
	
Next we'll create a database object called `db`. `db` will be the `interface` of our database schema, representing the publicly accessible aspects of the database module. We'll get to writing the database module in a moment so you can see how it works.

	// server/lib/api/index.js
	var db    = require('./../mongoSchema');
	
Finally, we'll require the routing module that will route incoming requests to the proper database actions--creating, updating, reading, or destroying data. Again, we'll get to writing this module momentarily. 
	
	require('../page')(app,db);

#### Writing the Database Schema

In our database schema, we need to describe the data that we want to persist in our MongoDB collection (similar to a table for our relational database fans out there). For our Excel-minded people, the "schema" is similar to the columns in a spreadsheet--these are the "types" of data that an individual page might have on them--a title, body content, tags, etc. 

Within the schema we'll also create a Model object--an object that not only has the properties of a page in our page schema, but that also includes methods for interacting with each document in the database, allowing us to add new documents, read documents, update documents, or destroy documents. 

We can describe our schema using Mongoose, our Object-Document Mapper, so let's start by requiring that library:
	
	// server/lib/mongoSchema.js
	var mongoose = require('mongoose')
	
Next we'll define our configuration options, which are fairly standard in the Mongoose documentation:
	
	// server/lib/mongoSchema.js
	var config = {
	    user: process.env.MONGODB_USER,
	    pass: process.env.MONGODB_PASS,
	    host: process.env.MONGODB_HOST || 'localhost',
	    port: process.env.MONGODB_PORT || '3000',
	    db: process.env.MONGODB_DB || 'my_great_database'
	  }
	  
Finally, we'll connect to the database, which is also boilerplate code:

	// server/lib/mongoSchema.js
	if (process.env.MONGOLAB_URI) {
	  mongoose.connect(process.env.MONGOLAB_URI);
	} else {
	
	  if (config.user && config.pass) {
	    user = config.user + ':' + config.pass + '@';
	  }
	  mongoose.connect('mongodb://' + user + config.host + ':' + config.port + '/' + config.db);
	}
	
Now we'll get to the good stuff. We'll create a schema object, describing the types of data to store on our collection; an object-document mapper API, to store the final API that other objects in our program will use to interact with the database; and a genericAPI that will add methods to each Model object, allowing the Model to interact with the database.

To start, the schema object and odmAPI object will be empty:

	// server/lib/mongoSchema.js
	var schema = {}, odmAPI = {};
	
And the genericAPI function will be a factory function that will take each model on the schema and transform it into a fully-functioning Model object:

	// server/lib/mongoSchema.js
	genericAPI = function(entity) {
	    var Model = mongoose.model(entity, schema[entity]);
	
	    return {
	      create: function(data, cb) {
	        data.id = new mongoose.Types.ObjectId();
	        object = new Model(data);
	
	        object.save(function(err) {
	          cb(err, object);
	        });
	      },
	      findAll: function(cb) {
	        Model.find(function(err, results) {
	          cb(err, results);
	        });
	      },
	      find: function(attributes, cb) {
	        Model.find(attributes, function(err, results) {
	          cb(err, results);
	        });
	      },
	      update: function(id, attributes, cb) {
	        delete attributes._id; //cant update id 
	        //Maybe later can be change so _id is not sent
	        Model.findOneAndUpdate({
	          id: new mongoose.Types.ObjectId(id)
	        }, attributes, function(err, doc) {
	          if(cb){
	            cb(err, doc);
	          }
	        });
	      },
	      remove: function(id, cb) {
	        Model.findOneAndRemove({
	          id: new mongoose.Types.ObjectId(id)
	        }, function(err, doc) {
	          cb(err, doc);
	        });
	      }
	    };
	  };
	  
Now let's describe our schema for each page:

	// server/lib/mongoSchema.js
	schema.page = new mongoose.Schema({
	  id: mongoose.Schema.Types.ObjectId,
	  title: {
	    type: String,
	    required: true
	  },
	  content: {
	    type: String,
	    required: true
	  },
	  publish: {
	    type: Boolean,
	    default: false
	  }
	});
	
Now we'll use our genericAPI factory function to create a Model object out of our schema. You'll notice this function is nice and generic, allowing us to easily add a new schema that will get mapped to a Model object as well if we see fit in the future:

	// server/lib/mongoSchema.js
	for (entity in schema) {
	  odmApi[entity] = genericAPI(entity);
	}
	
So at the end of this code, our `odmAPI` object contains a model for our `Page` schema, along with any other Model objects we happen to create in the future. The last thing we need to do is export our odmAPI so other objects in our application can work with it:

	// server/lib/mongoSchema.js
	module.exports = odmApi;
	
#### Routing RESTful Actions

Finally, it's time for us to use Express to handle the heavy-lifting of the server: responding to incoming URL requests with the appropriate Model functions to interact with our database. This section is where you'll get to see the rest of our RESTful API actions in play. 

To respond to requests, we'll set up a router, which will parse the incoming requests and perform the appropriate Model actions.

##### The Index Action

We start by exporting the module with an index function. The index function typically returns every instance of a given resource--each page in our collection. We might use this action for creating links to each page using the page title:

	// server/lib/api/pages.js
	module.exports = function(app, db) {

	  //*****************************// Index //**********************************//
	  app.get('/api/features', function(req, res) {
	    var cb = function(error, features) {
	      if (!error) {
	        res.json(features); // Write the jsonified features to the response object
	      } else {
	        res.json(error);
	      }
	    };
	    features = db.feature.findAll(cb);
	  });
	  
Let's break this route down: `app` refers to the server object we created in `index.js`, which is passed into the function at the top, while `db` refers to the database object we created in `index.js` and populated with the Page model in `mongoSchema.js`. 

One of the functions that comes built-in to Express is `get`, which refers to the HTTP method GET. The idea behind REST is one of mapping HTTP requests to CRUD actions (database actions--CRUD stands for Create, Retrieve, Update, and Destroy), and GET maps to Retrieve--meaning this action is intended to Retrieve pages from our database. In this case, we want to GET all the pages, so we route requests to `api/features` to the `findAll()` action we established with our `genericAPI`. `findAll()` naturally retrieves and returns all the documents in our collection.

We can already test our `index` action by either visiting /api/pages/ in a web browser, or by making a cURL request in the terminal. 

	$ curl http://localhost:3000/api/pages
	
	[]
	
We'll notice that currently an empty array is returned because we haven't added any pages to our collection yet, but if we had added some pages, we would see something more like;

	[
	  {
	    "__v": 0,
	    "_id": "5228e5612f271c0000000002",
	    "title": "The Best Page Ever",
	    "content": "A very good page.",
	    "id": "5228e5612f271c0000000001",
	    "publish": false
	  }
	]
	
##### The Create Action

So we probably want to get our collection populated with some data, and there's no better way to do it than with a Create action. Create is the RESTful API action that maps to POST HTTP requests and Create CRUD actions (you following me there cameraman?). Let's get down to it:

	  // server/lib/api/pages.js
	  //*****************************// Create //*********************************//
	  app.post('/api/pages', function(req, res) {
	    var data = req.body;
	
	    db.page.create({
	      title: data.title,
	      content: data.content,
	      publish: data.publish
	    }, function(error, feature) {
	      if (!error) {
	        res.json(feature);
	      } else {
	        res.json(error);
	      }
	    });
	  });
	  
Here we'll use Express's `post` function to map post requests to the same URL (`api/pages`) to the `create` action we set up on our `genericAPI`. This time it will be easiest for us to test our API via cURL, since we'll need to POST a JSON object to the URL, and we don't want to go through the hassle of setting up a form just for this test. 

	$ curl POST -H "Content-Type: application/json" -d '{"title":"A Great Page","content":"One of the best"}' http://localhost:3000/api/pages
	
Now our browser will return the JSON object if we navigate to `/api/pages`, and our previous cURL request will also function as we thought it would. 

#### The Show, Update, and Destroy Methods

Our final three RESTful actions will be Show (display a single page), Update (update the data on a page), and Destroy (delete a page from our collection). These will use GET, PUT, and DELETE HTTP functions respectively. Let's take a look:

	  //******************************// Show //**********************************//
	  app.get('/api/pages/:id', function(req, res) {
	    db.page.find({
	        id: req.params.id
	      },
	      function(error, feature) {
	        if (!error) {
	          if (feature.length) {
	            res.json(feature[0]);
	          } else {
	            res.json({
	              message: 'Object Not Found'
	            });
	          }
	        } else {
	          res.json(error);
	        }
	      });
	  });
	
	  //*****************************// Update //*********************************//
	  app.put('/api/pages/:id', function(req, res) {
	    var data = req.body;
	    db.page.update(req.params.id, req.body, function(error, feature) {
	      if (error) {
	        return res.send(error);
	      } else {
	        res.json(feature);
	      }
	    });
	  });
	
	  //*****************************// Destroy //********************************//
	  app.del('/api/pages/:id', function (req, res){
	    return db.page.remove(req.params.id, function (error) {
	        if (!error) {
	          res.send('');
	        } else {
	          res.send(error);
	        }
	      });
	    });
	
	};

Again, we can test these best via cURL, which we can see via the following cURL commands:

	// Show
	$ curl http://locaolhost:3000/api/pages/0

	// Destroy
	$ curl -X DELETE http://localhost:3000/api/pages/0
	
	// Update
	curl -X PUT -H "Content-Type: application/json" -d '{"title":"A New Wonderful Title"}' http://localhost:3000/api/pages/objectid
	
#### Service

	angular
	  .module('app')
	  .factory('featureService', [
	    '$resource',
	    function($resource) {
	      return $resource('/api/features/:id',
	        { id: '@id' },
	        {
	          update: {
	            method: 'PUT'
	          }
	        }
	      );
	    }]);



