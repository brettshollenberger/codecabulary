# Barewords

In Ruby, there are lots of fancy ways of referring to data--constants, global variables, local variables, instance variables, class methods, instance methods; the list goes on. Most of these modes of reference are decorated-- `$global_var` or `CONSTANT`--for example. That means that if we change the _way_ we refer to data, we have to change each reference to that data, or we'll introduce bugs.

_Barewords_ on the other hand, are undecorated data references--`local_var` and `method_call`--for instance. Their lack of decoration adds an inherent flexibility; if we decide to change their implementation later, we can implement them as another form of bareword and save ourselves from bricking our implementation. Barewords also happen to be much more readable.

#### Types of Barewords

In Ruby, there are three types that are called as barewords:

* Local variables

	
		def salutation
			greeting = "Hello "
			puts greeting + "Brett"
		end
	
	
* Method parameters

		def salutation(greeting)
			puts greeting + " Brett"
		end
		
		salutation("Hello")
		

* Parameterless methods

		def greeting; "Hello "; end
		
		def salutation(greeting=greeting)
			puts greeting + "Brett"
		end
		
		salutation
		>> "Hello Brett"
		
With these three types, we can and should replace nearly any other implementation, as long as we know a bit more about how Ruby works. 

For instance, we know that methods defined on the _main_ object have the magical quality of being added as `private` methods to all other objects, and therefore we can replace global variables with methods defined on the main object:

	$global_var = "Accessible anywhere, but decorated."  # becomes:
	def global_var; "Accessible anywhere, and undecorated."; end
	
#### Gotchas

The primary gotcha I see with barewords is the propensity for collisions. Consider:

	def greeting; "Hello "; end
	
	def salutation(greeting)
		greeting = "Hola "
		puts greeting + "Brett"
	end
	
	salutation "Sup "
	
Which of the forms of greeting gets called in the implementation above? Of course it's the local variable; in each case, it's easy to see which will attain precedence by proximity or order of definition. Just be aware that not only can these types of collisions occur, they can also make it hard to root out the definition of a particular bareword for users new to your codebase. In all, barewords will save your company time and money by keeping you from having to dig up the origins of bugs introduced by changing implementations, and I'd still highly recommend using them. Barewords allow your methods to concern themselves with how they work, not where they get their data.
	
		
		