# Template Method Pattern in Angular Templates

Let's say we have model instances that will use much of the same functionality, with slight variances among a given algorithm. We're good programmers, and we don't like to repeat ourselves, so we want to vary the algorithm slightly without writing the same code over and over again--even when we work on the front end. The Template Method Pattern lets us do this, and thanks to Angular's `transclude` property on directives, we can include placeholders for these minor variations in our templates. 

At Faculty Creative, we've been working on a project where our design team asked me to style active projects in color and old projects in black and white. As you might image in, the HTML and LESS for the projects was pretty much the same, with some minor changes to applied classes in a few places. Here's one approach to keeping the code DRY:

Angular lets us write highly reusable code using directives--custom HTML templates with superheroic abilities. 

	angular
	  .module('app')
	  .directive('projectQuickView', function() {
	    return {
	      restrict: 'E',
	      replace: false,
	      templateUrl: 'app/templates/partials/projectQuickView.html',
	      transclude: true
	    };
	  });

In this directive, I've declared that we'll be able to write the directive as an element (`<projectQuickView></projectQuickView`), that we won't replace this element, but will rather interpolate the template into it, and I've declared where the template to interpolate lives. The kicker line is `transclude: true`, which allows us to utilize the built-in `ng-transclude` to interpolate a placeholder in our template, thus allowing us to vary the algorithm as we see fit. 

	<div class="project">
		<ng-transclude>
		... awesome project layout ...
	</div>
	
Then for old projects, our layout looks like this:

	<projectQuickView><span class="black-and-white"></span></projectQuickView>
	
And for new projects, our layout looks like this:

	<projectQuickView><span class="in-living-color"></span></projectQuickView>
	
And presto! We've used the same layout with slight differences for highly reusable code that can also take additional new stylings as we expand the project. Super rad. 