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
	
	