# Pure Function

One of the foundations of functional programming is pure functions--pure functions everywhere, all the time, for everyone! So what's a "pure" function? And why should you care? 

Pure functions:

1) Given the same parameters always evaluate to the same result. 
2) Have no side-effects (they do not alter any variables)

Okay, that sounds like it might be valuable, but it's actually super valuable. You see, programs composed entirely of pure functions are "referentially transparent," which is a fancy way of saying that you could replace the function with the value it produces, and still have the exact same program. It turns out referential transparency has some crazy good benefits:

1) Lazy evaluation, which as you might know, leads us to be able to construct:
2) Infinite data structures -- streams and the like are possible and efficient
3) Parallelization - Since any computer anywhere any time could evaluate the function and produce the same result, and since the function doesn't rely on or alter any shared context, you could distribute jobs among any number of computers, which turns out to be the way we're primarily scaling nowadays (horizontally)
4) Memoization - Which seems like a minor benefit in relation to the others, but is nonetheless pretty nice. Since you can replace a function call with its result, and that's what memoization is, you of course can put your money where your mouth is and actually memoize. 