# Interactive Rebasing

Developers use interactive rebasing to rewrite history: to break up a single, big commit into atomized pieces, to clean up and clarify commit messages, or to simplify the timeline. Git's interactive rebasing tool allows you to rewind history and stop before each commit you want to modify. As with standard rebasing, interactive rebasing should only be done with commits that have not yet been pushed up to a public repo.

1) Tell Git how far you want to rewind history:

	git rebase -i HEAD~3
	
The syntax above tells Git to replay the last five commits; you may not want to edit all of them, but you at least want to replay some of them.

2) Git opens a file in your text editor to provide you with options. 
	
	pick 47b18f4 Add X
	pick 25de9f0 Add Y
	pick f746bd4 Add Z
	
	# Rebase 58a61e6..f746bd4 onto 58a61e6
	#
	# Commands:
	#  p, pick = use commit
	#  r, reword = use commit, but edit the commit message
	#  e, edit = use commit, but stop for amending
	#  s, squash = use commit, but meld into previous commit
	#  f, fixup = like "squash", but discard this commit's log message
	#  x, exec = run command (the rest of the line) using shell
	#
	# If you remove a line here THAT COMMIT WILL BE LOST.
	# However, if you remove everything, the rebase will be aborted.
	
You'll notice is that your commits are presented in a reversed order from the `git log` command. If we consider that rebasing works by measuring the `git diff` between commits, and then replaying those differences, in order, back onto your timeline, then it makes sense--this is the order that the changes will be reapplied in (the same order in which they were originally applied). 

You'll also notice that Git provides us with some helpful documentation regarding our options:

