# Dominance Relations

When we describe orders of growth (how quickly processes grow, usually based on their inputs), we group a lot of complexity together into a few basic classes of functions that scale in a particular way. Even though a function might perform `2n` basic operations, and other function may use `2000n` basic operations, we reduce the problem to the class `O(n)`, noting that there is some constant `c` that can be multiplied by `n` to reliably provide an upper bound.

The reasons we reduce the complexity of this problem's order of growth to a specific class are two-fold: 1) It's easier to understand quickly; we know what it's like to have an `O(n)` algorithm, and what it's like to have an `O(n^2)` algorithm. We know what generalizations can be made about these types of procedures. 2) These rates consume the lion's share of resources in the problems we're concerned with (those with very large inputs). We don't really care how a procedure scales when there are 6 items to sort in a list. But we do care when there are 6,000,000 items to sort. Then an `O(n^2)` algorithm will use 3.6e+13 basic operations--certainly much more than `2000n`. Such an operation will be untenable at this size.

The way we describe the relationship between these different classes of functions is through _dominance relations_. We say that a faster-growing function _dominates_ a slower-growing one. For instance, functions that grow at a rate of `O(n^2)` dominate functions that grow at a rate of `O(n)` (or `fn1 >> fn2`). 

## The Most Common Function Classes

1) Constant functions `f(n) = 1`: These functions always take the same, predictable number of basic operations to complete. Adding two numbers together, or printing out the lyrics to "Back in Black" are examples of such functions.

2) Logarithmic Functions `f(n) = log n`: Logarithmic functions are those that reduce the size of the input by a factor each time (for instance, dividing the length of a list in two). They are said to be logarithmic because the number of steps required is equal to the logarithm of some base to `n`. Which base exactly? It depends on the function. For instance, our function that divides a list in two is base 2, since half as many items need to be processed in the next iteration. That means a list of 100,000 gets cut into 50,000, 25,000, 12,500, 62,000, 31,000, 15,500, and so on. Since `log base 2 of 100,000` is less than 20, the number of steps grows refreshingly slowly compared to a linear function (one that grows at a rate of `O(n)`, which would take 100,000 steps to complete). 

3) Linear Functions `f(n) = n`: Such functions measure the cost of looking at each item once, or twice, or a hundred times. Again, we lop of the constant, since it's less important than the fact that we'll need to look at each item for large `n`.

4) Superlinear functions `f(n) = n lg n`: These functions arise in sorting algorithms such as quicksort and mergesort. They grow a little faster than linear functions, by just enough to represent their own dominance class.

5) Quadratic Functions `f(n) = n^2`: Measures the cost of looking at most or all pairs of items. These grow very quickly compared to linear functions.

6) Cubic functions `f(n) = n^3`: You can guess what these do

7) Exponential functions `f(n) = c^n` for a given constant `c>1`: Functions like these arise when enumerating on all subsets of `n` items.

8) Factorial functions  `f(n) = n!`: Functions like this arise when generating all permutations or orderings of `n` items

Put together, this information tells us that `n! >> c^n >> n^3 >> n^2 >> n lg n >> n >> log n >> 1