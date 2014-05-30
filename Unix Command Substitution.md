# Unix Command Substitution

The `STDOUT` of a Unix command can be substituted as the `STDIN` to another program by surrounding the command in backticks.

	$ ls `echo $PATH | tr ':' ' '`
	
The `STDOUT` from the command above is a whitespace-separated list of directories in the user's `$PATH`, the perfect input to the `ls` program, which lists the contents of each in `STDOUT`.

Unix command substitution can be mighty powerful in combination with other commands:

	for dir in `echo $PATH | tr ':' ' '`; do
		cd ${dir}
		
		for file in `ls *`; do
			if [ -x ${file} -a ! -d ${file} ]
				echo ${file}
			fi
		done
	done | sort
	
The `for` loop in the example above loops through each directory in the user's `$PATH`, `cd`s into the directory, and `echo`s the name of the file if the file is executable (`-x`) and (`-a`) it is not (`!`) a directory (`-d`). The program pipes (`|`) the output to the `sort` program, and the final result is a sorted list of executable programs in the user's `$PATH`, the list of directories Unix will search when a user attempts to run a program.