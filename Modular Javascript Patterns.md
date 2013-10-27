# Modular Javascript Patterns

Polluting the global namespace is a well-known Javascript anti-pattern, but it would be a well-known anti-pattern in other languages too if those languages didn't also provide mechanisms for creating modular code. The reason seasoned Javascript developers have to discuss the global namespace at all is because younger developers have to learn patterns to simulate namespaces in Javascript. Let's take a look at a few:

#### Namespace Pattern

In the namespace pattern, we avoid naming collisions by attaching our properties to a single global object:

	app = {};
	app.models = {};
	
Etc. We can create a general, non-destructive namespacing function to ensure modules we intend to use first exist, or create them as empty objects if they do not:

	MYAPP.namespace = function(namespace_name) { 
		var parts   = namespace_name.split('.');
		var parent = MYAPP;
		var i;
		
		if (parts[0] === 'MYAPP') parts = parts.slice(1);
		
		for (i = 0; i < parts.length; i++) {
			if (typeof parent[parts[i]] === 'undefined') parent[parts[i]] = {};
			parent = parent[parts[i]];
		}
		
		return parent;
	}

#### Access Control

Although Javascript doesn't provide the concept of "private" variables or methods, we can mock privacy and privilege through closures. Though closures sound scary to developers with formal, object-oriented backgrounds, the concept is simple enough:

* All Javascript functions are closures
* Each function creates its own little scope, where its local variables exist
* Outside of the function, other objects do not have access to these local variables
* Only methods exposed on a returned object are public (as in a constructor function)
* We expose public instance methods and properties with the `this` keyword in a constructor
* Javascript functions maintain access to their own private variables, even if they are invoked in other contexts
* Javascript variables also maintain reference to the scope in which they were defined

These last two are important, and you should play around with the concepts in a browser or node terminal to get the hang of it. The local variables defined in a function travel with the function itself on its journey throughout the world--that seems to make sense. But the scope in which the function was defined? Let's look at an example:

	var scopeA = {};
	var scopeB = {};
	
	scopeA.one = 1;
	scopeB.one = 2;
	
	scopeA.returnOne = function() { return this.one; }
	scopeB.returnOne = function() { return scopeA.returnOne(); }
	
	scopeB.returnOne();
	>> 1
	
Although we might expect returnOne when invoked in scopeB to return its local variable one, since it scopeA.returnOne evaluates to `this.one`, `returnOne` maintains the context it was defined in. 

All this fancy closure talk is just to say that we're able to define private variables within function constructors that only the other methods defined within the constructors will have access to. In this way we can mock variable privacy. 

#### The Module Pattern

The module pattern uses namespacing, immediately invoked function expressions, and private methods through closures to mock the concept of modularization. The only new idea here is the IIFE, a function that executes immediately, and then never again. In this case, we use it to create a new module object with private variables if necessary.

	var sayCoolModule = namespace('MYAPP.sayCool');
	
	sayCoolModule = (function() {
		var cool = 'cool';
		return {
			sayCool: function() {
				return 'cool';
			}
		}
	})();
	
The returned module itself is an object. 