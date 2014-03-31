# Partial Application

Partial application is a concept that's related to function currying. Function currying is a form of refactoring where the developer reduces the number of arguments required for a given function to one by splitting the function into separate, often nested functions. For example, a function with three arguments, like this one: 

  function example(a, b, c) {
    return a + b + c;
  }

Would be split into three nested functions:

  function example(a) {
    return function(b) {
      return function(c) {
        return a + " " + b + " " + c;
      }
    }
  }

In general, we can say that function currying refers to refactoring a method that accepts `n` arguments into `n` functions that accept 1 argument each.

Instead of calling the function with three arguments, we'd call the first function, which itself would return a function. 

  var a = example("Hello");
  a
  >> [Function]

We'd continue calling these functions until we reached the final function, which returned `a + b + c`.

  var b = a("there");
  b
  >> [Function]
  b("world");
  >> "Hello there world"

One of the benefits of currying is that it allows us to pre-fill certain arguments in order to make different functions. Pre-filling these arguments is called _partial application_:

  # Ruby Example of Partial Application:
  apply_math = proc do |method, a, b, *args|
    current_a = a.send(method, b)
    args.each { |arg| current_a = current_a.send(method, arg) }
    current_a
  end

  add = apply_math.curry.(:+)
  
  add.(1, 2, 3, 4, 5)
  >> 15

  increment = add.curry.(1)
  
  increment.(7)
  >> 8

As you can see from the example above, not only can partial application provide us with DRYer code by allowing us to perform similar tasks in generalized ways (we could just as easily add subtraction, multiplication, and division based off of our `apply_math` base), but we can also continue currying functions based off of previously curried functions (e.g. we base increment off of `add` by filling in the next argument). 

## Javascript Tools

Currying and partial application are such common tools that many libraries provide some form of partial application tool. Lodash provides a nifty one:

  var add = _.curry(function(a, b) {
    var answer = a + b;
    var args   = Array.prototype.slice.call(arguments, 2);
    _.each(args, function(arg) { answer += arg; });
    return answer;
  });

  add(1, 2, 3);
  >> 6

  add(1)(2);
  >> 3

  var increment = add(1);

  increment(10);
  >> 11

Here we've both curried a function and partially applied arguments using Lodash's built-in `curry` method. What's more is that we can continue to call our curried function like a traditional function, which makes it quite versatile.

## Rolling Our Own Partial Application Functions:

These tools will allow us to partially apply a curried (or un-curried) function in either the first or last position:

  var __slice = Array.prototype.slice;

  function callFirst(fn, arg) {
    return function() {
      var args = __slice.call(arguments);
      return fn.apply(this, [arg].concat(args));
    }
  }

  function callLast(fn, arg) {
    return function() {
      var args = __slice.call(arguments);
      return fn.apply(this, args.concat([arg]));
    }
  }

  var increment = callFirst(add, 1);
  var decrement = callLast(subtract, 1);

  increment(1);
  >> 2

  decrement(2);
  >> 1

A building block of programming is _partial application_ of a curried function. When a function takes multiple arguments, we "apply" the function to the arguments by evaluating it with all of the arguments, producing a value. But what if we only supply some of the arguments? In that case, we can't get the final value, but we can get a function that represents a _part_ of our application. 

	# Partially applying a map:
	
	function square(array) {
		return _.map(array, function(n) { return n*n; });
	}
	
But why write a function every time we want to partially apply a function to map?

	function mapWith(fn) {
		return function(array) {
			return _.map(array, fn);
		}
	}
	
	var square = mapWith(function(n) { return n * n });