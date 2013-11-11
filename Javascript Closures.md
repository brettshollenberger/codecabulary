# Closures in Javascript

Lots of Javascript developers get by without ever understanding the concept of closures--but they use them every day without realizing it. Closures are the scope created by functions (all Javascript functions create a scope) that allows the function to access and manipulate external variables. Variables created within the function's scope are local to that function, and cannot be referenced by parent scopes, which can allow us to mimic the concept of private variables, even though Javascript doesn't give us private variables as a distinct feature of the language. Another nice features of closures is that they will keep the data they need from being garbage collected. They act as a type of protective bubble that keeps the variables they require in scope as long as they themselves are a part of our application.

So what exactly does a closure have access to?

* Every variable in its encompassing scopes. Variables and functions declared on the global scope? Obviously accessible. Variables declared in the root scope of the same module? Accessible. Any scope which surrounds the scope in question is accessible, whether or not the variables are declared after the function itself. This is an important distinction, because variables declared in the same scope are not forward referencable. For example:
	
	var a = b;
	> ReferenceError: b is not defined
	
	var a;
	function bGetter() { a = b; }
	b = 1;
	bGetter();
	a
	> 1
	
While it may be obvious that the example above will work because `bGetter` doesn't run until we have a value for `b`, the reason this works is because closures have access to the variables in their parent scopes, even those that are declared after they themselves are. This allows us to assign a to b without b being yet defined. One bit of magic in the court for closures.

* Function parameters. Another perhaps obvious piece of a function's scope is the set of parameters passed to it. It's probably going to need those.

Closures do not have access to:

* Forward referencable variables within the same scope. We saw an example of that above.

#### Using Closures to Mimic Private Variables

Javascript doesn't have the notion of private variables--variables that can only be accessed from within the class itself, and aren't exposed as a part of the API, but which are still necessary for maintaining state while the API performs work behind the scenes. However, Javascript's closures are a common means of providing the same functionality:

	function Cat() {
		var meows = 0;
		this.meow  = function() {
			meows++;
		}
		this.countMeows = function() {
			return meows;
		}
	}
	
In our constructor function, we create a private variable meows. This variable wouldn't normally exist for long; after our constructor function exits, it would cease to exist unless it were somehow maintained by a closure, and in this case, it is. Both `this.meow` and `this.countMeows` maintain a reference to the "private" variable `meows`, which keeps the variable from being garbage collected. Since both functions require the variable `meow` to operate correctly (and not be force to throw a reference error) they maintain a reference to `meows` by wrapping it in their closure. The closure creates a protective bubble that ensures the variable can be called at a later time, even if its own scope has finished executing.

