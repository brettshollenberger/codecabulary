# Metaprogramming Methods

Metaprogramming is the use of dynamic programming languages constructs that allow code to either be dynamically created or evaluated. It's often described as "code that writes code," but that's not 100% accurate--the real concept behind metaprogramming is dynamic evaluation. Static languages like Java are run through a compiler to determine whether or not particular objects respond to particular methods--if they don't, you'll get loud compiler errors. Dynamic languages like Ruby or Javascript won't complain about a method they can't understand until you actually call a method that doesn't exist. Dynamic evaluation leaves the problem of a non-existent method until runtime--when it could still be created or found by some other means than existing on the object itself. Let's look at a few techniques.

## Dynamic Dispatch

Dynamic dispatch is the process of determining which methods to run at runtime. These methods may not be known until the code is actually called, and are evaluated as normal parameters.

	# Ruby
	# OpenStruct is a class that allows us to add arbitrary instance variables to an object
	
	config = OpenStruct.new
	config.favorite_language = "Ruby"
	config.favorite_language
	>> "Ruby"
	
	# Dynamic dispatch allows us to do things like loading a file to determine which methods to run:
	# 
	# File: config.rc
	# 
	# name : "Brett"
	# favorite_language: "Ruby"
	
	# We can dynamically dispatch "key=" calls to the config object using #send in Ruby
	YAML.load_file("config.rc").each do |key, value|
		config.send("#{key}=", value)
	end
	
	// Javascript
	// Add a helper for adding new methods to Javascript objects
	Object.prototype.define_method = function(name, body) {
		Object.defineProperty(this, name, {
			enumerable: true,
			configurable: true,
			value: body
		});
	}
	
	// Define a Mailer class that automatically defines new methods called "send_" + message_name
	function Mailer() {
		this.messages = {}
		this.addMessage = function(name, body) {
			name = name.toLowerCase();
			this.messages[name] = body;
			this.define_method("send_" + name, function() {
				console.log("Sent: " + body);
			});
		}
	}
	
	// Create a mailer instance, and add a "Join" and "Leave" message
	var mailer = new Mailer();
	mailer.addMessage("Join", "Thanks for joining our site!");
	mailer.addMessage("Leave", "Sorry you left us.");
	
	// We only use Backbone here for the eventing system. 
	// The User model describes the events "Join" and "Leave" which a user
	// may perform. When the user performs one of these methods, we dynamically
	// dispatch a method call named "send_join" or "send_leave" to the mailer.
	var User = Backbone.Model.extend({
		initialize: function() {
			var events = ["join", "leave"];
			_.each(events, function(events) {
				this.on(event, mailer["send_" + event]);
			}, this);
		},
		join: function() {
			this.trigger("join");
			// join code
		},
		leave: function() {
			this.trigger("leave");
			// leave code
		}
	}
	
	// When the user joins, we dynamically dispatch "send_join" to the mailer instance
	var user = new User();
	user.join();
	>> Sent: Thanks for joining our site!
	
	
	
	
	