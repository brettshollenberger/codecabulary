# Template Method Pattern in Angular Templates

Let's say we have a lot of functionality that's the same across multiple directives--very similar HTML output with minor differences based upon the state of a certain object that's being output. We know we want to DRY up our templating, and thanks to Angular's transclude attribute in directives, we can follow the Template Method Pattern to vary our algorithm based on these minor differences.

Recently our designer told me to output projects in color if they were active, and black and white if they were closed. The HTML and CSS was basically the same for the projects, so how to switch up the styling based on the project's state?

In Angular, we can vary the algorithm with transclusion:

	<project-item><img ng-src={{project.img}} class="black-and-white"></project-item>
	
Then in our directive:

	transclude: true
	
And in our template:

	<div class="project">
		<span><ng-transclude></span>
		... awesome project layout ...
	</div>
	
And the image with the proper class will be interpolated into the position of the `ng-transclude` call. Thereby we can have two templates doing mostly the same thing, but varying their class based on project state.