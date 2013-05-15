#### Johnny's rb group chat

####Closures:
> A function or method that can be passed around like an object. It remembers variables that were in scope when it was created.

Blocks are nameless functions, which you can pass to another function that can use the functionality.

Many other languages use this style (lambda in Python). In Ruby the difference is mainly a different kind of syntax.

#### Blocks

Why blocks? 

* Versus for loops: For loops are tightly coupled; dependent on the collection or iterator.
* Blocks help you decouple logic from iteration. 
* They appear after method invocation
* They take parameters, like methods
* Locals defined in blocks are inaccessible to the outside world, but they can access vars defined elsewhere.
* Yield used to transfer control to the block and back
		
		def double n
			yield n
		end
		
		double(50) { |n| n*2 }

* Block rules: 
	* Parameters to a block are _always_ local to that block. Similarly named variables outside the scope are overridden. 

#### Procs

 		double = Proc.new{ |n| n*2 }	
 		[1,2,3].collect ( &doubler )
 		[1,2,3[.map ( &doubler )
 		
* Methods like collect and map expect a block
* The ampersand in front of the Proc name converts it to its block origins

Proc benefits:
* They can be passed around as objects, unlike blocks
* Unlock blocks they can be called upon multiple times, making the code DRYer

#### Lambdas

		lambda { puts "Hello Boston" }
		# Equivalent to
		Proc.new { puts "Hello Boston" }
		
		names = ["x", "y"]
		symbolize = lambda { |e| e.to_sym }
		symbols = s.collect( &symbolize )
		
* Lambdas check the number of arguments passed in. Procs do not.
* Procs ignore any missing arguments, assigning nil to them.
* Lambdas return to the calling method. Procs return immediately without going back to the caller.

#### Behavior of missing arguments
		cute_proc = proc { |name| puts "Aww, my little #{name} looks so cute!" }
		cute_proc.call "Ellie" => "Aww, my little Ellie looks so cute"
		cute_proc.call => "Aww, my little  looks so cute"
		
		cute = lambda
		Lambda raises ArgumentError if arguments are missing
		
#### Behavior of return statements

		def return_from_proc
			my_proc = proc {return "Return from proc inside method" }
			my_proc.call
			return "Return from proc on last line of method"
		end
		
		def return_from_lambda
			my_lamda = lambda { return "return from.." }
			my_lambda.call
			return "Return from..."
		end
		
		puts return_from_proc
		puts return_from_lambda

* Calling return from inside a proc will return from calling method
* Calling return from lambda will return only from the lambda

#### Scope Retention of Closures
Potential ways to retain variables in closures:
* Create a copy of all variables at time of definition
* Retain reference (like a picture) to variables (Ruby uses this method) -> Better for garbage collection

[wickedgoodruby.com](wickedgoodruby.com)
		
		
		