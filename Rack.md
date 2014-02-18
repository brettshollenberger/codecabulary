# Rack

Rack is a modular interface for handling web requests, written in Ruby, with support for many different web servers.

It abstracts the handling of HTTP requests to a single, simple, call method.

Rack can be used by anything from a plain Ruby script to a Rails application.

	class HelloWorld
		def call(env)
			[200, {"Content-Type" => "text/plain"}, ["Hello world!"]]
		end
	end
	
	Rack::Handler::Thin.run HelloWorld.new, :Port => 8080
	
Incoming HTTP requests invoke the call method, and pass a hash of environment variables.

The call method should return a 3-element array consisting of the status, a hash of response headers, and the body of the request.

In a Rails application, we have a Rake task to see which Rack middlewares are in use:

	rake middleware
	
