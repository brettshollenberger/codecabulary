# Angular Dependency Injection

> 140 character version: Angular's DI system does not fully employ the DI design pattern. We can inject mocks during testing & fill in the gaps w/ the factory pattern.

One of Angular's most discussed features is its dependency injection system, which is really a dependency lookup system rather than an implementation of the dependency injection design pattern. The design pattern is an implementation of the strategy pattern applied to a function's signature--it allows developers to create flexible, duck-typed interfaces that will accept any dependency with an interface that fulfills the contract invoked by the object that uses it. If an emailing service requires an object that responds to an email method to provide an email address, the service doesn't care if it receives a user object from our system or a user from a Facebook API call--it just needs an object that maintains the contract declared between the two. 

In a way, Angular provides this functionality by allowing us to swap out dependencies during the configuration phase. This flexibility allows us to create mock dependencies during testing and proves valuable for actually isolating individual objects during unit testing. However, Angular services are singletons--created once from a pre-configured method call--and we cannot create multiple instances with slightly different dependencies to suit our needs. Instead, we rely on our services to implement a factory pattern to achieve a similar type of functionality that we'd use dependency injection for in other languages. 

#### Using and Abusing Angular's DI System

Angular's dependency injection system removes a lot of boilerplate code required to lookup dependencies by registering those dependencies through a "provider" syntax akin to Google Guice for Java, and it helps Angular achieve modularity; it's easy to let modules register explicit dependencies on one another because you don't have to specify file paths as long as modules are registered with the system. 

A typical dependency lookup system splits its API into two categories: '

1) A provider syntax - A syntax that declares what will be injected when an object asks for an object or function registered with the DI system (`$provide`)

2) An injection syntax - A syntax that declares which objects or functions a module, class, or function relies on to be injected (`$inject`)

In a language like Java, these syntaxes are declared using annotations. Javascript does not implement annotations, so the Angular team uses two of its own internal services to provide the same functionality--`$inject` and `$provide`. 

#### Provider Syntax

The provider syntax allows us to declare what will be passed to an Angular object when that object declares a dependency on our service. There are four methods provided by Angular to declare our services, but three of them are actually convenience methods that shortcut the other method--`$provide.provider`:

1) Provider is the father of all the other methods. It provides the most flexibility when declaring our services, but also requires us to write the most code. It allows us to create private methods, methods that can be used to configure our module, and a `$get` method that describes what will be injected into functions and classes that rely on our service. To use the `provider` method, we declare a constructor as the last argument. Any publicly exposed methods that are not the `$get` method can be used to configure our service; the `$get` method describes what to return when our service is evaluated and invoked as a dependency:

	angular.module('myModule', [])
		.provider('URLHelper', function() {
			var config = {
				links: []
			}
			
			this.setLinks: function(links) {
				config.links = links;
			},
			this.$get: ['NavLink', function(NavLink) {
				_.each(config.links, function(link) { return new NavLink(link); });
				return config.links;
			}
		});

In the example above, we describe a function `setLinks` that allows us to configure the links for our application. We might configure the service in our `app.js` like so:

	var links = {};

	links.home = {
	      href: '/#/dashboard',
	      text: 'Home',
	      subcategories: {}
	};

	links.security = {
	      href: '/#/security/events',
	      text: 'Security',
	      subcategories: {
	        events:  { href: '/#/security/events', text: 'Events', parent: 'Security' },
	        sensors: { href: '/#/security/sensors', text: 'Sensors', parent: 'Security' },
	        devices: { href: '/#/security/devices', text: 'Devices', parent: 'Security' }
	      }
	};

    	URLHelper.setLinks(links);
    	
2) `factory` describes a provider that only consists of a `$get` function. We use it when our module requires no configuration. 
    	
When a module declares a dependency on our URLHelper, it will receive these configured links via the `$get` function.

#### General Overview of Dependency Injection

Dependency injection, regardless of language, is a good design pattern to keep code loosely coupled. A class that relies on other classes to do part of its work is tightly coupled to a single particular implementation. Let's say we were building a submit button in Angular, and we knew right now, today, that we wanted the send button to also send the user an email follow up thanking them for signing up for our page:

	function EmailSender() {
		return {
			send: function() {
				...
			}
		}
	}

	angular.module('app.directives').directive('submitButton', function() {
		EmailSender.send(data);
	});
	
Now our directive is totally reliant on EmailSender. If tomorrow we need a new contact strategy, supplied by the user on the form, like say, an SMS-based thank-you, we have to start relying on multiple modules and type-checking our thank yous. Not a grand idea. 

The traditional factory solution to this problem adds an additional layer of abstraction between our module and its dependencies:

	function EmailSender() {
		return {
			send: function() {
				...
			}
		}
	}
	
	function TextSender() {
		return {
			send: function() {
				...
			}
		}
	}
		
	function ContactClient(type) {
		types: {
			email: new EmailSender(),
			text: new TextSender()
		}
		
		return {
			$get: function(type) {
				return types[type];
			}
		}
	};
	
	angular.module('app.directives').directive('submitButton', ['$scope', function($scope) {
		ContactClient.$get($scope.form.contactPreference).send(data);
	}]);
	
The factory pattern is a bit better, but still ends up hardcoding in its dependencies (ContactClient will end up relying on EmailSender, TextSender, and whatever new strategies we come up with next week). It's a one-step-removed version of the same problem. Dependency injection aims to solve that. 

In many languages, dependency-injection is an implementation of the strategy pattern--we change the implementation of the SubmitButton in a given context by passing in the function we want to act as the means of contact--whether email, text, or otherwise. Here's an implementation in plain Javascript of such a constructor:

	function SubmitButton(contactMethod) {
		this.submit = contactMethod(data);
	}
	
	SubmitButton(emailSender.send(data));
	
	SubmitButton(textSender.sendSMS(data));
	
Here, our implementation isn't even concerned that textSender and emailSender implement two different interfaces. As long as we pass a function, we can instantiate as many different types of submit buttons as we want, each using different strategies that we choose depending on the occasion.

#### Angular's Dependency Injection System

Angular's dependency injection system is a bit different from these strategy pattern-driven implementations. Angular's components--services, directives, controllers--are configured and declared during the application bootstrapping. We can't vary them by changing the dependencies we pass in, which makes Angular's dependency injection system more like a dependency lookup or dependency resolution system. In our application code, we _will_ need to use the factory pattern in order to change the different objects we return from our services; no dice on switching those up with dependency injection.

Where Angular's really shines though is that it _can_ change up the dependencies we pass in when it's bootstrapping. Since unit testing will require us to manually bootstrap the application, we can mock the dependencies we wish to pass in. This allows us to reduce the mocked dependencies to only the contract used by the object--providing better documentation on the object being tested, and what it's really relying on. If we want to pop the module into a different context, that relies on a different object to fulfill one of those dependencies, we can mock out the new object and ensure it works without even transplanting the module into the new application. 

#### How to Use Angular's Dependency Injection System

The system is composed of two interfaces: the `$injector` and the `$provider`. 

In other dependency injection architecture implementations, the language itself supports the use of annotations (as in Java with Google Guice). Annotations provide metadata about classes or objects in a predictable manner. Google Guice defines `@Provides` as an annotation on constructor functions that allows the rest of the application to retrieve that constructor when attempting to lookup a class by the annotated name. 

Javascript does not provide annotations in the language, so the Angular team made up a few of their own. These follow the paradigm of `$injector` and `$provider`, as in Google Guice:

* Injection annotation is the dependency injection notation. It defines which other services a new service, controller, or directive depends upon. 
* Provision annotation is the API. It defines how to instantiate the service should other services choose to use it. It also registers the service with the injector, so that it can be looked up. 

#### Injection Annotation

Angular provides several means of describing a service, directive, or controller's dependencies. The most common, and best practice version, is called inline annotation:

	myModule.controller('myController', ['myServiceName', function(serviceA) {});
	
Notice in the version of the annotation above, we're able to lookup the service by its name (`myServiceName`) and rename it within the context of the controller. This makes it really easy to swap out dependencies that share a common interface, because we only need to change the name of the new dependency in one location. Another reason this notation is the preferred notation is because it protects against minification. Many minifiers will reduce function arguments to single characters (e.g. myServiceName -> a); another form of injection annotation only provides the service names as function arguments, which fails as soon as the code is minified. 

The other method of injection annotation that defends against minification is to call the `$inject` service directly, and the primary reason developers don't use it is because it's a bit more verbose than the inline annotation:

	myController.$inject(['$scope']);
	
	function myController() {};
	
	myModule.controller('myController', myController($scope));
	
