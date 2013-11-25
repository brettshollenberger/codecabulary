# Javascript Scope

In computer programs, we use identifiers (names that refer to parts of the application--variables, keywords, functions, etc), and scopes to limit the portion of the program that a given identifier exists in. A variable by the same name may refer to two separate objects in two different namespaces, and this is convenient because it allows us to set names that make sense without worrying about whether or not a name has already been used in some other namespace.

Scopes are created in different ways in different languages. In Javascript, they are only created by functions. At the time a function is declared, Javascript gathers all of identifiers that are within the function's scope into a private attribute called `[[scope]]` that isn't available to Javascript developers--it's all under the hood. `[[scope]]` actually refers to a scope chain--a collection whose members are individual scopes. Check out the following example:

	var currentScope = 0;
	
	(function() {
		var currentScope = 1, one = 'scope one';
		(function() {
			var currentScope = 2, two = 'scope two';
			(function() {
				var currentScope = 3, three = 'scope three';
				console.log(one, two, three);
				console.log(currentScope);
			})();
		})();
	})();
	
	>> 'scope one scope two scope three'
	>> 3
	
In this example, the scope chain for the inner-most function would start with the scope created the function itself, and then work its way back to the global scope. Each scope in the scope chain is represented by a variable object--an object containing key-value pairs for each variable contained within the scope, and its associated value within that scope. Identifier resolution begins at the top of the scope chain (in the zero position of the scope chain collection), and if an identifier is found within that scope, the value is resolved and lookup stops. 

#### Execution Context

When a function is called, it creates another function property--the execution context. The execution context is an object that also contains a scope chain--which it initiates from the function's `[[scope]]]` attribute. The function then creates an `activation object`, which contains the value of `this`, the arguments collection, named arguments, and all local variables for the given execution of the function. The activation object is then pushed to the front of the scope chain, so that identifier resolution begins with the activation object, and then proceeds backwards.

#### Dynamic Scopes

In optimized Javascript engines, like Chrome's V8 and Safari's Nitro, identifiers are cached for faster resolution, meaning that in general the speed concerns associated with the hash-based lookup of the scope chain is avoided. However, dynamic scopes force hash-based look-up, since identifiers cannot be resolve through static resolution. 

Dynamic scopes are created through the use of `eval` statements, which can execute arbitrary code at the time the function is called. Since any given identifier could be manipulated at run time, cached resolution will be invalidated by dynamic scoping, and should be avoided for high performance code.

#### Closures and Memory

	function assignEvents() {
		
		var id = 01231;
		
		document.getElementById('save-btn').onclick = function() {
			saveDocument(id);
		}
	};
	
The above function creates an event handler for a button, which is able to remember the `id` variable via a closure created by the function. The closure object maintains a reference to the activation object created by the function, which contains the `id` property. Normally, after a function has finished executing, its activation object is destroyed when its execution object is destroyed--allowing them both to be garbage collected. However, since the closure maintains a reference to the activation object for later lookup, the activation object cannot be garbage collected. This means that closures require more memory overhead than nonclosure functions. Performance concerns can be minimized by storing out-of-scope variables in local variables within the function.



In computer science, a scope is a part of a computer program in which a given identifier refers to a given entity, or in which the identifier even exists.