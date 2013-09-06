# Building RESTful NoSQL APIs with Express

Today let's build a RESTful interface with MongoDB, Mongoose, Node, and Express. To start, we'll organize our server-side code under folder hierarchy.

#### The Folder Hierarchy

	> server 
		> lib
			> api
				- index.js
				- other_router_files.js
			- mongoSchema.js

#### The Express & Mongo Boilerplate

Let's start with that `index.js`, which will be the hub of our server. Simpler Express and Node walkthroughs exist to get you up to snuff with simpler servers, so let's skim over this code, which is a lot of boilerplate:

	// index.js
	var express   = require('express');
	var app       = module.exports = express();
	var db    = require('./../mongoSchema');
	
	require('./other_router_file1');
	require('./other_router_file2');
	
	...etc...
	
Here we've declared that our app object will be an Express server instance, and that db will be a hash containing our Mongo model objects, which we'll take a look at next. We've also included the other files in our API hierarchy here, and we can use those to map our various collections' routes to (collections are the equivalent of tables in NoSQL databases).

#### The Mongoose Schema and Object Data Mapping API

A lot is going on in this next file, so let's break down the steps quickly first:

1) Require the library mongoose (duh)

2) Configure the Mongoose connection. We'll skip this since it's well-documented and not exactly earth shattering, but I'll show you where we place it in our setup traditionally.

3) Setup a genericAPI factory function that builds and returns an object containing model-specific API functions for the generic RESTful actions. I tend to name these as they are in Rails (create, update, index, edit, new, show, destroy), and my primary recommendation is to name them consistently for your individual team, even if Jerry really likes naming the `index` action `showAll` because he's the project lead and he gets to name it whatever he wants.

4) Create each of your schemas using the generic Mongoose conventions, and add them to an object called "schema." I've provided an example below of a simple `feature` schema for a blog. 

5) Loop through each entity in the schema, and for each create a new object on the odmAPI object that contains the result of running the schema through our genericAPI function.

6) Export the odmAPI so our API can reference it easily. ODM stands for Object Document Mapper, the equivalent of an Object Relational Mapper for Document-Oriented Databases. The Wikipedia article is fairly good on ODM-ing if you need to catch up on some background first.

	// mongoSchema.js
	var mongoose = require('mongoose'),
	
	config = { ... configuration settings ... check out the API for more info };
	
	var schema = {}; odmAPI = {}; entity, 
		genericAPI = function(entity) {
		 var Model = mongoose.model(entity, schema[entity]);
		 
		 return {
		 	create: function(data, cb) {
			 	data.id = new mongoose.Types.ObjectId();
			 	object = new Model(data);
			 	
			 	object.save(function(error) {
			 		cb(error, object);
			 	});
		 	}, ... more RESTful API methods ...
		 };
		 
	schema.feature = new mongoose.Schema({
		id:  mongoose.Schema.Types.ObjectId(),
		title: {
			type: String,
			require: true
		},
		content: {
			type: String
		}
	};
	
	for (entity in schema) {
		odmApi[entity] = genericAPI(entity);
	}
	
	module.exports = odmApi;
	
#### Our Express API

Now we can add files to our API folder for each resource we want to map to. Following up on my previous example, let's wire up a `feature` API:

	// feature.js
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
		
		  //******************************// Show //**********************************//
		  app.get('/api/features/:featureID', function(req, res) {
		    db.feature.find({
		        id: req.params.featureID
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
		
		  //*****************************// Create //*********************************//
		  app.post('/api/features', function(req, res) {
		    var data = req.body;
		
		    db.feature.create({
		      title: data.title,
		      content: data.content,
		      image: data.image, //we save the InkBLob of the image to delete it when the project is deleted
		      InkBlob: (data.InkBlob && typeof data.InkBlob === "object")?  JSON.stringify(data.InkBlob) : data.InkBlob,
		      publish: data.publish
		    }, function(error, feature) {
		      if (!error) {
		        res.json(feature);
		      } else {
		        res.json(error);
		      }
		    });
		  });
		
		};
		
		//*****************************// Destroy //********************************//
		app.del('/api/features/:id', function (req, res){
		  return db.feature.remove(req.params.id, function (error) {
		      if (!error) {
		        console.log("Removed feature");
		        return res.send('');
		      } else {
		        console.log(error);
		      }
		    });
	        });
	        
	        //*****************************// Update //*********************************//
	      app.put('/api/features/:id', function(req, res) {
	        var data = req.body;
	        db.feature.update(req.params.id, req.body, function(error, feature) {
	          if (error) {
	            return res.send(error);
	          } else {
	            res.json(feature);
	          }
	        });
	      });
	        
#### Testing Our REST API with cURL

	// Create
	$ curl POST -H "Content-Type: application/json" -d '{"title":"A Great Feature","content":"One of the best"}' http://localhost:3000/api/features	
	// Destroy
	$ curl -X DELETE http://localhost:3000/api/features/objectid
	
	// Index
	$ curl http://localhost:3000/api/features
	
	// Show
	$ curl http://locaolhost:3000/api/features/objectid
	
	// Update
	curl -X PUT -H "Content-Type: application/json" -d '{"title":"A New Wonderful Title"}' http://localhost:3000/api/features/objectid
	
	
	

