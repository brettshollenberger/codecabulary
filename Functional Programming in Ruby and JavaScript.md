# Functional Programming in Ruby and Javascript

Developers new to Javascript often deride it as being quite unlike what they're used to, and in terms of object-orientation at least, this is a well-made point. But one of the cool benefits of working with Ruby and Javascript is that they're both strongly support functional programming techniques, which in Ruby are often considered "advanced" techniques, but in Javascript are a staple of the language. 

Ruby uses procs, blocks, and lambdas to support flexible, dynamic functional programming:

	%w("the rain in spain").map { |word| word.capitalize } 
	
By adding a few quick functions to Javascript's library, we can perform the same quick one-liner in Javascript:

	function map(array, funk) {
	  returnArray = [];
	  for (i = 0; i < array.length; i++) {
	    returnArray.push(funk(array[i]);
	  };
	  return returnArray;
	};
	
	function capitalize(str) {
	  return str[0].toUpperCase() + str.slice(1);
	};
	
	map("the rain in spain".split(" "), function(word) { return capitalize(word) });
	
In this way, anonymous functions in Javascript work exactly the same way as blocks or lambdas in Javascript, and can be manipulated on the fly to create highly flexible scripts.
