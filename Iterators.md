# Iterators

#### In Ruby, iterators are _methods_ that return the successive elements of a container, _not_ the container itself. 

		pyth_dict.iteritems()		# Not valid Ruby. In Python, iterators are collections.
		
		ruby_collection.each		# In Ruby, iterators are methods.

#### Many of the looping constructs of other languages are simply method calls in Ruby

		for item in array: 			# Python
			print item
		
		array.each do |item|		# Ruby
			print item
		end
		
