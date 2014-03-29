# Javascript Function Basics

When we invoke a function, we create a new environment, or an execution context. An environment is a (possibly empty) dictionary that maps variables to values by name. Arguments, variables, and functions ar e expressions that can be evaluated by looking up their value in the environment. 

Let's take a look at this new environment being created:

	(function (x) { return x; })(2);

As Javascript evaluates this function, it binds the argument 2 to the variable 'x' in the environment. The environment looks like this: `{x: 2}`, a dictionary where the variable `x` is bound to 2. When the body of the function runs, the expression 'x' is evaluated within the environment we just created, and is determined to be 2. And that's the result of this function. 

## Call by Sharing

The process above is called "call by value," a common evaluation strategy in programming languages, in which the values of expressions are determined before they are applied to the function. For example, we could have rewritten the function in the same way:

	(function (x) { return x; })(1+1);
	
In this case, `1+1` would be evaluated, then the result would be assigned to `x` in the dictionary, and so the result would be the same.

When Javascript binds names to values, like the name `x` to the value `2`, it makes a copy of the value and places the copy in the environment, assigned to the proper name. Value types like strings and numbers are identical to one another if they have the same value. That means that Javascript can make as many copies of strings, numbers, and booleans as it wishes. 

Reference types, like objects and arrays, are never copied, and so they are not copied when applied to a function. Instead, Javascript places references to reference types in environments, and when the value needs to be used, Javascript uses the reference to obtain the original. Because many references can share the same value, and because Javascript passes references as arguments, Javascript can be said to implement "call by sharing" semantics. 

An important distinction between call by value and call by sharing is that functions that operate on values via sharing can have side-effects in the program. Take for example, the following functions:

	var a = 1;
	
	function assignByValue(n) {
		n = 15;
	}
	
	function addByValue(n) {
		return ++n;
	}
	
	assignByValue(a);
	addByValue(a);
	
	a
	>> 1
	
With value types, no matter what happens in the body of the function, the variables passed into the body of the function won't be impacted. That's because each function makes a copy of the value, and assigns it to a variable in its environment. The function has no concept of the original variable. 

Functions that operate on reference types (by sharing), however, can alter the variables passed into them, because they each have a reference to the original object:

	var a = {val: 'one', num: 1}
	
	function assignByReference(n) {
		n.val = "edited";
	}
	
	function addByReference(n) {
		++n.num;
	}
	
	assignByReference(a);
	addByReference(a);
	
	a
	>> { val: "edited", num: 2 }
	
## Closures and Free Variables

Variables that are not bound within the function's dictionary are said to be "free variables," or "non-local variables." Let's take a look:

	function rememberer(x) {
		return function(y) {
			return x;
		}
	}
	
	var cool = rememberer("cool");
	
	cool("dog");
	>> "cool"
	
	cool(1);
	>> "cool"
	
The new function returned by the rememberer function remembers the value for `x` that was originally passed in, long after the rememberer function itself finishes executing. But how exactly does this work? When the function returned runs, each time it creates a new environment that looks something like: `{ y: "dog" }`, but there's no mention of `x` there. Or is there? 

The variable `x` is remembered through a closure, the environment created by its surrounding function. The environment for the returned function actually looks something like: `{ y: 'dog', '..': { x: 'cool' } }`. `'..'` means something like "parent" or "enclosure", or "super-environment." It's the parent function's environment, and when the variable `x` isn't found during evaluation in the child's environment, lookup proceeds to the parent environment. 

	
	