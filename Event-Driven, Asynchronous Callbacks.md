# Event-Driven, Asynchronous Callbacks

In languages like Ruby and Python, when a process proceeds slowly, like a massive database query, it proceeds slowly for the user that requests it. In Javascript, there's just one process, and we could quickly encounter global issues that affect all users when just one or two users makes a resource-intensive request.

Javascript solves this issue differently from Ruby or Python. In each language, there's a basic, procedural means of writing, which proceeds to each command after completing the previous command:

	var records = db.query("SELECT * FROM massiveTable");
	doSomethingElse();
	
Since all users would be affected by the blocking nature of a single user's procedural chain in Javascript, Javascript provides a means of asynchronously completing its tasks. This a asynchronicity is built on three principles:

1) _Callback events as function parameters_: If a request could take a while, Javascript lets you provide a callback function as an argument to that function (don't forget, as a functional language, functions can be passed around as first-order objects). 

It uses the callback function as a mental note of what to do when it's finished with the long request, and tells the next command in the procedural chain that it can get started on its work while the lengthy command finishes up its work.

Instead of the code above, a function with a callback would look like:

	db.query("SELECT * FROM massiveTable", function() {
	  somethingToDoWhenFinished();
	});
	
2) _Asynchronous processing_: Built on the back of the callback function, Javascript's procedures can proceed without finishing previous tasks as long as the previous tasks have callbacks to tap into when they're finished. Functions with callbacks will allow the next task to begin after they get to work.

3) _Event Loop_: When Javascript has nothing else to do, it enters the event loop, and will continue to cycle through the event loop until a new event occurs. When an event occurs, like the long database request finishes processing, its callback is fired and takes precedence. Since node can only run a single process at a time, only one callback will ever occur at a time, and the rest will wait in line, though there's no guarantee on the order the callbacks will fire in. 

