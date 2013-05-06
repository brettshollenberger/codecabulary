#Hashes

> "Never modify a collection whilst traversing it."  
 ~Great Master of All the Rubies

#### As of 1.9, hashes are ordered. 


#### Prefer symbols to strings as hash keys. In fact, never use mutable objects as hash keys.

		# no zen
		hash = { 'one' => 1, 'two' => 2, 'three' => 3 } 
		
		#zen and the art of motorcycle hashing
		hash = { 
			one: 1, 
			two: 2,
			three: 3
		} 
		
####Prefer literal arrays & hash creation, unless you need to pass parameters to their constructors.

		# no bueno
		hash = Hash.new
		arr = Array.new
		
		#bueno
		hash = {}
		arr = []

### Hash Methods

		> hash.fetch(k, d)
		
#### Hash.fetch will return the value of key _k_, or else return the default value _d_.

#####Use hash.fetch when dealing with keys that should be in the hash. Use the second (default) value to set a default.

