# Function Currying

Currying is the technique of transforming a function that takes multiple arguments in a way that it can be called as a chain of functions, each with a single argument (partial application). Said another way, currying is taking a function that takes `n` arguments, and splitting it into `n` functions that take one argument, and partial application is calling a function with some number of arguments in order to get a function back that will take many less arguments. 

	# Currying in Javascript
	
	function curryAddition(x) {
		return function(y) {
			return function(z) {
				console.log(x+y+z);
			}
		}
	}
	
	curryAddition(1)(2)(3);
	>> 6
	
	# Partial Application in Ruby
	
	@a = proc { |x, y| proc { |z| x + y + z } }
	@b = @a[1, 2]
	@b[3]
	>> 6

If that all looks pretty theoretical, don't worry. We have other important uses here that can help us DRY up our code:

	call_on_args = proc do |meth, actor, *args|
		current_actor = actor
		args.each { |arg| current_actor = current_actor.send(meth, arg) }
		current_actor
	end
	
	add = call_on_args.curry.(:+)
	subtract = call_on_args.curry.(:-)
	
	increment = add.curry.(1)
	decrement = subtract.curry.(1)
	
The important thing to notice here is that we're able to use partial application in conjunction with currying to write general functions and then derive different functionality from them as necessary. 