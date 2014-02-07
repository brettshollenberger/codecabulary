# Insertion Sort

The insertion sort is an easy-to-understand, but unfortunately inefficient sorting algorithm. The concept is this: we start with the first element in the list, which is trivially sorted (since it is only a single element long):

	[5, 4, 2, 1, 3]
	
	>>  [5]
	
Then, we add a single element at a time to the list, ensuring that it is sorted appropriately:

	>> [4, 5]
	
And we continue:

	>> [2, 4, 5]
	
And so on. You'll notice that any any given point, we will have sorted one more item than the number of iterations we've presently run, and that we've turned our problem into a simple recursion. What does this look like in code? We need to keep track of two state variables, the original index of the current element we're working on positioning (`i`), and the current index of the current element as we position it (`g`):

	def insertion_sort(elements)
		i = 1
		while (i < elements.length)
			g = i
			for (g > 0 && elements[g] < elements[g-1])
				elements[g], elements[g-1] = elements[g-1], elements[g]
				g -= 1
			end
			i += 1
		end
	end

Simple, but powerful ideas. Unfortunately the worst case complexity is `O(n^2)`, the average case complexity is `O(n^2)`, and while the best case complexity is `O(n)`, the one instance out of all possible permutations of the last (of which there are `n!`, obviously) where the complexity is `O(n)` is when the entire list is already sorted. Not great odds in favor of the insertion sort.