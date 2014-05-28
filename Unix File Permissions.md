# Unix File Permissions

In general on Unix systems, we have the concepts of users & groups. Users are individual operators of the system, and groups are logic sets of users that share certain permissions (access to files and directories on the system). 

File permissions are set in triplets: 1) What the owner of the file can do with it, 2) What the owner group can do with the file, and 3) What everyone else (others) can do with the file.

File permissions have three modes: 1) Read, 2) Write, and 3) Execute.

File directories have the same three modes, with different meanings: 1) Read - view the contents of a directory, 2) Create or delete files within the directory (note: a user with directory permission can delete files they do not have specific file-wise write access to), and 3) Execute - `cd` into the directory.

## Viewing File Permissions
To view the permissions of files: `$ ls -l <filename>`

| Owner user            | Owner group   | Others             | 
| -------------------------  |:------------------:|:-------------------:|
| -rwx                         | r-x                   | r-x                   |
| read, write, execute | read, execute | read, execute |

## Updating File Permissions
To update file permissions, use the `chmod` program:

`chmod [options] mode file(s)`

Updating modes:

	u - Owner user
	g - Owner group
	o - Others
	a - All (default)
	
	+ - Set bits
	- - Clear bits
	
	r  - Read bit
	w - Write bit
	x - Execute bit
	
Examples:

	# set execute bit for owner
	chmod u+x some.file
	
	# set read bits for all
	chmod a+r some.file
	
	# alternately
	chmod +r some.file
	
	# set write bit for owner group
	chmod g+w some.file
	
	# clear write bit for others
	chmod o-w some.file
	
	# setting many bits at once for owner
	chmod u+rwx some.file
	
## Funny Numbers

Often, you'll see permissions described using a numerical shorthand, such as 755:

	user rwx => 4 + 2 + 1 = 7
	group r-x => 4 + 1 = 5
	others r-x => 4 + 1 = 5
	

	
	

