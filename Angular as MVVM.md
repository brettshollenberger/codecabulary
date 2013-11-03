# Angular as MVVM

Angular's core team refers to it as MVW--model view whatever--but one way of looking at the framework is as an MVVM implementation. The part here that's likely unfamiliar is the ViewModel, the "model of the view," which exposes a "view API"--data and functionality to the view--and allows the view to update the model through data bindings. 

The ViewModel in Angular are the `$scope` objects. Each `$scope` object represents a node in the DOM tree, exposes a set of functionality to that node in the tree, can update the model, and can inherit functionality from its parents in the tree, though it doesn't have to. Let's take a look at how the view creates `$scope` objects (ViewModel objects):

1) Each Angular directive can create a new scope, or use its parent scope:

	<-- ng-app creates $rootScope. Anything exposed on $rootScope is accessible here. -->
	<body ng-app="app">			
		<-- ng-controller creates a new scope that inherits from $rootScope. Anything accessible on either rootScope or MainCtrl's scope is accessible here -->
		<div ng-controller="MainCtrl">
			{{rootScopeProperty}} {{mainCtrlProperty}}
		</div>
	</body>
	
2) Your own directives can also create new scopes through the `scope: true` property. This property ensures that the new scope will inherit from its parent scope.

	.directive('myDirective', function() {
		return {
			scope: true
		}
	});
	
3) Your directives can also create new scopes that do not inherit from their parent scope via the `scope: {}` property. This means data accessible in parent scopes will not be accessible in the child scope, unless we explicitly pass it in through an attribute on the view:

	<body ng-app="app">
		<div my-isolate-directive user="user"></div>
	</body>
	
	.directive('myIsolateDirective', function() {
		return {
			scope: { user: '=' }
		}
	});
	
In the new scope, we explicitly say we want to inherit the `user` object from `$rootScope`, but nothing else will inherit in the isolate scope. It will become modularized.

4) Your directives do not have to define a new scope. By default, they do not. We can also explicitly say we do not want to create a new scope (we want to use the parent scope) by setting the `scope: false` property:

	.directive('my-directive', function() {
		return {
			scope: false
		}
	});