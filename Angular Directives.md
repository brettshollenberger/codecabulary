# Angular Directives

One of the most iconic attributes of Angular.js is its `directives`, reusable hunks of code that are meant to teach HTML new tricks. If we want to make a `slider`, a `nav`, or a `Create Project Button` that instantiates a new project object for us, we can use directives to write DRY code. 

#### Writing Directives

In order to tap into our directives, we have to define them on an Angular module object and give them a name that we can refer to later. Here we'll make an accordion UI unit, and we want to namespace it so that it doesn't bump into other directives we might want to use; I work at Faculty Creative, so I namespace my modules with `faculty-`:

	angular
		.module('app') 	// This should be named whatever the name of our app module is named
		.directive('faculty-accordion', function() {
			return {};
		});
		
The name of the directive is important. If we use the directive as an element, we'll write `<faculty-accordion>Some content</faculty-accordion>` in our HTML, and if we use it as an attribute, we'll write `<div faculty-accordion>Some content</div>`. We'll also use the name of the directive if other directives want to communicate with this directive, like if we created an entire set of accordion objects that knew to close if another accordion object in the set was open. 
		
#### The Directive Definition Object

As you can see in the example above, directives return an object, the directive definition object. The directive definition object defines a number of options that describe how your directive works. For instance, we can use the `restrict` option to restrict how we can instantiate our directive via HTML (as an element, attribute, class, or comment, or any combination of the above). 

Before we get into an exhaustive list (you can check out the documentation for that if you really want to read it now), let's just look at a few of the most common options for directives, and keep moving to show you how we'll instantiate our directives, how we can create reusable components, and how we can have our directives talk to one another. 

	angular
		.module('app')
		.directive('faculty-accordion', function() {
			return {
				restrict: 'EA', 	// E stands for elements and A for attribute. These are the two most common options.
				template: '<div ng-transclude></div>', // Sets up the template that our directive will inject into the HTML whenever it is used
				transclude: true,  // We use transclusion when our directive is meant to be a container that wraps additional content. Since we expect to add accordion-objects to our accordion, we set transclusion to true, and use the ng-transclude directive in our template above to describe where transcluded content will be placed in the final template.
				replace: true // If we set this to true, the <faculty-accordion> element will be replaced with the template. Otherwise, the template content will be wrapped in the <faculty-accordion> object.
			}
		});
		
Our directive can be used as either an HTML element or attribute, and it will replace the `<faculty-accordion>` or `<div faculty-accordion>` object with the template `<div ng-transclude></div>`, which will transclude child content, our accordion-objects, into the div, so the final HTML will look something like:

	<div ng-transclude>
		<accordion-object></accordion-object>
		<accordion-object></accordion-object>
	</div>

Before we move on, it's important to note that in production, we're more likely to use the `templateUrl` option on directives, which allows us to pass the URL of a template to use, instead of writing inline HTML. We're using `template` instead of `templateUrl` in this example because it's easier to see what's going on in one go, and also because if you aren't hosting your Angular project on a web server, Chrome will not interpolate locally hosted templates without receiving special command line instructions. 

#### 
				
