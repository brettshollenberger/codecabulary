# Array Methods

#### Array.at(index) -- Returns value or nil if index DNE

		a = [1, 2, 3]
		a.at(0)
		>> 1
		
		a.at(100)
		>> nil
		
#### Array.fetch(index) -- Returns value or IndexError if index DNE

		a.fetch(100)
		>> IndexError
		
#### Array.first

		a.first
		>> 1
		
#### Array.last
		
		a.last
		>> 3
		
#### Array.take(num)

		arr = (1..6).to_a
		>> [1, 2, 3, 4, 5, 6]
		
		arr.take(3)
		>> [1, 2, 3]
		
		arr
		>> [1, 2, 3, 4, 5, 6]	# Take is non-destructive

#### Array.drop(num)

		arr.drop(3)
		>> [4, 5, 6] 		# Drop is also non-destructive
		
#### Array.count

		arr.count
		>> 6
		
#### Array.length

		arr.length
		>> 6
		
#### Array.empty?

		arr.empty?
		>> false
		
		b = []
		b.empty?
		>> true
		
#### Array.include?

		arr.include?(99)
		>> true
		
		arr.include?(101)
		>> false
		
		b = ['strawberry']
		b.include?('strawberry')
		>> true
		
#### Array.push(value) -- Add value at the end of the array

		c = [1, 2, 3]
		c.push(4)
		>> [1, 2, 3, 4]
		
#### Array.pop -- Removes and returns the last element in an array (destructive)

		c.pop
		>> 4
		c
		>> [1, 2, 3]
		
#### Array.unshift(value) -- Add value at the beginning of the array

		c.unshift(0)
		>> [0, 1, 2, 3]
		
#### Array.shift(value) -- Remove and return the first item (destructive)

		c.shift
		>> [1, 2, 3]
		
#### Array.insert(index, value)

		c.insert(1, 'apple')
		>> [1, 'apple', 2, 3]
		
#### Array.delete_at(index)

		c.delete_at(1)
		>> [1, 2, 3]
		
#### Array.delete(value)

		c.delete(1)
		>> [2, 3]
		
#### Array.compact -- Returns an array without nil values; not in place

		arr = ['a', 1, 'b', 2, nil, nil, 3, nil, 'happy']
		arr.compact
		>> ['a', 1, 'b', 2, 3, 'happy']
		arr
		>> ['a', 1, 'b', 2, nil, nil, 3, nil, 'happy'] 	# Compact is not performed in-place

#### Exclamation point performs method in-place

		arr.compact!
		>> ['a', 1, 'b', 2, 3, 'happy']
		arr
		>> ['a', 1, 'b', 2, 3, 'happy']
		
#### To remove duplicates in an array, there's the non-destructive Array.uniq and the destructive Array.uniq!

		a = [1, 1, 2, 3, 3]
		a.uniq					# Modification not in-place
		>> [1, 2, 3]
		a
		>> [1, 1, 2, 3, 3]
		a.uniq!					# Performs modification in-place
		>> [1, 2, 3]
