# Unix Processes

Unix processes are programs that are run in-memory. Each process has a unique process identifier (PID) to identify it.

	# list all currently running processes in long-format
	$ ps -l 
	
Processes (parent processes) like shells, which are just programs designed to run other programs, can spawn other processes (child processes). Child processes inherit their parent's context--all of the environment variables set, the current directory, etc. But child processes are also free to live a life of their own without directly altering the parent process (if run as shell scripts). Child processes can alter environment variables like `$PATH`, or `cd` all over the file system without fear of altering the variables in their parent process, or changing the parent's current directory. When the process exits, the parent process (usually the shell) will continue in the same directory, with unaltered variables.

Shell functions on the other hand tend to operate in the same process. A notable exception is in `for` loops that redirect `STDOUT` to another process, which will spawn a child process. 