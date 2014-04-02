# Composition

One of the most basic building blocks of computer programs is composition:

	function cookAndEat(food) {
		return eat(cook(food));
	}
	
	function compose(a, b) {
		return function(c) {
			return a(b(c));
		}
	}
	
	var cookAndEat = compose(eat, cook);
	
If that's all there was to it, composition wouldn't matter much. But like many patterns, using it when it applies is only 20% of the benefit. The other 80% comes from organizing your code such that you can use it: Writing functions that can be composed in various ways.