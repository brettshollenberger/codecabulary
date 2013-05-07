# Array Iterators

Like all classes that include the Enumerator module, Array has an each method which defines which elements should be iterated over and how.

#### Array.each

		a = [10, 20, 30]
		a.each { |num| print num -= 10, " " }
		>> 0, 10, 20 >> [10, 20, 30]			# Array remains unchanged
		
#### Array.reverse_each

		a = %w(rats live on no evil star)
		a.reverse_each { |word| print "#{ word } " }
		>> star evil no on live rats
		
		a.reverse_each { |word| print "#{ word.reverse } " }
		>> rats live on no evil star
		
#### Array.map used to create a new array using modified values

		a = [1, 2, 3]
		z = a.map { |num| num + 1 }
		>> [2, 3, 4]
		a
		>> [1, 2, 3]
		
		a.map! { |num| num + 1 }
		>> [2, 3, 4]
		a
		>> [2, 3, 4]

#### Array.each_with_index 

		a = [10, 5, 2, 1]
		a.each_with_index { |value, index| puts "#{ value } is at index #{ index } }
		
		>> 10 is at index 0
		>> 5 is at index 1
		>> 2 is at index 2
		>> 1 is at index 3