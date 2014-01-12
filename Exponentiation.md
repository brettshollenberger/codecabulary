# Exponentiation Algorithms

O(logn):

	class Integer
		def pow(n)
			pow_iter(1, self, n)
		end
		
		def pow_iter(total, base, count)
			return total if count == 0
			pow_iter(total * base, base, count-1)
		end
	end