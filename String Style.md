#String Style

#### Prefer interpolation over concatenation.

		# Good
		"Something #{ Sarah } said."
		
		# Bad
		"Something " + Sarah + " said."
		
#### Prefer String#<< over String#+
		
		# Good
		hello = 'hello'
		hello << ' world'
		#> 'hello world'
		
		# String + creates separate strings, where String << mutates in place.
		# Using << is therefore much faster.
		
#### Pad the interpolated var with space to set it apart from the string.

		# Zen
		"I am the #{ compliment }."
		
		# No Zen
		"I am the #{compliment}"
		
#### Prefer single-quoted strings when you don't need interpolation or ASCII character replacement.

		# Yes
		'Brett'
		
		# No
		"Brett"
		
#### Global and instance variables should also use curly brackets for interpolation.

		# Zen
		"Das #{ $what } she #{ @said }"
		
		# Yucky
		"She #{$didst} say #{@dat}?"
		
	