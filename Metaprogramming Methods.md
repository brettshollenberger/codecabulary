# Metaprogramming Methods

Metaprogramming is the use of dynamic programming languages constructs that allow code to either be dynamically created or evaluated. It's often described as "code that writes code," but that's not 100% accurate--the real concept behind metaprogramming is dynamic evaluation. Static languages like Java are run through a compiler to determine whether or not particular objects respond to particular methods--if they don't, you'll get loud compiler errors. Dynamic languages like Ruby or Javascript won't complain about a method they can't understand until you actually call a method that doesn't exist. Dynamic evaluation leaves the problem of a non-existent method until runtime--when it could still be created or found by some other means than existing on the object itself. Let's look at a few techniques.

## Dynamic Dispatch

Dynamic Dispatch is the process of determining which methods to run at runtime. These methods may not be known until the code is actually called, and are evaluated as normal parameters.

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
	
## Dynamic Method

Dynamic Methods might be better termed "dynamic method creation." Just like with dynamic dispatch, we can achieve the flexibility we need by passing the name of the method to define as an argument to our function. Since the name is just a regular argument, we have the full flexibility of the programming language at our fingertips--we could pass arrays of names, grab the value of some property, or whatever else we like to use as the name. 

In the Javascript example of dynamic dispatch, above, we've actually already seen one example (using the `define_method` method that we wrote). Let's see how we can accomplish the same task in Ruby:

	# Ruby
	class Post
		attr_accessor :status
		
		def self.statuses
			["draft", "published", "archived"]
		end
		

		self.statuses.each do |status|
			define_method "#{status}?" do
				@status == status
			end
		end
	end
	
Since the names of the methods are just strings, we can add arbitrary formatting, such as the addition of the `?` at the end of the method name--a common Ruby convention for methods that return a boolean value. 

## Ghost Methods

_Sorry Javascripters, this functionality is not available in JS. Everything described here applies to Ruby (and thus is unafraid to use strictly Ruby terminology)._

Ghost methods are messages that can be sent to a particular object--that the object will respond to--that are not defined on the object's class, or anywhere in the class's ancestors chain. Huh? If you're as pedantic about Ruby as I am, you know that all of an object's methods are housed in those locations, so what gives? Where else could a method possibly be defined? 

The spooky part is _ghost methods aren't defined anywhere_. I mean, spooky, right? It's not quite as mysterious as that though--the methods may not be defined formally, but they do match a particular pattern that's been established on the class, and handed off to another method.

When Ruby calls a method on an object, it looks through that object's class and ancestors chain until it finds the method. If it doesn't find the method, it gives up, and hands off the call to a special method in the object's class: `method_missing`. Method missing can define arbitrary rules for pattern matching (or really anything it wants), and determine that it can, in fact, respond to this particular method call. It's easier with an example:

	# An example from the Rails source
	#
	# Define a String subclass that responds to any method ending with a question mark "?"
	# as if the method were interrogating the value of the string.
	
	class StringInquirer < String
	
	private
		def method_missing(method_name, *args, &block)
			if method_name[-1] == "?"
				self == method_name[0..-2]
			else
				super
			end
		end
	end
	
	fun = StringInquirer.new("fun")
	
	fun.fun?
	>> true
	
	fun.not_fun?
	>> false
	
	fun.something_else?
	>> false

Pretty neat, huh? An important note about ghost methods: it's important that the class respond to `respond_to?` appropriately as well.

	# The problem
	fun.respond_to?(:fun?)
	>> false
	
	# The solution:
	# Re-open the class and add `respond_to_missing?` method, stating that we'll
	# respond to any method ending in a question mark
	class StringInquirier
		def respond_to_missing?(method_name, include_private=false)
			method_name[-1] == "?"
		end
	end
	
	fun.respond_to?(:fun?)
	>> true
	
## Dynamic Proxies

Dynamic proxies are classes that wrap another object, and forward calls onto that other object, almost exclusively relying on `method_missing`. An example might be an API wrapper that forwards calls onto an API, and returns responses. We can look at a simpler example instead:

	class Component
		attr_accessor :name, :cost
		
		def initialize(name, cost)
			@name = name
			@cost = cost
		end
		
		def spec
			"#{@name} for #{@cost}"
		end
	end
	
	class Computer
		attr_accessor :components
		
		def initialize(*components)
			@components = components
		end
		
	private
		def method_missing(name)
			if @components.map(&:name).include?(name.to_s)
				@components.select { |component| component.name == name.to_s }.first.spec
			else
				super
			end
		end
		
		def respond_to_missing?(name, include_private = false)
			@components.map(&:name).include?(name.to_s)
		end
	end
	
	monitor = Component.new("monitor", 100)
	ram = Component.new("ram", 100)
	computer = Computer.new(monitor, ram)
	
	computer.monitor
	>> "monitor for 100"
	
	computer.respond_to?(:ram)
	>> true
	
	
	
	