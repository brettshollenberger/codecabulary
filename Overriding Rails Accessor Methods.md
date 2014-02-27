# Overriding Rails Accessor Methods

With ActiveRecord, we can override the accessor methods to provide reasonable defaults:

	class SillyFortuneCookie < ActiveRecord::Base
	
		def text
			self[:text] || 'n/a'
		end
		
		def text=(value)
			self[:text] = value + " in bed"
		end
	end