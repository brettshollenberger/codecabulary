# Node.js Modules

Users of node's libraries assign the libraries to variables, and call functions on the public interface on those variables, like so:

	var http = require('http');
	
	http.createServer(function(request, response) {
			...
	});
	
As creators of modules in node, we also need to expose our public API in some way. Here are a few of the most common:

#### Expose a Single Function

One option, if you have a single function as the public interface, is to attach `module.exports` to the function itself:

	module.exports = function() {
		public function...
	};
	
Now when we require the function, we use it directly:

	var imported_function = require('./my_module');
	
	imported_function();
	
#### Expose a Singleton-Type Object

We could also place a number of publicly accessible functions on the module.exports object itself, so that the module acts as a singleton:

	module.exports.coolFunction = function() {
		...
	};

	var imported_singleton = require('./my_module');
	
	imported_singleton.coolFunction();
	
#### Expose a Psuedo-class

If the module is a class, you can either expose the constructor directly, or expose a function to allow the class to be instantiated:

	var CoolClass = function() {
		...
	};
	
	CoolClass.prototype.coolFunction = function() {
		...
	};
	
	module.exports.create = function() {
		return new CoolClass;
	};
	
Now the module would be used via the `create` method:

	var instance1 = imported_class.create();
	
Although this use case prevents users from silly errors like forgetting the `new` keyword when using the constructor, it violates the Open/Closed Principle. We ought to make our class open to extension.

Being that that's the case, we can simply expose the class itself:

	module.exports = CoolClass;
	
	$ var instance1 = new imported_class();
	
And this final method tends to be the right choice for advanced users of our module, since it enables the most flexibility, including the ability to extend our module.

	
	