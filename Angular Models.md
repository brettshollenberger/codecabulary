# Angular Services as Access to Models

In Angular.js, the concept of a model is not well-described. That's because--seriously--there aren't models in Angular, even if the powers that be say there are. Traditionally, we think of a model as a representation of a type of data and a means of interacting with a database to persist that data. That means the model must live on the server, and Angular lives on the client. 

Fine, but pedantry aside, interacting with our data in Angular is also kind of a pain for a "framework" that describes the model as the ultimate source of truth. In Angular, we could easily setup a "model" that persists nowhere and is defined in a single place:

	<input type="text" ng-model="data">
	{{data}}
	
In the example above, anything typed into the input becomes bound to the "data" model, and updated in the binding below (since Angular's two-way binding automatically represents changes to the model in the view, and vice-versa. And those things are all super rad, but this "model" is bound to its scope, and _not_ to anything global, since Angular's allergic to things that should be global, and I count data and services among those things.

For instance, we can see how in two different scopes (two separate controllers and `<div>`s) we already lose the wonderful power of the two-way binding--the "data" model only refers to the "model" within its own scope: 

	<div ng-controller="FirstScope">
		<input type="text" ng-model="data">
		{{data}}
	</div>
	
	<div ng-controller="SecondScope">
		<input type="text" ng-model="data">
		{{data}}
	</div>

These scopes don't share "data," which should be the canonical representation of data, which should end up being wildly confusing in an app of any respectable size. So how can we share data among controllers?

#### Useful Angular "Models"

Again, I'm using the term "model" here loosely because a service is not a model, but _services_ are the accepted means in the Angular community of hooking up data among various controllers. A service is a factory that returns a singleton object containing the properties of a particular model. Since that's the case, we can share this singleton among our controllers through the magic of Angular's dependency injection, and cause these two scopes to update the same "data" model:

	myApp.factory("Data", function() {
		return {
			message: "Service data coming atcha!"
		};
	});
	
	function FirstScope($scope, Data) {
		$scope.data = Data;
	};
	
This method is better, but still kind of crummy--why should we have to inject the canonical service into every controller? We should be creating an ApplicationController a la Rails, which the other controllers inherit from, and injecting the dependencies there so we don't have many injections to perform with every new service. 

