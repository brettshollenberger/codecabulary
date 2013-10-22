# Angular Scope

Angular scope objects define instance methods for Angular's view directives. Angular's views instantiate scope objects, which inherit prototypically from the scope objects that contain them. For example:

	<div ng-app="app">
		<div ng-controller="MainCtrl"></div>
	</div>

The `ng-app` directive injects `rootScope`, and calls `$rootScope.$new()`, returning a new scope object containing any properties and methods that have been declared on the `$rootScope` object anywhere else in the application. `ng-controller` takes the scope variable that was instantiated by `$rootScope.$new` (let's call it `scope1`), and calls `scope1.$new()`, which returns a new `scope2` that inherits prototypically from it. Literally:

	scope2.__proto__ = scope1;
	
Though `scope2` does not define the variables defined on `rootScope`, it has access to them. It also has access to whatever instance variables and methods were declared on the `ng-controller` directive with the name `MainCtrl`. In this way, Angular services (including self-created directives, built-in directives, and controllers) act similarly to "scope classes," (aka classes that instantiate scope objects), _whose prototypes are not known until runtime_.  

In Angular, the best description of scopes offered by the core team is: "an execution context for expressions;" this description fits in with our classical understanding of scopes, but there are some other, more confounding descriptions offered by the core team that deserve clearing up. First, the core team says that the scopes of an application refer to the application model--and what they mean is not the capital M Model in MVC, but the application hierarchy that very closely mimics the DOM. For example, take a look at this HTML snippet:

	<html ng-app="app">
		<body>
			<div ng-controller="MainCtrl">
				{{babysFirstBinding}}
			</div>
		</body>
	</html>
	
In the example above, Angular creates a hierarchy of scopes starting at the `ng-app` directive. `ng-app` creates the `$rootScope`, which is equivalent to the highest level namespace in your application. All other scopes hang off of the `$rootScope`, and inherit any variables defined there. Scopes, like other Javascript objects, inherit prototypically--they can inherit from and override variables defined in the parent context. So when we declare the `ng-controller` directive, it too instantiates a scope object, which inherits from `$rootScope`, and we have a simple hierarchy that is closely akin to our DOM structure. 

Scope objects are bound to and exposed to a view, and upon further inspection, we can see how scopes 

The Angular core team also says that scopes are where we define the business logic of our program, classically a concern of the controller in a strict MVC design architecture, but it's also important to note that we use scopes to define things like DOM manipulation, which should not be a controller concern. In this regard, it's more useful to defer to the definition of a scope as simply an execution context in which variables, objects, and methods exist, and not strictly as a hub for business logic. In this regard, when the core team says that scopes are the glue between a controller and a view (the go-between that allows a view to update a model, for instance), it's also important to understand that a scope can be the glue between a view and a directive's link function, which is meant to handle DOM manipulation; a scope is really the glue between the view and some other part of the application, not just the controller, which should maintain a specialized business-logic role in solid design architecture. 

In this regard, a scope is also the glue between a view and a model, since model object instances will be defined on a controller. For instance, in Ruby on Rails, we define model objects in a separate layer, and instances of these objects are available in our controllers:

	def new
		@user = User.new
	end

If the Ruby example above is foreign, understand that `new` refers to a controller action, and that `@user` refers to an instance variable available within the `new` view; it's the same as a scope object exposed on an Angular controller. In Angular, it's often up to the developer to define a bit more of the model than it is in Ruby on Rails, since we'll often define the methods we use to create, read, update, and destroy model objects in the database on our Angular models. We then inject our model classes into the controller, which in Rails would be handled for us. In all though, it's no different from:

	$scope.user = new User();
	
Here we simply expose a user instance to a view, an empty user object with the properties and methods we need to allow the object to interact with the database. In Ruby the `@` sign refers to an instance variable, which is also true of `$scope`--it essentially says `this.user` where `this` refers to the view instance. 

Other Angular developers may note a potential point of contention here--we use `this` in directive controllers, so how could `this` be similar to `$scope`? The difference is that `$scope` refers to an object bound to and exposed to a view. 

#### The Glue

Angular is a front-end framework that's really good at helping us create single-page applications--applications that don't require a user to perform a time-intensive full-browser refresh to update the data on their screen. Instead we perform these updates asynchronously, but it's often up to us to let Angular know when data has changed so that the update can be immediately reflected on-screen. 

Angular's built-in directives (anything prefaced with `ng`) perform these asynchronous view-model updates for us, which may seem a bit magical, but it's a perfectly normal part of the process. Some Angular developers think that explicitly telling Angular about a model update that's occurred on the view is somehow going against the grain of the framework, but under the hood that's what Angular's doing for us already, and we just need to know how to tap into Angular's information propagation services. 

#### $watch, $digest



