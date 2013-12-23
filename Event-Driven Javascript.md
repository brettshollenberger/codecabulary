# Javascript Events

Javascript is a single-threaded language, meaning only a single process can be handled at a time. If a long-running process were to tie up the thread, it would spell disaster (and slow code). 

To handle this issue, Javascript relies on event-driven callbacks--functions that are registered to be called when an asynchronous process completes. 

The most important point here is that Javascript event handlers do not run until the thread is free. Even if an asynchronous event should fire immediately, it will not actually fire until the rest of the code has finished processing, and the thread is ready:

	var functionHasReturned = false;
	
	setTimeout(function() {
		console.assert(functionHasReturned);
	}, 0);
	
	var functionHasReturned = true;
	
The code above will never fail. Though the callback fires after 0 milliseconds, Javascript will not enter the event loop, and call the callback until after it has processed functionHasReturned = true.