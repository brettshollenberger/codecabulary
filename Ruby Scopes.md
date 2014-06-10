# Ruby Scopes

Any programming language, like any legal document, has a set of terms it defines--the basic concepts that either document works with. In a legal document, we might say:

	- MegaCorp, hereafter referred to as "the defendant," blah blah
	- Mom & Pop Shop, hereafter referred to as "the prosecutor," blah blah
		- Some nested statement mentioning "the defendant"
		- Some nested statement mentioning "the prosecutor"
		
In some other legal document, "the defendant" and "the prosecutor" probably have two totally different meanings. They might have the same meaning, but we wouldn't suspect that they did unless someone explicitly told us that they did. 

This is what "scopes" are in programming languages--the contexts in which we understand terms (or variables, constants, methods, etc) to have particular values. The interpreter will complain if it doesn't understand a particular term, because it hasn't been defined in the context it's evaluating. 

	@fun = true
	
	class WaterPark
		def fun?
			@fun
		end
	end
	
	waterpark = WaterPark.new
	waterpark.fun?
	
	>> NameError: undefined local variable or method `fun' for #<WaterPark:0x00000101b492e8>
	
What's that? We defined `@fun` right there! You probably guessed by now that we're in a different scope, a different "legal document," if you will, and that without being explicitly told, the program can't understand that both `@fun`s are the same. Well that's no fun--I don't want to start writing legal documents--I want to write code! Fortunately for us, it can be fairly easy to intuit when we've changed scope if we think about the question from the perspective of a language designer like Matz (the creator of Ruby).

If you were designing a programming language, you'd run into the same problem that you'd run into if you were designing a file system: naming collisions. Let's imagine a world where computer programs and file systems are one big blank sheet of paper. On that sheet of paper, we start to write down some names on the left side, with arrows pointing to the things they signify on the right side. On our file system, we have the names of files on the left, and the files they represent on the right. Pretty soon, however, we'd have two files that we wanted to name "cool_cat.jpg," one a photo a cat wearing sunglasses, and the other, a photo of a Louis Armstrong, a popular jazz musician. Without context, we end up having to make a compromise: name the second one "cool_cat2.jpg." When we return to our files, we end up opening both in the pursuit of the file we want--it's hard to remember which "cool cat" is which. 

The solution on a filesystem is directories--one named "Cat Photos" and the other named "Musician Photos." Now both can be named "cool_cat.jpg," and it's actually easier to remember which is which. A directory is an example of an abstract concept called  a "namespace" (a container filled with identifiers), and programming languages also have namespaces, or scopes, for the same reason. 

In a programming language, we have a similar problem. Two libraries want to define a class called `Text`, and one program wants to use both. How can it do so without causing confusion? The answer is, as in our directory example, is namespaces.

	module TextReader
		class Text
		end
	end
	
	module TextWriter
		class Text
		end
	end
	
	TextReader::Text.new
	
So now let's return to the original question at hand: If you were a programming language writer, which of the following would you say should be a new context, a blank slate in which names should be presumed to hold new meaning?

* A class definition
* A module definition
* A constant name
* A method definition
* A block (or its callable relatives, proc & lambda)
* A variable name
* A new file

Ruby responds to this question with the following answers:

* A class definition
* A module definition
* A method definition

Modules are the primarily namespacing tool in Ruby by convention. If you want to imply to other programmers that you want to group several concepts, the best way to do so in Ruby is with a module. The module is like the directory in our file system example, so it's a good choice to ensure it creates its own scope by default. As we'll see later, opening a module doesn't _have_ to create a new scope, and we have the tools to ensure it doesn't, just like we could say that "this addendum is a part of the same legal document, and everything declared before still applies." In fact, every choice about what "should" create a new context is just a "sane default" put in place by the creators and maintainers of Ruby, just like how by default we would assume that a new document is not an addendum to some previous document, and has its own context. 

You probably already guessed that a class definition creates a new context (as we saw above). A class is actually an instantiatable module--a module with a few additional methods that make it easy to create objects out of the namespace (`new`, `allocate`, and `superclass`). So it makes sense that if a module is a new context, so too is a class. This is both a convenience and a guide to writing good code; we assume when creating a class that the class will entirely manage its own concerns, and making classes create their own context by default also encourages this programming practice, which is easier to understand for new developers entering a project. 

It may surprise you to see methods included on our list, but not blocks. After all, both are callable things in Ruby; they're pretty similar, right? This choice probably derives from differences in that way methods and blocks are used. Methods are always bound to classes (even in the case when they're declared in the global namespace--in that case they're implicitly bound to the `Object` class), but procs exist on their own. Since they exist on their own, and code must run in some context, blocks inherit their surrounding context:

	def my_method
		x = "Goodbye"
		yield "cruel"
	end
	
	x = "Hello"
	my_method { |y| "#{x}, #{y} world" } # => "Hello, cruel world"
	
Here we see two important properties: blocks inherit their context (it saw x = "Hello" not x = "Goodbye"), and also that methods create their own context (if we didn't set x = "Hello," the block _would not_ have been able to find x = "Goodbye"). It wasn't a case of us writing a "more localized" context for `x`, it's that method definitions are in fact different contexts, as we've already described. Now that we've seen how this works, let's look at our other scope-creators: classes and modules:

	fun = true
	
	class Party
		if fun
			def fun?
				true
			end
		end
	end
	>> NameError: undefined local variable or method `fun' for Party:Class
	
In the context of our class, fun is not set, so we get an error. Likewise, if we try to evaluate a variable local to a class, module, or method from outside of that context, it won't be found:

	class Party
		fun = true
	end
	
	fun
	>> NameError: undefined local variable or method `fun' for main:Object
	
Simple enough. These each create new contexts, and outside of that context, we have no idea what `fun` means. 

## Self: The Current Context

So we've described why we need different contexts in order to create identifiers (variables, method names, and constants) by the same name, but _how does Ruby know what context it's currently evaluating?_ And also, how does Ruby know whether or not an identifier exists within the current context? The answer is an object called _self_, which Ruby sets automatically it keep track of the context. 

### Self & Instance Variables

Say for example, we have our `Party` class, and an instance named `party`. We want to know if this particular `party` is fun.

	class Party
		attr_accessor :fun
		
		def initialize
			@fun = true
		end
		
		def fun?
			@fun
		end
	end
	
	party = Party.new
	party.fun? # => true
	
When we call `party.fun?`, we want to send messages to `party`--to ask it questions about itself or ask it to perform work for us. Ruby notices that we've sent the `fun?` message to `party`, and it sets `party` to the object `self`. Inside the `fun?` method, all instance variables are presumed to belong to `self`. We can see this more plainly if we use `instance_eval`, which explicitly sets the value of `self` in a given block:

	jury_duty = Object.new
	jury_duty.instance_variable_set(:@fun, false)
	
	party = Object.new
	party.instance_variable_set(:@fun, true)
	
	def fun?
		@fun
	end
	
	jury_duty.instance_eval { fun? } # => false
	party.instance_eval { fun? }  # => true
	
Within the fun method, the `@fun` instance variable is presumed to belong to `self`, so we get different answers, depending on who's acting as `self`. If you're familiar with Javascript's `call` or `apply`, the idea is the same. Let's look at one final example to prove that methods evaluate instance variables in the context of `self`:

	class Party
		@fun = true
		
		def fun?
			@fun
		end
	end
	
	party = Party.new
	party.fun? # => nil
	
What gives in the example above? Classes are in fact just objects, and they can have instance variables, too. Since we haven't set the `@fun` instance variable within one of the class's instance methods, we've actually set `@fun` to `true` on the `Party` class. When we called the `fun?` method, its context was implicitly set to an instance of `Party`, and the `Party` instance doesn't have an `@fun` instance variable--its class does. To prove that the `@fun` instance variable belongs to the `Party` class, we can use `instance_variable_get`:

	Party.instance_variable_get(:@fun) # => true

### Self & Methods

`self` is also important for knowing who to send methods to. If a message does not have an explicit receiver, its receiver is presumed to be `self`:

	class Party
		def fun
			true
		end
	end
	
	class JuryDuty
		def fun
			false
		end
	end
	
	def fun?
		fun
	end
	
	party = Party.new
	jury_duty = JuryDuty.new
	
	party.instance_eval { fun? } # => true
	jury_duty.instance_eval { fun? } #=> false
	
Since the `fun?` method defines no explicit receiver for the `fun` message (it isn't `party.fun` or `jury_duty.fun`, just `fun`), Ruby attempts to send the message to the object acting as `self`. It's equivalent to writing the method this way:

	def fun?
		self.fun
	end

But we don't need to write it that way--the `self` part is implied. This is only true when we have no explicit receiver. When we have an explicit receiver, Ruby doesn't need to evaluate `self` in order to determine who the message should go to:

	class FunEvaluator
		def fun?
			"I think so"
		end
	end
	
	class Party
		def fun
			FunEvaluator.new
		end
		
		def fun?
			fun.fun?
		end
	end
	
	party = Party.new.fun? # => "I think so"
	
In the example above, we see that the message `fun?` gets sent to the result of the first call `fun`. Since the result is an instance of `FunEvaluator`, the answer is "I think so." If the message instead got sent to `self`, we would have ended up with a stack overflow, since the method would have kept calling itself, as we could see demonstrated below:

	class Party
		def fun?
			fun?
		end
	end
	
	party = Party.new.fun?
	>> SystemStackError: stack level too deep
	
## Flattening the Scope

We know about scopes, and we know about `self`, but what we may not know is that Ruby actually provides us with enough flexibility to ensure that we can work in whatever scopes we want. To review what we said before, the following create new scopes in Ruby:

* Class definitions
* Module definitions
* Method definitions

These three definition blocks make sense to create new scopes by default. A lot of the time, that's the behavior we want. But Ruby provides alternate means of performing each of these tasks via method calls, and method calls are not on that list. That means that we don't have to create new scopes if we don't want to:

	fun = true
	
	Party = Class.new do
		define_method "fun?" do
			fun
		end
	end
	
	party = Party.new
	party.fun? # => true
	
Almost miraculously, `fun?` just works. It has the correct context, because we didn't create any new scopes, even though we defined a new class and a new method. The trick is that we did so via _method calls_, not the _class_ or _def_ keywords. Of course, as Uncle Ben says, with great power comes great responsibility, and our responsibility in this case is to understand how scopes are created, and when we want to break this rule. We can illustrate some gotchas and also show a module definition method at the same time:

	fun = true
	
	Fun = Module.new do
		define_method "fun?" do
			fun
		end
	end
	
	class Party
		include Fun
	end
	
	party = Party.new
	party.fun # => true
	
	fun = false
	party.fun # => false
	
Since `fun` isn't encapsulated within the `Fun` module, it's easy to end up overwriting it, and thereby overwriting our other objects without meaning to. 

## Clean Rooms

The problem: We have a complex application, with many possible variables in scope. We want to load code into a new context, evaluate it, and avoid interfering with the rest of the application.

The solution: We create a new object whose sole purpose is to act as an environment in which to evaluate our blocks.

	Object.new.instance_eval do
		@sky_height = 100
		puts "Sky is falling" if @sky_height < 150
	end
	
Rather than create a new `@sky_height` variable in the parent context, or alter an existing variable, the clean room shields our code from the rest of the program, and the rest of the program from our code. 

If it seems weird to evaluate code "inside an object," prepare for a bit of a surprise: Ruby _always_ evaluates code in the context of an object. The global context is called `main`, and it's actually an instance of `Object`, the same class we instantiated to create a clean room for our blocks. Don't believe me? Fire up an `irb` session:

	self.class # => Object
	
	@fun = true
	self.instance_variable_get(:@fun) # => true
	
	def fun?
		@fun
	end
	
	self.send :fun? # => true
	
One potentially unexpected side effect of this truth is that, since methods are defined on classes and not objects, that defining a method in the global namespace actually makes it a private method of `Object`, and thus all classes in the program:

	def fun?
		true
	end
	
	class Party; end
	
	party = Party.new
	
	party.send :fun? # => true




