# Haskell List Comprehensions

List comprehensions in Haskell will be familiar to those with a math background, and for those without, they're quite simple.

A set is a list of distinct (e.g. non-repeating) items. You can get really specific about defining your set:

The set of natural numbers (aka integers, counting numbers) "such that" a < b < c <= 10.

The "such that" syntax is where we define and filter our sets. We use a pipe (`|`) to define it:

```
[ (a, b, c) | a <- [1..10], b <- [1..10], c <- [1..10], a < b, b < c]
```

The syntax above reads: the set `(a, b, c)` such that `|` a, b, and c could be in the range `1..10`, where `a < b`, and `b < c`. Set comprehensions illustrate a common pattern in functional programming: start with a set of candidate solutions, and filter them down until a solution is reached.

For instance, we can use this syntax to find a right triangle where:

* The lengths of the three sides are all integers
* The length of each side is less than or equal to 10
* The triangle's perimeter (the combined lengths of its sides) is 24
* a^2 + b^2 = c^2 (definition of a right triangle repeated for clarity)

This is really easy in Haskell--we're so close in the previous example we should be able to taste it:

```
[ (a, b, c) | a <- [1..10], b <- [1..10], c <- [1..10], a < b, a+b+c == 24, a^2 + b^2 == c^2]

>> [(6,8,10)]
```

