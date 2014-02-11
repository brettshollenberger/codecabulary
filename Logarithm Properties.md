# Logarithm Properties

Three logarithm bases play a particularly important role in the world of computer science:

## Base 2

The binary logarithm, usually denoted `lg x`, is a base 2 logarithm. This logarithm appears in algorithms that repeatedly halve (binary search) or double (trees) their inputs. Most algorithmic applications of logarithms employ binary logarithms.

## Base e

The _natural log_, usually denoted `ln x`, is a base `e` logarithm (`e = 2.71828...`). The inverse of `ln x` is the exponential function `exp(x) = e^x`. Thus, composing these functions gives us:

	exp(ln x) = x
	
## Base 10

The common logarithm, base-10, is usually denoted `log x`. This base was employed in slide rules and logarithm books in the days before calculators, and is less widely used in algorithms today. 

## Properties

Logarithms properties arise out of the properties for exponents, as logarithms are often considered the "inverse" of exponents. What mathematicians really mean by that is that logarithms and exponents are complimentary concepts that approach the idea of exponential growth in different ways; the exponent computes the value of some base after a given duration or change, say `2^3` is the base 2 doubled 3 times. Logarithms, instead, compute the time it will take to reach a certain growth, so log<sub>2</sub>8 = 3. 

Exponents and logarithms are related functions for approaching growth, and so it makes sense that they would feature the same properties--these properties are inherent to the problem of exponential growth. 

### Composing and Decomposing Growth Functions

When we multiply two exponentials with a common base, we can add the exponents together to give us the answer. Take for example:

	8 * 8 = 64
	
We could also have solved this problem by noticing:

	8 = 2^3
	2^3 * 2^3 = 2^6 = 64
	
This makes sense when we think about it. An exponent, when we break it down, is just repeated multiplication, so what we're really saying is:

	(2 * 2 * 2) * (2 * 2 * 2) = 2^6
	
Logarithms are another way of looking at the same growth function, where the quantity we're interested in is time (the exponent):

log<sub>2</sub>64 = 6

We could similarly decompose this logarithm to the problem from above:

log<sub>2</sub>64 = log<sub>2</sub>(8*8) = log<sub>2</sub>(8) + log<sub>2</sub>(8) = 3 + 3 = 6

Again, this makes sense. We're simply breaking the problem down into smaller component parts that continue to add up to the same larger problem. 

Thus, in general:

log<sub>b</sub>(xy) = log<sub>b</sub>(x) + log<sub>b</sub>(y)

### Changing Bases

Changing the base of an algorithm is easy:

log<sub>a</sub>b = log<sub>c</sub>b/log<sub>c</sub>a

#### Implications

The implications of these logarithm properties are more important to algorithm writers:

1) The base of the logarithm has trivial impact on the rate of growth. We care about the rate of growth for very large `n`, and in comparison to linear or super linear functions, logarithmic growth will always be trivial. log<sub>2</sub>(1,000,000) = 20 while log<sub>100</sub>(1,000,000) = 3. In algorithmic terms, this means that a change in the size of the base has little bearing on the growth of the process.

2) Logarithms cut any function down to size. The growth rate of the logarithm of any polynomial function is O(lg n), because:

log<sub>a</sub>n<sup>b</sup> = b * log<sub>a</sub>n

This is an incredibly powerful observation, and it's the reason why binary search finds applications on a wide range of problems. Performing a binary search on `n^2` sorted items requires only twice as many comparisons as a binary search on `n` items. 

Take for example, a binary search that does not halve its input each time. Instead, it divides the problem into chunks of 1/3 the size and 2/3 the size of the original problem. It discards the 1/3 chunk, and the 2/3 chunk remains. How effective is binary search now?

Well at 1,000,000 items, base 2 will solve the problem in 20 iterations. A base 3/2 logarithm will solve the problem in 35 iterations. This observation reinforces the notion that the base of the logarithm is not important for time concerns; what is important is that the approach is logarithmic. 