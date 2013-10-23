# Angular Minification

To minify Angular scripts for production, we need to use the array notation for dependency injection:

	angular.module('app', [])
		.controller('MyController', ['$scope', function($scope) { }]);
		
The reason we do this is because function arguments will get reduced to simple variables by the minified:

	angular.module('app', [])
		.controller('MyController', ['$scope', function(a) { }]);
		
With the array notation, it doesn't matter if the variable name gets minified, because it's still able to look up the dependency name using the un-minified string "$scope." In this notation, the order of our arguments matters--we use that order to determine that `a` refers to `$scope`. 

It's important in Angular to understand when array notation is necessary, and when it isn't. When Angular is injecting dependencies--array notation is required. Angular injects dependencies into `services`, including `directives`, `controllers`, `providers`, `factories`, and of course `services` themselves.

Let's check out the difference between a directive's controller function and its link function (remember, the controller function is _still_ a controller). 

	angular.module('directives', [])
		.directive('myDirective, function() {
			return {
				controller: ['$scope', function($scope) { 
					$scope.myCoolVariable = "cool!";
				}],
				link: function(scope, element, attars) {
					scope.myOtherCoolVariable = "other cool!";
				}
			}
		});
		
In the controller function, we have to use the array notation to inject the dependencies. The link function however is a standard function using ordered arguments. We could have also written the arguments like this:

	link: function($scope, $element, $attrs) { };
	
Or even this:

	link: function(element, attrs, grapefruit) { };
	
The first argument in a link function always refers to the scope, the second to the element, and the third to the attributes--no matter what they're called. These are ordered parameters. For this reason, we don't need to protect these against minification. When we use the array notation for dependency injection, we transform the arguments in that function into order parameters, and offer them the same protection, keeping our app safe and happy. 