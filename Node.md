# Node

Node is a runtime environment and a library--it's a Javascript implementation outside of the browser and a collection of modules to extend Javascript.

What really differentiates node is that when we write applications, we not only implement our application, we also implement an HTTP server. Not only is this different from other Javascript frameworks, which don't have the ability to serve webpages, it's also different from working in languages like Ruby, Python, and PHP, which don't handle the HTTP request hand-off and page serving in the language itself. 

#### Writing a Basic Server

Since node is part-library, it contains useful modules for us to do good things like implement servers. The library we'll want to include to get started is HTTP, which contains, as we might expect, a tasty API for creating web servers. Let's look at an example of a static server:

	var http = require("http");
	
	http.createServer(function(request, response) {
		response.writeHead(200, {"Content-Type": "text/plain"});
		response.write("Hello World");
		response.end();	
	}).listen(8888);
	
Now the gist of what's going on should be fairly easy to parse--we're returning a header with a 200 status code and a plain text body reading "Hello World." Visiting port 8888 shows us a successfully returned HTTP response, but what exactly's going on here?

`createServer` returns a new server object, which expects a `requestListener`, a function that's automatically added to the `request` event. The request event is automatically emitted each time there's a request. Thus the `createServer` function in the example returns the same static response object every time it receives an HTTP request. The returned HTTP server object has a method named listen that can take the port name to listen to, as we've done here.