# Ruby Sort By

Ruby implements a `sort_by` method that takes a block to sort by; let's take a look at how we could create one ourselves:

#### n log n Complexity (aka, the bad way):

If we open up the Array class to add a sorter function that takes a block, we could do so by comparing each element via the block like so:

	class Array
		def sorter(&block)
			self.sort do |x, y|
				block.call(x) <=> block.call(y)
			end
		end
	end
	
Doing so means that we're calling the block twice for each comparison, resulting in an _n log n_ complexity, but we could rewrite the function to an _n_ complexity.

#### n Complexity (aka, the good way):

Let's revise this in long-form so we can see what's going on, and then refactor down to a shorthand that will make our mothers proud:

	def sorter(&block)
		return_array = []
		self.each do |x|
			return_array << [x, block.call(x)]
		end
			
		self.sort { |x, y| x[1] <=> y[1] }.map { |x| x[0] }
	end
	
Here we see that we're creating an array of arrays wherein each sub-array consists of the element itself and the result of the call to the block. 

We can use map again to improve this code:

	def sorter(&block)
		self.map { |x| [x, block.call(x)] }.sort { |x, y| x[1] <=> y[1] }.map { |x| x[0] }
	end