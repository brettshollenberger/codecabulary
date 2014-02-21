# Rails Middleware

Rails middleware exists as a filtering layer to intercept a request and handle responses differently as that request goes to our Rails application. To understand middleware in Rails, we have to know a bit about Rack applications; Rails middleware are simply Rack applications.

Rack wraps HTTP requests and responses in the simplest possible way in order to unify and distal the API for web servers, web frameworks, and middleware into a single `call` method. A Rack endpoint accepts requests, calls its `call` method, and returns a response. Rails controllers themselves are just Rack endpoints.

In Rails 4, we can create a Rack middleware by placing it in `app/middleware/middleware_name.rb`. This file should define a class that has (at least) an `initialize` and a `call` method. The `initialize` method will be passed our Rails application, and our `call` method will be passed our environment.

	class MyMiddleware
		def initialize(app)
			@app = app
		end
		
		def call(env)
			@status, @headers, @response = @app.call(env)
			[@status, @headers, self]
		end
		
		def each(&block)
			block.call("<!-- Add some HTML comment to the response -->") if @headers["Content-Type"].include? "text/html"
			@response.call(&block)
		end
	end
	
Whatever the value of the response we return, that response must respond to an `each` method, and return string values. The body itself should not be an instance of string. As a Rack reminder, a Rack endpoint should return an array containing the values of the response status, headers, and body.

