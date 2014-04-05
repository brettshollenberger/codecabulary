# Javascript Recipe Cheat Sheet

## Apply and Call

Functions are applied with `()`. They also have _methods_ for applying them to arguments. `call` and `apply` set the function context explicitly. 

	plus.apply(this, [2, 3])

## Slice

Arrays have a `slice` method. The function can always be found at `Array.prototype.slice`. It works like this:

	[1, 2, 3].slice(0)
  >> [1, 2, 3]

  [1, 2, 3].slice(1)
  >> [2, 3]

  [1, 2, 3, 4, 5].slice(1, 4)
  >> [2, 3, 4] // The 1st position up to but not including the 4th position

Note that slice always creates a new array, so `slice(0)` makes a copy of an array. The arguments pseudo-variable is not an array, but you can use `slice` with it like this to get an array of all or some of the arguments:

  var __slice = Array.prototype.slice;

  function argumenter() {
    var args = __slice.call(arguments, 0);
    for (var arg in args) {
      console.log(arg);
    }
  }

## Concat

Arrays have another useful method, `.concat`. Concat returns an array created by concatenating the receiver with its argument. 

  [1, 2, 3].concat([4, 5])
  >> [1, 2, 3, 4, 5]

Note that `concat` also creates a new array, regardless of whether or not there's anything to be concatentated:

  [1, 2, 3].concat();
  >> [1, 2, 3];

  var a = [1, 2, 3];

  a.concat() == a;
  >> false

## Function Lengths

Functions have a `length` property that counts the number of arguments declared:

  function(a, b, c) {}.length
  >> 3

This is most often used when overloading a function with abilities based on the arguments passed to it. 
