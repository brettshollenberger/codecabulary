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

#### Array.each_with_index 

		a = [10, 5, 2, 1]
		a.each_with_index { |value, index| puts "#{ value } is at index #{ index } }
		
		>> 10 is at index 0
		>> 5 is at index 1
		>> 2 is at index 2
		>> 1 is at index 3