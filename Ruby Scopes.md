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
	
What's that? We defined `@fun` right there! You probably guessed by now that we're in a different scope, a different "legal document," if you will, and that without being explicitly told, the program can't understand that both `@fun`s are the same. 