# Javascript Ninjahood

1) Functions create scopes, blocks do not

2) Named functions are forward referenceable, but variables are not

3) The browser runs the event loop and dispatches events; there are four types of events:

* Browser events (page loaded)
* Network events (AJAX request complete)
* User events (mouse clicks, key presses)
* Timer events (request timeout)
* 
4) Functions are first-class objects; they can:

* Have properties assigned to them
* Be passed as arguments to functions
* Be returned from functions
* Be created with literals

5) There are four ways to execute functions in Javascript, and each bears an impact on the value of `this` (the function execution context):

* As a function. A basic function call in which the window object is the value of `this`.
* As a method: An object-oriented function in which the value of `this` is the object itself (the method's owner).
* As a constructor: Initiates an empty object, which is passed as `this`, initialized, and returned (as long as nothing else is returned from the constructor).
* Via `call` or `apply`: Call and apply are methods of functions. They alter the execution context to be the first argument. The remaining arguments via apply should take the form of an array.

6) Function overloading (passing too many parameters to a function) results in the excess parameters not being assigned to named parameters. These are still accessible via the `arguments` hash, however.

7) Function underloading (not passing enough parameters to a function) results in the excess parameters being set to `undefined`.

8) Named functions are no longer anonymous, and can be copied without fear of the original function disappearing:

	var ninja = { chirp: function chirp(n) { return n; } }
	var sam = { chirp: ninja.chirp }
	ninja = {}
	sam.chirp(1)
	> 1
	// It still exists

