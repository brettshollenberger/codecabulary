# Syncing Angular Models

>tl;dr: Angular's dirty checking mechanisms will only sync objects if they are exactly the same object (===). Make self-caching models return instances they've previously generated (look up by primary key) to greatly simplify working across multiple scopes. 

One aspect that distinguishes modern web applications in a way that makes them begin to rival desktop applications is their ability to keep data in sync across the page. We'd expect that if we updated a cell in an Excel document that the data would be updated both in the cell itself and in the function bar at the top of the screen. We'd expect that the cells whose values depend on the data in the cell we're editing (via relationships or functions we've established) to update as we type. Without properly synced data, the user experience can become confusing, with users asking themselves which version of their data is correct.

One of the truly killer features in Angular is its dirty checking features, which allow us to keep our data in sync. The canonical "Hello World" example in Angular involves syncing data between two related entities, like a form field and a data binding. Those of us that have built AJAX applications by hand likely squealed with delight the first time we saw something like:

	<input ng-model="user.email">
	{{user.email}}
	
Oh yeah. That's the stuff. It just makes sense, right? Only one problem: real applications are big; they contain many scopes, whose relationships to one another may change at any given time, and often times we'll want to isolate the scopes of reusable components to ensure they only impact the data they're meant to impact. The main point here: we should not rely on scope inheritance to propagate data changes throughout our application; the example above works fine because the two units share a scope, and other simple examples work if scopes inherit from one another. But what if we break the chain through isolate scopes or our scopes are siblings of one another? We need a more robust way of ensuring that objects that are meant to share data are in fact the same object. 

#### Sure-fire Data Syncing through Model Caching





Angular.js provides us with powerful tools to keep this data in sync, namely `$apply` and `$watch`. If we use any of Angular's built-in directives, they'll make use of these tools under the hood for us--no assembly required. Angular includes an observer pattern to propagate model changes throughout the system that allows us to both `$watch` data for changes (register event callbacks when data changes), and to `$apply` new changes (tell the `watchers` that something has changed, and fire their callbacks). Let's look at an example of how Angular's built-in directives work together to keep one another in sync:

	<input ng-model="user.email">
	{{user.email}}
	
In the example above, we've actually used three directives--data binding, `ng-model`, and `input`. The `ng-model` directive registers the `$apply()` call under the hood--it uses the browser's built-in "input" event (except in IE9) to fire an event listener that checks whether or not the new value in the input field matches the old one; if it doesn't, `ng-model` fires the `$apply` to alert any `$watch`ers on the page that the data has changed. The data binding directive meanwhile has set up a `$watch`, so when the `$apply()` call kicks fires, the data binding will know that it needs to update.

The really super duper important point about `$apply` is that it will fire a different event--the `$digest` cycle, _which fires every $watch on every scope in the application_. This is meaningful because if multiple instances of the same object exist in different scopes in the application (even isolate scopes), every single one of those will be updated for free by Angular's built-in mechanisms. 

Due to this aspect of Angular's model architecture, we should construct constructors to cache model objects that they've instantiated during the application lifecycle. If different controllers and directives use the ID of a user to instantiate a new User object, the constructor should return the cached user object if it's already been instantiated to ensure the same object is used across the application. 



Angular is a smart system, but it doesn't know when one of our directives is making a change to model data unless we tell it so; we have to explicitly call `$scope.$apply` to alert Angular of the changes. `$apply` will kick off the `$digest` loop, which evaluates all `$watch` on all scopes, ensures all model values are stable, and then repaints the UI in a single batch when everything is ready to go. This batching process is more efficient than updating the DOM on every change because DOM events, DOM rendering, and Javascript execution all share a single thread in the browser, and have to pass control back and forth; if we updated the DOM on every change, we'd see some ugly UI flickering.  
