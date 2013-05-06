# Regexes

#### A single character of: a, b, or c:

		[abc]

#### Any single character except: a, b, or c:

		[^abc]
		
#### Any single character in the range a-z:

		[a-z]
		
#### Any case insensitive character in the range a-z

		[a-zA-Z]
		
#### Line starts with:

		^
		
#### Line ends with:

		$
		
#### Start of string:

		\A
		
#### End of string:

		\z
		
#### Any single character:

		.
		
#### Any whitespace character:

		\s
		
#### Any non-whitespace character:

		\S
		
####	Any digit:

		\d
		
#### Any non-digit:

		\D
		
#### Any word character (letter, number, underscore):

		\w
		
#### Any non-word character:
ˇ
		\W
		
#### Any word boundary:

		\b
		
#### Capture everything enclosed:

		(...)
		
#### a or b:

		(a|b)
		
#### Zero or one of a:

		a?
		
#### Zero or more of a:

		a*
		
#### One or more of a:

		a+
		
#### Exactly 3 of a:

		a{3}
		
#### Three or more of a:

		a{3, }
		
#### Between three and six of a:

		a{3, 6}