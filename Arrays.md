#Arrays 

####Prefer literal arrays & hash creation, unless you need to pass parameters to their constructors.

		# no bueno
		hash = Hash.new
		arr = Array.new
		
		#bueno
		hash = {}
		arr = []

#### Prefer %w syntax to literal array syntax when you need an array of words.

		#ick
		STATES = ['draft', 'open', 'closed']
		
		#yum
		STATES = %w(draft, open, closed)
		
#### Prefer %i to literal array syntax when you need an array of symbols.

		#no-no
		ice_cream = [:vanilla, :chocolate, :rocky_road]
		
		#yes-yes
		ice_cream = %i(vanilla, chocolate, rocky-road)
		
#### Avoid creating large gaps in arrays

		# no good
		arr = []
		arr[100] = 1
		
		# Now you've got a mess of nils
		
### Array Methods