# Higher Order Functions

Higher-order functions do one or more of the following:

* Accept a function as an argument
* Return a function as the return value of a function

Take for example `map` in Ruby:

	# Accepting a function as an argument
	
	def increment(n)
		n + 1
	end
	
	[1, 2, 3].map(&method(:increment))
	
Or currying in Javascript:

	# Returning a function as the return value of a function
	
	function plusN(a) {
		return function(b) {
			return a + b;
		}
	}
	
	var increment = plusN(1);
	
	increment(2);
	>> 3
	
There are several specific types of higher-order functions.

## Combinators

Combinators have a precise technical meaning in mathematics:

> "A combinator is a higher-order function that uses only function application and earlier defined combinators to define a result from its arguments."

We can use a looser definition of combinator: 

> "Higher-order pure functions that take only functions as arguments and return a function."

We won't be strict about using previously defined combinators in their construction. 

### Practical Combinators

Code that uses a lot of combinators tends to name the verbs and adverbs (like `double` and `increment`) while avoiding language keywords and the names of nouns. So one perspective is that combinators are useful when you want to emphasize what you're doing and how it fits together, and more explicit code is useful when you want to emphasize what you're working with. 

#### Compose

Logicians call this the B combinator or "Bluebird", though programmers tend to call it "compose."

	function compose(a, b) {
		return function(c) {
			return a(b(c));
		}
	}
	
#### Function Decorators

A function decorator is a higher-order function that takes one function as an argument, returns another function, and the returned function is a variation of the argument function. Here's a ridiculous example of a decorator:

	function not(fn) {
		return function() {
			return !fn(arguments);
		}
	}
	
	function cool() { return true; }
	
	var uncool = not(cool);
	
	uncool();
	>> false