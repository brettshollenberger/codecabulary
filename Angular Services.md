# Angular Services

To create modular, reusable code in Angular, we create services--singleton objects that can be injected into any part of the app and that are singularly responsible. The Angular service paradigm can be responsible for a lot; a service could encapsulate a constructor or factory function, whose job is to instantiate other objects, as in the `$resource` service, which creates model instances with basic functionality for communicating with a RESTful API. A service could be a simple method, like the `$http` service used to generate HTTP requests across the application. Whatever type of reusable code the service represents, we'll want to have some way of allowing all the parts of our application to access it. 

Angular is known for its awesome dependency-injection architecture, which takes our services and makes them available throughout the application. If we need to make an HTTP request in a part of our application, we can simply declare a dependency on it, and Angular will wire it up for us. 

Since Angular can only wire up dependencies that it knows about, we have to first register our service. There are three ways of doing so, however each way must return a singleton object that represents our service to the rest of our application. The returned singleton is the API, and whatever we want our service to do, our singleton should be capable of doing. Is it itself a constructor or factory function or method or a singleton object with a collection of methods? It doesn't much matter, but when we register our service we must return the canonical version of our code to the rest of the application. Each of these three ways will return a singleton object created by one of the "object-creation recipes" provided by Angular.

We use the `$provide` service to register providers with the `$injector` service. A provider is an object with a `$get` method, which is used to create new instances of _the service itself_ (though the service itself will be a singleton in any given app). If, for instance, we were creating the `$resource` service, the `$get` method would itself be a factory function--making the `$resource` service's `$get` method a factory factory--it creates instances of a service that creates instances of Model objects. If all of this sounds a bit confusing, that's kind of because it is--but you can think of a service as a class and its `$get` method as its `new` method--what the object calls when it wants to instantiate new objects. 

#### Using Provide#provider

The most basic example of using `$provide` is via its `provider` method. The provider method returns an object that features a `$get` function, which returns whatever we want our service to be. If we want our service to be a constructor, we could write something like this:

	myModule.provider('myGreatService', function() {
		return {
			$get: function() {
				return function(name) {
					this.name = name;
				}
			}
		}
	});

If we just wanted a simple method, we could return something like:

	myModule.provider('myGreatService', function() {
		return {
			$get: function() {
				return function(request) {
					do processing
					return request;
				}
			}
		}
	});
	
#### Using Provide#factory

We'll notice in the previous examples that we only include a `$get` function, so why should we have to include all that boilerplate? Thanks to the `factory` method, we don't. We can just return whatever we would have returned as the `$get` method:

	myModule.factory('myGreatFactoryService', function() {
		return function(name) {
			this.name = name;
		}
	});
	
This does the same work as above, and still allows us to keep "private" methods outside of the returned function. 

Sitting atop all is Angular's built-in `$provide` service, which registers recipes and then passes them to the `$injector` service to provide instances to our application. 

#### Register a Constructor Function with `service`

The `service` method allows us to use a constructor function to create our singleton. As a constructor, any properties set on the newly instantiated object will be publicly available as part of our API:

	myMod.service('commentSystem', function(commentModel) {
		this.commentModel = commentModel;
	});
	
Here our comment system service depends on a commentModel, which is passed to the constructor function as an argument and exposed directly on the system service. This decouples our code to make it possible to pass any commentModel service to our commentSystem.

#### Register a Factory Function with `factory`

The `factory` method on Angular modules is a more flexible approach to registering services, since we can register any object-creating function. We can therefore create "private" variables that we don't expose, since the returned object is all that will be visible in a factory function.

	myMod.factory('commentSystem', function(commentModel, PER_PAGE) {
		var PER_PAGE = PER_PAGE;
		return {
			commentModel: commentModel,
			comments: commentModel.find().limit(PER_PAGE);
		}
	});
	
Now only the commentModel and comments attributes will be available via our service, but the private constant PER_PAGE can be used by the returned object to limit the number of comments returned in the comments property. The new lexical scope created by the factory function enables us to create unexposed, private variables.

WE can then also supply configuration variables via constants that allow users of our service to use it as they deem fit:
	
	myMod.constant("PER_PAGE", 10);
	
One issue here is that our users must provide a constant in order for our service to function correctly. 

#### Register A Provider that Returns a Factory Function

The most flexible approach to Angular service creation is via the `provider` method, which allows us to set configuration defaults without obligating us to:

	myMod.provider('commentSystem', function() {
		var config = {
			perPage: 10
		};
		
		return {
			setPerPage: function(PER_PAGE) {
				config.perPage = PER_PAGE || config.perPage;
			},
			$get: function(commentModel) {
				return {
					commentModel: commentModel
				}
			}
		}
	}
	
Here we must return an object featuring a `$get` function, which is itself a factory function that will expose the service's API. This also lets us expose configuration options that are injectable during the configuration phase that won't be injectable during the run phase of our service.

#### Module Lifecycle
	
The module lifecycle consists of two phases, configuration and run. During the configuration phase, we can set configuration options, and during the run phase, we can execute post-instantiation logic on our service singletons.

During the configuration phase, we can configure our providers by including them as a dependency:

	myMod.config(function(commentSystemProvider) {
		commentSystemProvider.setPerPage(10);
	});
	
We add the provider suffix here to pass this dependency and allow for optional configuration options of on our service factory function.

During the run phase, we can register any work that needs to occur when our app is bootstrapped, for instance, setting variables that are required across many other templates:
	
	angular.module('metaInfo', []).run(function($rootScope) {
		$rootScope.title = "My Great Site";
	});
	
By separating our application into these various modules, our application becomes a collection of configure and run blocks--truly a collection of collaborating objects as opposed to a single entry point as is standard in many other languages.

Thus we can group related services into one module and create potentially reusable service libraries, which any app can depend on in a classic example of service-oriented architecture.

# Service Visibility Across Modules

So we've grouped our services into modules, now how can our other modules access them? Perhaps surprisingly, they can with ease, since Angular groups them all into the global namespace when it bootstraps the application.

Unsurprisingly, services defined in a module that is injected into a parent module are visible to the parent's children:

	angular.module('comments', [])
		.factory('CommentModel', function() {
			return {};
		}
	});
	
	angular.module('app', ['comments'])
		.factory('PostModel', function(CommentModel) {
			return {};
		}
	});
	
Perhaps more surprisingly, sibling modules can access one another's services:

	angular.module('comments', [])
		.factory('CommentModel', function() { return {} });
	
	angular.module('posts', [])
		.factory('PostModel', function(CommentModel) { return {} });
		
Without declaring a dependency on the comment module in its sibling (posts), the PostModel service is still able to declare a dependency on a service within the comments module without worrying about namespacing.

#### No Namespaces

Since Angular modules don't provide namespace resolution there can only be one service by any given name per app, regardless of module. But, importantly, modules can still help us to perform unit testing, and keep our code organized, so they're an essential structure for writing sound Angular code. 