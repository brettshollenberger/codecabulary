# pwd

`pwd` is a built-in command in bash, zsh, ksh, and other common shells that's used to _print_ the _working directory_ (hence `pwd`).

	 $ pwd
	 /Users/brettshollenberger
	 
#### Options

	$ pwd -L
	
Prints your logical path; this option is passed by default.

	$ pwd -P
	
Prints your physical location, which may differ from your logical path if you've followed a symlink.