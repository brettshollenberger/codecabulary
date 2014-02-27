# Serializing Rails Attributes

ActiveRecord text columns have the ability to serialize data as YAML (Yet Another Markup Language), Rails' default serialization format. To do so, let's add a column to our user model:

	def change
		add_column :users, :preferences, :text
	end
	
And in our model, we'll tell ActiveRecord to serialize the object:

	class User < ActiveRecord::Base
		serialize :preferences, Hash
	end
	
Additionally, we can set a default for our accessor, since this object will be `nil` by default:

	class User < ActiveRecord::Base
		def preferences
			self[:preferences] || self[:preferences] = {}
		end
	end

## ActiveRecord::Store

Alternately, you can perform all of this at once with the use of the `store` method:
	
	class User < ActiveRecord::Base
		store :preferences
	end