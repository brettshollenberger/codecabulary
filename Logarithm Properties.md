# Logarithm Properties

Three logarithm bases play a particularly important role in the world of computer science:

## Base 2

The binary logarithm, usually denoted `lg x`, is a base 2 logarithm. This logarithm appears in algorithms that repeatedly halve (binary search) or double (trees) their inputs. Most algorithmic applications of logarithms employ binary logarithms.

## Base e

The _natural log_, usually denoted `ln x`, is a base `e` logarithm (`e = 2.71828...`). The inverse of `ln x` is the exponential function `exp(x) = e^x`. Thus, composing these functions gives us:

	exp(ln x) = x
	
## Base 10

The common logarithm, base-10, is usually denoted `log x`. This base was employed in slide rules and logarithm books in the days before calculators, and is less widely used in algorithms today. 