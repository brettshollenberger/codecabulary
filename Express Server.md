# Express Server

Express is a Node module that 's used for creating RESTful APIs, one of our favorite things as developers. Getting started with Express is easy:

1) We always start with Node modules by installing them. To do so, we add the module as a dependency to our app, as in a `Gemfile` or `bower.json`; with Node, we add these dependencies to a file called `package.json`:
	
	{
		"name": "express-server",
		"description": "a very basic example",
		"version": "0.0.1",
		"dependencies": {
			"express": "3.x"
		}
	}

2) Now we can install our dependencies via Node Package Manager (npm):

	$ npm install
	
3) When using a module in our code, we first require the library:

	var express = require('express');
	
4) Then we can use `express` by calling the `express()` function, which returns an Express server instance. We can tap into that instance for configuration and routing:

	var app = express();
	
5) To add a route to our makeshift router, we can call `METHOD`s on our Express server instance, where `METHOD` is an HTTP method like `GET`, `POST`, `PUT`, or `DELETE`. The first argument we pass to the method is the route to match, and the second is a callback function, which is executed when the route is retrieved. The callback receives a `req` object and a `res` object (request and response), which Express has already added functionality to for us, which you can check out in the Express docs. These methods allow us to return HTTP responses, like so:

	app.get('/', function(req, res) {
		res.type('text/plain');
		res.send('I am body content');
	});
	
6) Finally, we call the `listen` function on our server instance, which tells the server which port to listen to:

	app.listen(process.env.PORT || 3000);
	
7) Now we can call our server using:
	
	node app.js
	
And visit it at localhost:3000

#### Building a RESTful API

Let's get down to the good stuff. Since we're working with Node, we probably want the benefits of working entirely in a Javascript-oriented stack, so we'll likely be returning a JSON object from our database request. Modeling that response, without getting bogged down in the details, let's look at how we'd build a RESTful API:

1) Let's set up some dummy JSON content:

	dwarves = [
		{ name: "Sleepy", state: "Sleepy" },
		{ name: "Sneezy", state: "Sneezy" }, 
		{ name: "Doc", state: "Pretentious" }
	]
	
Here we just have an array of Javascript objects. Fairly straightforward stuff.

2) Next let's create a route to return all dwarves (our `index` action in a traditional RESTful API):

	app.get('/dwarves', function(req, res) {
		res.json(dwarves);
	});
	
3) How about returning just one dwarf? Here we need to tap into the request params object:

	app.get('/dwarf/:id', function(req, res) {
		if (req.params.id > dwarves.length || req.params.id < 0) {
			req.statusCode = 404;
			return req.send("404! Dwarf not found.");
		};
		
		var dwarf = dwarves[req.params.id]
		req.json(dwarf);
	});
	
4) And what about creating a new dwarf? Here we'll need to hook up a POST request, and use express.bodyParse() to, well, parse the body of the request:

	app.use(express.bodyParser());

	app.post('/dwarf', function(req, res) {
		if (!req.body.hasOwnProperty('name') || !req.body.hasOwnProperty('state')) {
			req.statusCode = 400;
			return req.send("400! Invalid JSON!");
		};
		
		dwarf = {
			name: req.body.name
			state: req.body.state
		}
		dwarves.push(dwarf);
		res.json(true);
	}
	
We can then test our URL with a cURL request:

	$ curl POST -H "Content-Type: application/json" -d '{"dwarf":"Bashful","state":"Bashful"}' http://localhost:3000/dwarf
	
And if we visit `/dwarves`, we see that our request successfully added a new dwarf to the list.

From here it's simple enough to extrapolate the rest of our REST API. Happy coding! 
	