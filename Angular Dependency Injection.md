# Angular Dependency Injection

Angular's renowned for its dependency injection system; in part because dependency injection is a good design pattern, and in part because Angular removes a lot of DI boilerplate for resolving dependencies through factories in the vein of Java's Google Guice. 

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
	
