# Javascript Type Checking

Javascript is a dynamically typed language, which means that at times we'll want to be certain that objects are of a given type before proceeding with a function. 

#### Foolproof Type Checking

The `typeof` operator doesn't always provide the response expected; `variable.constructor` usually works better:

| variable   | typeof var  | var.constructor  |
| ------------- |-------------| -----|
| {an: 'object'}| object | Object |
| ['an', 'array'] | object | Array |
| "a string"    | string   | String |
| 5                | number | Number |
| function() {} | function | Function |
| true            | boolean  | Boolean |
| new User() | object     | User      |

#### Strict Type Checking

We can also write functions that strictly check all arguments passed in meet the expected types, and that the correct number of arguments is passed:

	function strict(types, args) {
		if (types.length != args.length) {
			throw "Invalid number of arguments. Expected " + types.length + "; received " + args.length + " instead";
		}
		for (var i = 0, l = args.length; i < l; i++) {
			if (args[i].constructor != types[i]) {
				throw "Invalid argument type. Expected " + types[i].name + "; received " + args[i].constructor.name + " instead.";
			}
		}
	}
	
And use like so:

	function listUsers(prefix, num, users) {
		strict([String, Number, Array], arguments);
		
		for (var i = 0; i < num; i++) {
			console.log(prefix + ": " + users[i]);
		}
	}
	
Type checking can improve the flexibility of methods you write to better adapt to the developers that use your code.


