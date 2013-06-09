# Git Log

The `.gitconfig` is one of those dotfiles devs tend to spend a lot of time on. After all, you're going to be getting pretty cozy with Git, and gitting it right will make your job a lot easier. So from me, to you, here's my current favorite `git log` option (aliased as `git hist`):

	[alias]
		hist = log --pretty=format:'%h %ad (%ar) | %s%d [%an]' --graph --date=short
		
Which comes out a little something like this:

	* 2ee1eea 2013-06-07 (2 days ago) | Add writing forms (HEAD, master) [Brett Shollenberger]

The `git log` allows you to visualize a project's history, and by default, it's a little verbose:

	commit def038298ed4b15de68070ff3347ef788ee73269
	Author: Brett Shollenberger <brett.shollenberger@gmail.com>
	Date:   Mon Jun 3 21:14:32 2013 -0400
	
	    Add Database Indices
	    
But there are also an incredible number of options to personalize `git log`. Pro Git describes a huge number of them, but let's keep it to the classics:

	%H  Commit hash
	%h  Abbreviated commit hash
	%T  Tree hash
	%t  Abbreviated tree hash
	%P  Parent hashes
	%p  Abbreviated parent hashes
	%an Author name
	%ad Author date (format respects the --date= option)
	%ar Author date, relative
	%cn Committer name
	%cd Committer date
	%cr Committer date, relative
	%s  Subject