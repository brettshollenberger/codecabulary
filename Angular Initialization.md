# Angular Initialization

Angular automatically initializes under one of the following circumstances:

* When the DOMContentLoaded event fires. This event fires when the browser has finished loading and parsing a document, excluding its dependencies, like stylesheets and images.
* When the Angular source code on a page is evaluated if at that time `document.readyState` is set to `complete`. While a document is loading, its state will be `loading`; while it's loading dependencies, its state will be `interactive`; and when it's finished loading, it will reach `complete` state.

Then Angular performs four actions during initialization:

1) Find the `ng-app` directive on the page. This directive might be on the `<html>` element if we want Angular to manage the entire document, or on a `<div>` if we want it managing just a part of the page.

2) Load the module associated with the `ng-app` directive. Although Angular does not require you to use its module system, this is a great reason to do so: you can determine what will be loaded during initialization. Also, modules are essential for managing larger, complex apps, and Angular is intended to be a front-end solution for single-page applications, which are increasingly complex on the front-end, so it's a good practice to get used to.

Generally, you'll have a root file called `app.js` (we store ours in `client/app/scripts` because we currently use the [Genesis Skeleton](http://genesis-skeleton.com) build for our MEAN apps), and in that file you'll create the app module:

	// Name the ng-app directive in the index.html file
	<html ng-app="app">
	
	// Use the same module name in index.js
	angular
		.module('app', [dependencies])
		.config([dependencies], callback() {
		});
		
3) Create the application injector. $injector is a service provided by Angular that allows for instantiation, annotation, and retrieval of other registered services. Often injection is performed via the inline notation that's seen on the previous example, wherein the names of a function's dependencies are listed as strings, which $injector looks up and injects into the objects or functions:

	app.controller('CommentsController', ['$scope', 'Comment', 'FacebookService', function($scope, Comment, Facebook) {
		... wonderful controller logic ...
	]});
	
4) Compile the DOM, treating the `ng-app` directive as the root of the compilation. In this way, you can use the `ng-app` directive to dictate that only a piece of your application should be visible to Angular, with the rest managed by Rails or some other framework.

