# Logarithms and Fast Exponentiation

Algorithms are important in computer science--especially the fundamental ones. The faster we can compute basic operations, the more we're able to accomplish in a shorter amount of time. Logarithmic time algorithms execute very quickly; if an algorithm executes in log<sub>2</sub>n time, it can process 100,000 pieces of data in 20 basic operations. At that rate, we can process inputs of nearly any size. 

We can process exponents in linear time (a number of steps proportional to the size of the input) simply by repeated multiplication:

	b^4 = b * b * b * b
	
But we can process exponents in logarithmic time when we make the observation that:

	b^n = (b^n/2)^2 	# if n is even
	b^n = b * (b^n-1/2)^2    # if n is odd
	
This type of algorithm illustrates an important concept related to the "divide and conquer" principle of algorithm design--it pays to divide the job as evenly as possible. It's not a big deal that in the case of an odd exponent that we simply multiply the even problem by the base once; after this important first step, we end up with the same type of algorithm that continues halving the size of the exponent on each iteration.

In Ruby, we might write this process as:

	def fast_exp(b, n, total)
		return total if n == 0
		return fast_exp(b*b, n/2, total) if n.even?
		fast_exp(b, n-1, total*b)
	end