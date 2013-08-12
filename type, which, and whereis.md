# type, which, and whereis

`type` is a bash built-in that searches your environment (files in $PATH, keywords, aliases, built-ins, and functions) for executable commands that you pass to it as arguments.

	$ type which
	which is a shell builtin
	
`which` is very similar, and widely used, but it only searches the files in $PATH and provides slightly different output:

	$ which which
	which: shell built-in command
	
#### Yes, but where are the files?

To find the location of executables with these commands, you can also pass the `-a` flag to either of them:

	$ type -a which
	which is a shell builtin
	which is /usr/bin/which
	which is /usr/bin/which
	
Or use the builtin `whereis`: 

	$ whereis heroku
	/usr/bin/heroku