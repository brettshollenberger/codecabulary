# Anglar Filters

Angular JS is a framework for front-end development, and as such provides filter functionality for working with data--allowing users to search through data, limit the data they see, order the data, etc.

An Angular filter works by using the pipe character (`|`), and filters can be stacked:

	<!doctype html>
	<html ng-app="app>
		<head></head>
		<body>
			<div ng-controller="FirstController">
				<table>
					<thead>
						<th>Actor</th>
						<th>Character</th>
					</thead>
					<tbody>
						<tr ng-repeat="actor in avengers.cast | filter:search | orderBy: 'name' | limitTo: 5">
						<td>{{actor.name}}</td>
						<td>{{actor.character}}</td>
					</tbody>
				</table>
			</div>
			<script src='http://ajax.googleapis.com/ajax/libs/angularjs/1.0.7/angular.min.js'></script>
			<script>
				var app = angular.module('app', []);
				
				app.factory('Avengers', function() {
					var Avengers = {};
					
					Avengers.cast = [
					    {
					      name: "Robert Downy Jr.",
					      character: "Tony Stark"
					    },
					
					    {
					      name: "Chris Evans",
					      character: "Captain America"
					    },
					
					    {
					      name: "Mark Ruffalo",
					      character: "Bruce Banner"
					    },
					
					    {
					      name: "Chris Hemsworth",
					      character: "Thor"
					    },
					
					    {
					      name: "Scarlett Johansson",
					      character: "Black Widow"
					    }
					];
					return Avengers;
				});
				
				app.controller("FirstController", [
					"$scope",
					"Avengers",
					function($scope, Avengers) {
						$scope.avengers = Avengers;
					}]);
			</script>
		</body>
	</html>
	
#### Defining Custom Filters

We can also define custom filters that can be reused throughout our applications. To register a filter, we use the `filter` method, which creates an injectable filter factory, and in our callback we return the filter function:

	app.filter('piglatinize', function() {
	  return function(line) {
	    return line.split(' ').map(function(word) {
	      return word.slice(1, word.length) + word[0] + 'ay';
	    }).join(' ');
	  };
	});