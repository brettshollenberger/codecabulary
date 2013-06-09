# Undoing in Git

#### Amend Your Last Commit

Git amend allows you to change two things:

1) Add files to the previous commit if you forgot to

2) Change a bad commit message
	
	git commit -m "Add x.rb and y.rb"
	git add z.rb
	git commit --amend -m "Add x.rb, y.rb, and z.rb"
	
What's important to note is _how_ this works: it isn't an undo button. `git commit --amend` takes the files in your staging area, and adds them to the commit, and then allows you to alter your commit message. It's pretty sweet, but it isn't a cureall.

And it does what it says--it amends the last commit. So you end up with the single commit in your history:

	* 1cffbdd 2013-06-09 (2 minutes ago) | "Add x.rb, y.rb, and z.rb" (HEAD, master) [Brett Shollenberger]

#### Unstaging a Staged File

To upstage a single staged file:

	git reset HEAD <filename>
	
Looking at an example:

	% git status                                                                                                                                                  
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   x.rb
	#	new file:   y.rb
	
	% git reset HEAD x.rb

	% git status                                                                                                                                                  
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   y.rb
	#
	# Untracked files:
	#   (use "git add <file>..." to include in what will be committed)
	#
	#	x.rb
	
In this example, we notice how `git reset HEAD` allows us to unstage certain files. It also allows us to upstage _all_ staged files, if we provide no argument, e.g.:

	git reset HEAD
	
#### Unmodifying a Modified File

You've committed files x, y, and z, and you realize you don't like the changes in x. You want to revert _just x_ back to the previously saved version. Or to the initial commit. Or whatever it looked like when you `git clone`d. Whatever. Here's what you do:

	git hist
	* 1cffbdd 2013-06-09 (2 hours ago) | Whoops (HEAD, master) [Brett Shollenberger]
	* 9d56a3a 2013-06-09 (2 hours ago) | Initial commit [Brett Shollenberger]

	git checkout 9d56a3a -- x.rb
	
The changes in y and z have been maintained, but x has been reverted to its state in the initial commit. We would also notice if we performed a `git status` again, that HEAD is still at the Whoops checkin. x.rb shows up as modified, _and_ staged to be committed. 

Inherently, this command is a dangerous one. The changes we'd previously made to x.rb are gone; we've copied another file over it. Much safer options include stashing and branching, which are the topic of another article.