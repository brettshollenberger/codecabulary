# Kestrels

In any language that supports higher-order functions (functions that either accept functions as arguments or return functions), programmers can use Kestrels, functions that return a function that returns a constant function. Don't worry if that sounds confusing; let's break it down:

	# function that returns a function
	function function_returner() {
		return function() {};
	}
	
	# function that returns a function that returns a constant (non-changing) function
	function function_returner(fn) {
		return function constant_function_returner() {
			return fn;
		}
	}
	
Actually not too complicated, right? But why on earth would we want to use a Kestrel, or "K Combinator?"

We could use it to perform some intermediary work:

	# Logs out the person in the intermediate stage, but is still able to work with the Person instance
	# without explicitly returning `p`
	address = Person.find(...) do |p|
		puts "Found person #{p}"
	end.address
	
In short, the block is only around to perform some side-effects. That means Kestrels are also usually useful in object initialization blocks:

	class Contact < Struct.new do
		def to_hash
			Hash[*members.zip(values).flatten]
		end
	end
	
Rather than return the value of the block, the block performs certain setup within the context of the class, and the new Struct is returned as the class's ancestor. 

	