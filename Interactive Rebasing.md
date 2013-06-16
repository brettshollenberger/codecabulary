# Interactive Rebasing

Developers use interactive rebasing to rewrite history: to break up a single, big commit into atomized pieces, to clean up and clarify commit messages, or to simplify the timeline. Git's interactive rebasing tool allows you to rewind history and stop before each commit you want to modify, to allow you significant flexibility to get your commits exactly as you'd like them to be. 

The same caveats with standard rebasing apply to interactive rebasing. Perhaps moreso, as interactive rebasing is an even more powerful tool. _People have been fired for rebasing poorly_. Rebasing is not a necessary tool, and there's a fairly common sentiment among developers not to mess with rebasing at all. That being said, let's look at how to rebase interactively:

1) Tell Git how far you want to rewind history:

	git rebase -i HEAD~3
	
The syntax above tells Git to replay the last three commits; you may not want to edit all of them, but they'll all be replayed.

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
	#
	# If you remove a line here THAT COMMIT WILL BE LOST.
	# However, if you remove everything, the rebase will be aborted.
	
You'll notice is that your commits are presented in a reversed order from the `git log` command. If we consider that rebasing works by measuring the `git diff` between commits, and then replaying those differences, in order, back onto your timeline, then it makes sense--this is the order that the changes will be reapplied in (the same order in which they were originally applied). 

You'll also notice that Git provides us with some helpful documentation regarding our options:

| Command  | Description  |
| :--------------:  | :-------------------------------- |
| Pick | Pick is used to include a commit _as is_. If you need to change a few commits here and there, and leave some commits in between the same, use pick. |
| Reword | Reword, like pick, uses the commit _as is_, but it also stops before making the commit to allow you to change the commit message. |
| Edit | Edit causes the rebase to pause the commit process before making this commit. You can use this pause to amend the commit as you would with `git commit --amend` (adding or removing files; changing the commit message), and you can also use the pause to create additional commits before you continue the rebase--allowing you to break up a large commit into more atomic save points. You should always ensure that your working tree and index are clean before you resume the rebase. |
| Squash | Squash merges commits together. During the rebase, when this merge-commit occurs, both commit messages will pop up in your text editor for you to edit down to a single commit message. |
| Fixup | Fixup is like squash in that it merges commits, but it will not pause the rebase to ask you to create a new commit message for the merge commit. Fixup merges the commit it's called on into the previous commit, and the previous message's commit is used. |



