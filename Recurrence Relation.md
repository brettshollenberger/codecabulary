# Recurrence Relation

A recurrence relation is a description of a solution to a problem whose solution depends on smaller instances of the same problem (recurrent problems). 

Consider factorials, where, we can observe that the factorial of a given number _n_ is equivalent to `n * factorial(n-1)`:

	n! = 1 * 2 * 3 * ... n = n * n-1!
	
We could describe the solution to a factorial as follows: 

Where T<sub>0</sub> is the solution to the 0th factorial:

T<sub>0</sub> = 1

T<sub>1</sub> = 1

T<sub>n</sub> = n * T<sub>n-1</sub> for n > 1