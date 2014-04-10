# Concatenating Regular Expressions in Javascript

Suppose we have a number of regular expressions defined by the user that we want to combine at runtime to find any matches:

	new RegExp([
		(regexSources.regexOne).source,
		(regexSources.regexTwo).source
	].join('|'), 'g');
	
The `source` property of regular expressions allows us to grab their text, without flags. We can pass in text to the RegExp constructor (rather than a regular expression, which will become wrapped in another set of backslashes, breaking our formatting).

We join each of the statements in the expression with the pipe (`|`), allowing each to match. 

Finally, we can pass flags at the end of our expression.