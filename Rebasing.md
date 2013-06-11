# How to Rebase

Rebasing is Git D-R-A-M-A. There are those that say `git merge` can effectively disappear thanks to `git rebase`, and those that will murder your firstborn son if you `rebase` without a very good reason. 

#### What is Rebasing?

Rebasing is used in cases where a [recursive merge](https://github.com/brettshollenberger/ruby_wiki/blob/master/Git%20Merging.md) might be used: 

![Git merge or rebase?](http://git-scm.com/figures/18333fig0327-tn.png)

In this case, `git merge` will take C3 and C4 along with their common ancestor, and `merge commit` C5, as long as there are no merge conflicts.

`git rebase` on the other hand would first take the common ancestor (C2), and perform a `git diff` for each commit along the topic branch, and then save the result of each `git diff` to a temporary file. The head then fast-forwards to C4 (since we are rebasing _onto_ the master branch), and _then_ plays out the results of each `git diff` onto C4, creating C3'.

![The timeline after git rebase master](http://git-scm.com/figures/18333fig0329-tn.png)

We notice here that master is still a step behind the experiment branch, but we can now simply perform a fast-forward merge to get up to speed. 

![After the fast-forward merge](http://git-scm.com/figures/18333fig0330-tn.png).

#### Why Use Rebase?

Rebase delivers a cleaner, linear (though some would say falsified) history. The work originally done on C3 now looks as though it was done after C4 (and it may have been, but it may not have been, too). However, it's only the history that's different; the changes have been applied just the same.

You might rebase to ensure commits apply easily on a remote branch, when it's a project you're not directly maintaining. Rebasing will make it easier for the maintainer to integrate your changes to the project.

#### When Not to Use Rebase

You absolutely shouldn't rebase if you have already pushed those commits to a public repository.

Others may have pulled the commits down already and begun working with them. If you rebase after that, your team member will end up with something like the image below:

![Don't rebase after pushing a commit to a remote repo](http://git-scm.com/figures/18333fig0339-tn.png)

In this instance, your colleague has already based C7 off of C6, which you effectively negated when you rebased to C4' instead. Now your colleague has to re-apply your changes (the same changes they already applied to the project) by `pull`ing C4' into C8. The `git log` will also look funny, since two commits will be identical. 