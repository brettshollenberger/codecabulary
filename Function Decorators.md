# Function Decorators

Function decorators are higher-order functions that take one function as an argument, and return another function. The returned function is a variation on the argument function.

## Tap (K Combinator, Kestrel)

One of the most basic combinators is the K Combinator, or the "kestrel:"

  function K(x) {
    return function(y) {
      return x;
    }
  };

It has some surprising applications. One is when you want to do something with a value for side-effects, but keep the value around:

  function tap(value) {
    return function(fn) {
      if (typeof(fn) === 'function') {
        fn(value);
      }
      return value;
    }
  }

## Maybe

A common problem in programming is checking for `null` or `undefined` (hereafter called "nothing," while all other values including 0, [], and false will be called "something"). Languages like Javascript do not strongly enforce the notion that a particular variable or particular property be something, so programs are often written to account for values that may be nothing.

This recipe concerns a pattern that is very common: A function `fn` takes a value as a parameter, and its behavior is designed to do nothing if the parameter is nothing.

  function isSomething(value) {
    return value !== null && value !== void 0;
  }

  function checksForSomething(value) {
    if (isSomething(value)) {
      console.log("Rejoice!");
    }
  }

Alternately, the function may be intended to work with any value, but the code calling the function wishes to emulate the behavior of doing nothing by design when given nothing:

  var something = isSomething(value) ? doesntCheckForSomething(value) : value;

## Unary Decorator

There's a common problem in Javascript's `map` method: It calls each function passed to it with _three_ arguments: the element, the index, of the element in the array, and the array itself. 

  [1, 2, 3].map(function(element, index, array) {
    console.log({element: element, index: index, array: array});
  });

  >> { element: 1, index: 0, array: [1, 2, 3] }
  >> { element: 2, index: 1, array: [1, 2, 3] }
  >> { element: 3, index: 2, array: [1, 2, 3] }

If you pass in a function that requires only one argument, it simply ignores additional arguments. They aren't necessary. But some functions have an optional second or even third arguments, for example:

  ['1', '2', '3'].map(parseInt)
  >> [1, NaN, NaN]

This example won't work because `parseInt` is defined as `parseInt(string[, radix])`. It takes an optional radix argument.  What's '3' in base 2? Nothing. It doesn't make sense. So we get `NaN`. Instead, we want to ensure that our parseInt function only accepts a single argument when we're mapping (and we can do this for similar map-related issues). Enter unary decorator:

  function unary(fn) {
    var fnLength = fn.length;

    if (fnLength == 1) {
      return fn;
    } else {
      return function(first) {
        return fn.call(this, first);
      }
    }
  }

Now we can ensure parseInt operates on only a single argument:

  ['1', '2', '3'].map(unary(parseInt));
  >> [1, 2, 3]

And we finally get what we expect.
