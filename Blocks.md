# Blocks

#### Prefer braces for single lines

		# bad
		names.each do |name|
			puts name
		end
		
		# good
		names.each { |name| puts name }
		
#### Prefer do...end for multiline blocks, control flow, and method definitions:

		# taste
		names.each do |name|
			puts name
			puts "#{ name } is an okay guy."
		end
		
#### Avoid do...end for chaining

	# ugly
	names.select do |name|
		name.start_with?('S')
	end.map { |name| name.upcase }
	
	# beaut
	names.select { |name| name.start_with?('S') }.map { |name| name.upcase }
	
#### All you can do with a block is associate it with a call to a method via the yield keyword:

		# Define the method
		def a_method
			puts 'Start of method'
			yield
			yield
			puts 'End of method'
		end
		
		# Pass a block into the yield statement
		a_method { puts 'In the block' }
		> Start of method
		> In the block
		> In the block
		> End of method
		
#### The method and block can have more of a conversation with one another by passing parameters:

		def param_pass
			yield('Bert', 'Hola')
			yield('Dan', 'Sup')
		end
		
		param_pass { |name, greeting| puts "#{ greeting }, #{ name }!"
		> Hola, Bert!
		> Sup, Dan!	