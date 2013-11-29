# Angular $Scope

Angular's scopes are probably the least documented and least understood portion of the framework. What information does exist about scopes is confusing if you've got a bit of background in Javascript or MVC patterns--a scope is not an execution context for expressions and it's not a model--it's a scope. If all that sounds confusing (or just straight repetitious), don't worry, I'll explain what I mean along the way:

#### Scopes are Scopes

The most important thing to know about Angular scopes is that they _are_ scopes (there's that repetition again). In computer science, scopes are the sections of our program in which identifiers are valid, and we can use identifiers within that scope to look up the values they refer to. In a Javascript program, I can declare a variable `someNum` and look up its value later:

	var someNum = 1;
	someNum
	>> 1

Simple enough. The scope of someNum is global in this case; I've declared it outside of any function, so it can be accessed from anywhere in the application. 

#### The Scope Chain

In Javascript, only functions create new scopes, and those functions limit the range of the variables declared within them to the function itself, and any children of that function:

	var currentScope = 0;
	(function() {
		var currentScope = 1, one = 'scope one';
		(function() {
			var currentScope = 2, two = 'scope two';
			console.log("I can see ' + one + ' and ' + two);
			console.log("The current scope is ", currentScope);
		})();
		console.log("I can see " + one);
		console.log("But logging the value of two in scope one would throw an error; it is outside of scope one's scope");
		console.log("The current scope is ", currentScope);
	})();
	
	>> I can see scope one and scope two
	>> The current scope is 2
	
	>> I can see scope one
	>> But logging the value of two in scope one would throw an error; it is outside of scope one's scope
	>> The current scope is 1
	
While this example may seem trivial, it's important to understand the scope chain well, because Angular creates scopes in a similar way. Both Javascript and Angular represent scopes as objects, so let's first see how Javascript handles this. All functions create scopes, and each function has a private property called [[`scope`]] that refers to its scope chain, the collection of scopes that it has access to, starting with its own scope, and expanding outwards to the global scope. Let's see what scope 2's scope chain from the example above would look like:

	[
		0: {
			currentScope: 2,
			two: 'scope two'
		},
		1: {
			currentScope: 1,
			one: 'scope one'
		},
		2: {
			currentScope: 0
		}
	]
	
We see here the scopes represented as objects (as they are in Angular); they store key-value pairs representing the variables available within their scope. Identifier resolution works its way through each scope, starting at the first scope in the chain (the scope created by the function itself), and working its way outwards. When we log out the currentScope in scope two, it doesn't need to continue looking for the variable in scope one, because it finds it in its current scope, which is why it resolves to `2` instead of `1` or `0`. 

#### Angular's Scope Objects Almost the Same...

Functions in Angular (perhaps obviously) create new scopes, but Angular's `$scope` objects are different from the scopes created by functions. For the rest of this article `scope` will refer to a scope created by a Javascript function, while `$scope` will refer to an Angular `$scope` object. `$scopes` are visualized as objects, just the same as scopes within vanilla JS scope chains, but they have a few notable differences:

1) Properties exposed on `$scope` objects are available in Angular templates
2) Not every variable declared in a Javascript function is added to the `$scope`; we explicitly set properties on the `$scope` object, thus defining a public interface for our `template objects` (directives and controllers, or objects that are meant to expose data to, or manipulate a template)
3) `$scopes` are not part of a `scope chain` as in vanilla Javascript. Instead, they are built via a scope constructor, and inherit prototypically. Prototypal resolution is almost identical to scope chain resolution, and for practical purposes, you won't notice a difference, but it may interest some to see how Angular creates scopes. 
4) `$scope`s have the ability to watch the properties defined on them for changes. Angular is able to dirty check all scopes to see if the current value of properties defined on them is the same as previous values defined on them. When scope properties change, Angular uses this watching mechanism to alert all instances of the same object to update on the page. 
5) Angular gives us the ability to explicitly define when new scopes will be created, and whether or not those scopes will inherit prototypically from their parent scopes.

#### Available to Templates 

Angular extends all properties defined on `$scope` objects to our templates--though the scope of those properties is inherently limited to the location where the `$scope` object exists. Many of Angular's built-in directives instantiate new `$scope` objects, such as `ng-controller`. This means that our templates are able to define scopes, so they can get access to the data they require; let's take a look:

	<div ng-controller="MainCtrl">
		{{currentUser.name}}
	</div>

The `div` above uses `ng-controller` to declare that it requires access to the data defined on the scope of the controller named `MainCtrl`. It explicitly expects MainCtrl to define a `currentUser` property, and for that currentUser to have a `name` property:

	.controller('MainCtrl', function($scope, User) {
		$scope.currentUser = User.getCurrent();
	});
	
This example expect us to have defined a User service capable of retrieving the current user. By attaching the currentUser to the scope as a public property, our template can now access it. 

#### Not All Properties Defined in a Scope are Available on A $scope

Scopes created by functions are different from the `$scope` objects created by Angular:

	.controller('MainCtrl', function($scope, User) {
		var user = User.getCurrent();
		$scope.currentUser = user;
	});
	
In the example above, the `user` property is not available to our templates, though it is a property of the `scope` created by the controller's constructor function. This property would be a part of the constructor's scope chain, but only the `currentUser` property is exposed publicly on the `$scope` object, and to our template. While I doubt this difference will trip any seasoned Angular dev up, it's worth noting for experienced Javascript devs that are new to Angular.

#### Scopes Inherit Prototypically

Nested functions in Javascript create nested scopes as we saw at the beginning of this article. Nested `$scopes` are created by nesting our DOM structure, which nests the functions our directives refer to inside one another as well:

	<div ng-controller="MainCtrl">
		<div current-user-widget user>{{currentUser}}</div>
	</div> 
	
In the example above, we nest a `current-user-widget` directive inside of our `MainCtrl`. Assuming the `current-user-widget` defines a new scope that inherits prototypically from its parent scope (though it's possible to break that link as we'll see later), it has access to all properties defined on the `MainCtrl`:

	.controller('MainCtrl', function($scope, User) {
		$scope.currentUser = User.getCurrent();
	})
	.directive('currentUserWidget', function() {
		return {};
	});
	
Our directive itself need not define anything special if it will be grabbing data from its parent scope. In this case, we merely need to return an empty directive definition object, which will follow Angular's defaults, and inherit `currentUser` from `MainCtrl`.

This relationship is known as prototypical inheritance, and it's made possible because Angular's `$scope` objects are plain Javascript objects. When our currentUserWidget is used in this way, it gets its scope by calling `$new()` on the `$scope` from the `MainCtrl`. This makes its `$scope's` prototype object `MainCtrl`'s `$scope`, so when properties are not found on its own `$scope`, lookup proceeds to its prototype `$scope`, and then to `MainCtrl`'s prototype `$scope`, and so on. 

This setup is very similar to creating a nested function scope chain in Javascript, and is meant to mimic such a setup, although the correlation is not one-to-one. For all intents and purposes, however, you can treat the relationship the same. Let's say, for instance, the `currentUserWidget` defined its own `currentUser` property:

	.directive('currentUserWidget', function() {
		return {
			link: function(scope) {
				scope.currentUser = {};
			}
		};
	});
	
In the same way as our functions at the beginning of this article shadowed the `currentScope` variable defined on their parent scopes, these `$scope` objects can shadow properties defined on their parent `$scopes`, and lookup will not proceed to the parent. Shadowing is different from overriding--we're not changing the value of `currentUser` on the parent scope, we're merely defining a new property `currentUser` in this `$scope`. 
	
#### $scopes Can Watch for Changes

When vanilla JS functions are called, they create an additional private property called the `execution context`. This execution context copies the function's private `scope chain` as a property on itself, and then pushes a new variable object to the front of the chain, called the activation object, which contains the value of `this`, the arguments collection, named arguments, and all private variables. These values are consulted first before proceeding up the scope chain. In this way, vanilla JS functions can return different evaluations depending on the context their called in and the arguments they're passed; they're not merely static objects.

When Angular's core team says that `$scopes` are an execution context for expressions, what they're saying may sound confusing, but what they mean might be something close to "Angular `$scopes` represent a constantly evolving `execution context`." When a function is called, its `execution context `is created, and then destroyed, but Angular's functions exist so long as the template is live and presented to the user, meaning that the values contained on the `$scope` are liable to change at any point in time. 

Angular allows us to update the execution context of these functions by `$watch`ing the `$scopes` for changes to their properties, and Angular will update all instances of a given object across `$scopes` for us, which is pretty nifty. Consider a login button:

	.directive('loginButton', function(User) {
		return {
			link: function(scope, element) {
				element.on('click', function() {
					var user = User.find({email: scope.email, password: scope.password});
					if (user) scope.$apply(scope.currentUser = user);
				});
			});
		}
	});
	
When we click this login button, we attempt to find the user specified using data on a form, and update the currentUser to the value found by the User service. If a user is found, we alert Angular's dirty checking mechanisms that the value of scope.currentUser has changed, and to update it anywhere that others are watching for changes. We use `scope.$apply()` to alert Angular to fire its dirty checking mechanisms. 

Basically this is the same as re-firing our directives' functions with a new execution context, allowing the function to return different results than it previously did. But we don't have to call the function again, instead, we merely alert the `$scope` that its properties have changed, and it updates the `execution context` for us, and reevaluates. 

#### Controlling Scope Creation

Angular gives us quite a bit of control over how `$scopes` are created--for instance by allowing us to isolate the scopes from one another. We've seen how `$scopes` can inherit prototypically, but we can also ask Angular not to allow them to do so:

	.directive('current-user-widget', function() {
		return {
			scope: {}
		}
	});
	
By setting scope to an empty object, we avoid Angular creating a new `$scope` that inherits from its parent. This is often useful when we want to explicitly supply data for the directive to work with, so that a directive can only manipulate data we've given it access to:

	.directive('current-user-widget', function() {
		return {
			scope: {
				currentUser: '='
			}
		}
	});
	
The example above sets a `currentUser` property on the `$scope` that is explicitly inherited from its parent--but nothing else. We pass in this property on the template:

	<div current-user-widget current-user="currentUser">{{curentUser.name}}</div>

We can also tell Angular not to create a new scope for our directive at all, which means it will use the same `$scope` as its parent:

	.directive('current-user-widget', function() {
		return {
			scope: false
		}
	});
	
By default scope is set to true, and here we tell Angular not to create a new scope. 

