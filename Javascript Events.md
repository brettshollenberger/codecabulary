# Javascript Events

### Javascript & The Future

Javascript is a single-threaded language, and it shares that thread with the browser. If we had a long-running process, and our code was being processed synchronously (one line after the next), then our entire application would halt during that process.

Javascript handles this problem by starting the long-running process, registering an event to fire when the process is done, and moving on. We can place a hunk of code to execute when the event finishes in a callback, but right now--the rest of the code needs to continue executing. The hunk of code that we give to the event when it's finished processing is called a _callback_; it's an ordinary function that runs when our event fires. 

	makeLongRunningRequest(function(response) {
		console.log('Finally! Here's the response: ', response);
	});
	
### Registering Events

Registering events can be a bit more confusing than it first appears. For instance, let's take a look at one of the most basic examples; we'll use the built-in `setTimeout`, which causes the function passed as the callback to be fired _no earlier than_ a certain interval from now:

	for (var i = 1; i <= 3; i++) {
		setTimeout(function() { console.log(i); }, 0);
	};
	
	> 3
	> 3
	> 3
	
Well that _is_ counter-intuitive, isn't it? Why isn't the output of our function 1, 2, 3? There are a few reasons:

* Javascript blocks (like `for` loops and `while` loops) do not create _scopes_. Scopes are _objects_ that contain references to all variables defined within them; for instance:

	var i = 1;
	function nestedOne(arg) {
		var j = 2;
	}
	
Creates a scope chain with two scopes--the global scope, containing `{ i: 1 }`, and the scope of nestedOne, containing `{ j: 2 }`. When we actually go ahead and run `nestedOne`:

	nestedOne('hello');
	
We create a new object, the _activation object_, that gets pushed to the front of the scope chain. The activation object has the highest priority; it sets the value of _this_ (the execution context), and the values of any arguments (`{arg: 'hello'}`). When deciding which variables map to which values, our function uses the scope chain, starting with the activation object, moving back to its own scope, and then to its parent scope, and so on. In this case, if we wanted to look up `i`, we would find it in the global scope still.

In the `setTimeout` example given at the start of this section, we have _not_ created a new scope in our block. Instead, each the variable `i` continues to overwrite its own value through each iteration of the loop. The `setTimeout` functions are created, and even though they have a timeout of `0`, _they cannot run until all other Javascript has finished executing and the thread is free_. That means that:

* After the loop, `i === 3`, since it was incremented until it failed the condition `i <= 3`. At 4 it did not run the loop again, and did not increment. 
* Javascript event handlers don't run until the thread is free. We said this before, but it bears repeating. When the `setTimeout` function runs, it evaluates the value of `i`. At this time, `i` is 3. So it return `3, 3, 3`. 

### Blocking the Thread