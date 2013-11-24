# Closures in Javascript

	function User(properties) {
		for (var i in properties) {
			(function() {
				var local = i;
				this['get' + local] = function() { return properties[local]; };
				this['set' + local] = function(val) { properties[local] = val; };
			}).call(this);
		}
	}
	
	var user = {name: 'brett', age: 24}
	
	var brett = new User(user);
	
In the example above, we create a number of privileged methods--methods that get or set the values of private variables. `brett` doesn't have access to get or set `name` or `age` directly, but it does have access to `getname`, `setname`, `getage`, and `setage`. So why the IIFE (immediately-invoked function expression) in the middle of the code? Isn't it a bit confusing? Is it even necessary?

The reason we create the IIFE is to create a closure--an encapsulated bubble in which our private variables live. Although we define our functions like `getname`, and then expect the code to cease executing, we couldn't write the getters above simply as `return properties[i]`. That's because blocks (like for statements) don't create scopes in Javascript, and the value of `i` changes each time we run through the loop. You already knew that, but what you might not have known is that, when our functions are called dynamically by the user, the code within the function is evaluated each time to return a value, and functions remember the values of the local variables within their scope. Since the `for block` doesn't create a new scope, only one scope has been created (the scope of the User function itself). That means that the value of `i` is currently the last value that ran through the loop (`age`), and `all` functions evaluate the latest value of `i`, will attempt to get or set age. 

The IIFE creates a closure with a private variable (`local`), which is the value of `i` during that iteration of the `for` loop. Each getter/setter pair gets its own private bubble (its closure), and so it gets its own private value for `local`, which it can remember via the closure. The closure won't go away unless we remove all public properties (the getters and setters) that needed to reference private values in the closure. If we did delete all public properties, the closure would be garbage collected, saving us from any fear of random private values floating around with nothing needing to reference them.  

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

