#IIFEs in CoffeeScript

Javascript's Immediately Invoked Function Expressions are a common paradigm for loading things like Backbone/Marionette applications. IIFEs, as their name suggests, are called as soon as they are evaluated, and then never called again--perfect for performing setup on an Application object and returning it to a property hanging off the window.

	(function() {
	  this.Demo = (function(Backbone, Marionette) {
	    App = new Marionette.Application();
	    
	    ...perform setup...
	    
	    return App;
	  })(Backbone, Marionette);
	}).call(this);
	
	// The call(this) paradigm is used by Backbone, Coffeescript, and others to explicitly declare the current value of this as the value of this in the IIFE. In our case, this will be the window object, which is the same as if we had invoked the function as a function ();
	
The function above will run immediately on setup, providing our window object with access to the Demo application:

	Demo
	> Marionette.Application {_regionManager: Marionette.RegionManager.Marionette.Controller.extend.constructor, ...
	
	// More explicitly:
	window.Demo
	// Returns the application as well
	
Cool, but we can't write that in CoffeeScript, right? Wrong. CoffeeScript's _do_ function performs the same setup:

	@demo = do (Backbone, Marionette) ->
	  App = new Marionette.Application
	  
	  ... perform setup ...
	  
	  App // Use CoffeeScript's implicit return
	  
Much more concise, and easier to read. Win win.