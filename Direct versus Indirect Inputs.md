# Direct versus Indirect Inputs

In Ruby, any time we send a message to an object other than `self` in order to use its return value, we're using `indirect inputs`. The more indirection that appears in a single method, the more closely that method is tied to the structure of the code around it, and the more likely it is to break. This observation is often referred to as the `Law of Demeter`. 

Input could also come from the surrounding system. For instance, we could use environment variables to influence code within the program:

	def format_time
		format = ENV.fetch('TIME_FORMAT') { '%D %r' }
		Time.now.strftime(format)
	end