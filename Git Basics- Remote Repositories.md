# Git Basics: Remote Repositories

As a junior dev, or dev-in-training, one of the most essential pieces to get right is collaboration through Git. It's easier than it sounds. On a given project, you'll have repositories you can pull from and push to, old topic branches that are no longer useful, and expectations from your team. 

#### Add A Remote Repo

The basic command to add a remote repository is:

	git remote add [shortname] [url]
	
Shortnames are short, easy-to-remember names that make it easier to work with your remote repos in the future (so you don't have to remember an entire URL). If you `git clone` a repo, it will automatically add that repo as the `origin` shortname, and will begin tracking the master branch of that remote repository for change. 

#### List Your Remote Repos

`git remote` is a helpful little command that will tell you the shortnames of your branches.

	% git remote
	origin
	
In practice I usually tack on the `-v` flag, because I like having a bit more detail. `git remote -v` or `--verbose`, will tell you more about what you want to hear (the following example from Pro Git):

	$ git remote -v
	bakkdoor  git://github.com/bakkdoor/grit.git
	cho45     git://github.com/cho45/grit.git
	defunkt   git://github.com/defunkt/grit.git
	koke      git://github.com/koke/grit.git
	origin    git@github.com:mojombo/grit.git
	
In the example above, we see that the shortnames are the names of the contributors, and that the contributors' URLs are HTTP, while the origin URL is SSH--meaning the user can pull from any of the remote repos, but only push changes to her own master branch (origin). This setup allows for personal accountability on projects (what I push to my repo is my responsibility), and still allows for collaboration.

#### Fetch or Pull from Remote Repos

`git fetch` grabs all the data from a remote repository that you don't have yet, and brings it back. Although it does put it into your local repository, it doesn't merge it with any of your work or change what you're currently working on.

`git pull` automatically fetches and merges the remote branch into your current branch. By default, `git clone` sets up your local master branch to track the remote master you cloned from; `git pull` will then try to merge in the remote master to your current branch.

#### Push to Remote Repos

When you've reached a good stopping point (say you've finished a feature), you can share your additions with the rest of the team by pushing upstream to the remote repository. 

	$ git push <remote> <local branch name>:<remote branch to push into>
	
For example, if you wanted to push the local master branch to the remote master branch, you could type:

	$ git push origin master:master
	
Or simply:

	$ git push origin master
	
#### Inspect a Remote Repo

The command `git remote show [remote-name]` will output helpful information about working with the repo:

	% git remote show origin
	* remote origin
	  Fetch URL: git@github.com:brettshollenberger/ruby_wiki.git
	  Push  URL: git@github.com:brettshollenberger/ruby_wiki.git
	  HEAD branch: master
	  Remote branches:
	    master  new (next fetch will store in remotes/origin)
	    working new (next fetch will store in remotes/origin)
	  Local refs configured for 'git push':
	    master  pushes to master  (up to date)
	    working pushes to working (up to date)
	    
#### Renaming Remote Repos

The `rename` command does exactly what it says it does:

	git remote rename origin new_name
	
	git remote
	new_name
	
#### Removing Remote Repos

And so does the `rm` command:

	git remote rm new_name
	
Removes the new_name remote. 