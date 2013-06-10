# Git Merges

#### Fast-Forward Merges

You check out a topic branch, make a quick fix, and merge it back into master. All master has to do is to move its pointer ahead one commit, since there's no other branching history to consider.

After merging, master will match the topic branch exactly, which can be removed using:

	git -d topicbranch

![Fast-Forward Merge](http://git-scm.com/figures/18333fig0312-tn.png)

#### Recursive Merges

Recursive merges occur when you have two histories on two separate branches: You checked out a topic branch, did some work, made some commits, and switched back to master, where you also did some work, and made other commits. When it comes time to merge the two together, Git must find the common ancestor of the two branches, the snapshot to merge into, and the snapshot to merge in, and merge the three together.

Provided none of the work you did on the two branches overlapped, Git will have no problems. If, for instance, you had just X.rb on the ancestor, used the topic branch to create Y.rb, and the master to create Z.rb, you'll end up with a final merged project containing X.rb, Y.rb, and Z.rb with no conflicts. You're also ready to delete your topic branch now, since Git performs a `merge commit`--all changes have been successfully merged _and_ committed. 

![The Three Amigos of the Recursive Merge](http://git-scm.com/figures/18333fig0316-tn.png)

![The Outcome](http://git-scm.com/figures/18333fig0317-tn.png)

#### Merge Conflicts

If you're not as lucky as in the example above, some of the work you did between the two branches conflicts with one another, and Git can't resolve the issue. 

It does have a few tools to help you out though. If you get the error:

	Auto-merging index.html
	CONFLICT (content): Merge conflict in x.rb
	
You'll know you need to check out x.rb to resolve the issue. If you have a number of issues, you can always check which files still containing conflicts using `git status`. The conflicting files will appear as:

	#   unmerged:   x.rb
	
The term `unmerged` refers to the fact that these files have not yet been successfully added to the merge. Since Git would have `merge commit`ted for us, we'll now have to manually add the changes ourselves when we're done:

	add x.rb
	
To alert Git that the conflicts have been resolved. Then we can `commit`, and we'll have a nice, clean master branch again.



