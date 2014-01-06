# Closed Form

In computer science, we often want our algorithms to be in "closed-form" because they're much faster to compute than the alternative, "recursive" functions. Recursive procedures are those that call themselves, and recursive algorithms calculate their value in relation to previous values.

Consider the definition of factorial(n) (the function that computes the factorial of an integer, `n`): The product of n and all integers below it. 

Since `factorial(4) = 4 * 3 * 2 * 1` and `factorial(3) = 3 * 2 * 1`, and `factorial(2) = 2 * 1`, it's easy to generalize if you notice that any quantity factorial(n) can be calculated by:

	n * factorial(n-1)
	
So long as the edge case `factorial(1)` is described as equaling one. This mathematical property translates directly into a recursive algorithm:

	def factorial(n)
		return 1 if n <= 1
		n * factorial(n-1)
	end
	
Recursive algorithms are easy to come up with, because they usually translate 1:1 from their mathematical definitions. But what happens when we call the function factorial(5)? Its results depend on the results from factorial(4), which depend on the results from factorial(3), which depend on the results from factorial(2), until we get to factorial(1), which can be solved intrinsically:

	factorial(5)
	n * factorial(4)
	n * n-1 * factorial(3)
	n * n-1 * n-2 * factorial(2)
	n * n-1 * n-2 * n-3 * factorial(1)
	n * n-1 * n-2 * n-3 * 1
	n * n-1 * n-2 * 2 * 1
	n * n-1 * 3 * 2
	n * 4 * 6
	n * 24
	120
	
Notice both the shape of the process (a pyramid), and the number of lines (10, or roughly 2n). The number of lines is equivalent to the amount of time it takes the process to complete--the more lines, the longer the process will take. I don't think you'd need to see another example to agree that the amount of time required by this recursive algorithm scales linearly with the size of `n`. If we compute factorial(100), we're looking at a 200 line process. This linear order of growth is so common that it has a special name and notation: Big O of n, `O(n)`. 

I also pointed out the shape of the process. Notice how it scales out, before scaling back in. This is equivalent to the amount of memory, or space consumed by the process. Before `factorial(5)` can be solved, it must remember the value of `n * n-1 * n-2...` and so on, until it is able to solve for those intermediate values. It needs to save placeholders to come back to, and remember its way back. If the process shuts down, there will be no way to recover the information stored in memory, and the process will be lost. In terms of space, this process also scales at `O(n)`. Compare this definition for a factorial with an iterative model:

	def factorial(n)
		fact_iter(1, n)
	end
	
	def fact_iter(answer, count)
		return answer if count <= 1
		fact_iter(answer*count, count)
	end
	
The shape of this process evolves quite differently:

	factorial(5)
	fact_iter(1, 5)
	fact_iter(5, 4)
	fact_iter(20, 3)
	fact_iter(60, 2)
	fact_iter(120, 1)
	120
	
Although `fact_iter` continues to scale linearly with `n` in terms of time, it does not scale linearly in terms of space. Its space is constant (it is always invoking one process, with no memory of previous processes). If the process were shut down at fact_iter(60, 2), those two variables would be enough to complete the process with the same result; this crucial information is not being stored in memory. We have another special name for values that scale constantly (not in relation to `n`): Big O of 1 `O(1)`. So this process, as a whole, scales:

	Time: O(n)
	Space: O(1)
	
Which is an improvement over the initial process, which scaled in time and space at `O(n)`. The improvement made by `fact_iter` may seem maddeningly confusing, because common languages like C (and thereby any languages built on C), do not allow for the interpreter efficiency described by the `fact_iter` process above. In these languages, all recursive procedures (procedures that call themselves) are also recursive processes (processes evolve by building up a chain of deferred operations, and that thereby scale in space at at least `O(n)`). 

#### Okay, now for the closed form stuff:

Each of the recursive procedures described above scales in terms of time by at least `O(n)`, and for large quantities of `n`, it's likely that at some point, our users and our bosses are going to want something better.

Enter closed form. Closed form functions do not refer to their values in terms of their previously computed values. A technical definition is:

"An expression for a quantity f(n) that uses a fixed number of "well-known" standard operations independent of n."

There are a few key parts to that sentence:

* We already know what it means to compute a value independent of `n`. Instead of factorial referring to previous values of `n`, we want a function that does not need to invoke a chain of additional functions to achieve its result. That's where the huge efficiency comes in.

* What does it mean to be a "well-known" operation?

Well-known operations include: 

1) Constants (a number with some special significance, like pi and phi); 
2) Single variables (like n itself). That means that:
	
2n + 1
	
Is a valid closed-form expression. These variables are not, however, used to reference previous values (as in a recurrence relation):

2<sub>(n-1)</sub> + 1

Is not a valid closed-form expression. It references a previous function result in order to compute its current result.

3) Elementary operations like addition, subtraction, multiplication, and division

4) nth roots; 

5) Exponents

6) Logarithms. 

Basically, the things you know how to do already. That makes it easy, but it also doesn't make it easy to write. While recursive functions often write themselves based on the mathematical definition, closed-form functions require a bit of legwork and a bit of luck.
