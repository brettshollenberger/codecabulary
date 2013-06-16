# How to Deploy an App with Heroku

Heroku is _the_ stupid-simple way to move Rails apps to production using Git commands. This article will walk you through the steps you need to take a Rails app to production via Heroku, and includes the _gotchas_ a Heroku first-timer will want to avoid. 

_Have other Heroku tips and tricks? Leave them in the comments!_

1) Install the [Heroku Toolbelt](https://toolbelt.heroku.com), a command-line interface for working with Heroku apps. The Heroku Toolbelt will grant us access to the Heroku commands we'll use throughout the rest of this article.

2) Heroku uses Postgres databases, so include the `pg` gem or the argument `database=postgresql` when setting up your app

	rails new app_name --database=postgresql
	
3) In your home directory, ensure your `.gitignore_global` contains `config/database.yml`. Heroku will create its own `database.yml`, though you'll need to maintain yours separately for your testing and development environments.

4) `git init` your repo. 

5) Add a remote repository on GitHub for your code.

	git remote add origin <url>
	
6) It's a good idea to manage a branch for production and a branch for staging soon-to-be-released features for non-technical stakeholders. 

	git checkout -b staging
	git checkout -b production
	
7) These branches should coincide with Heroku apps. Traditionally, the real app name coincides with the production branch, and the staging app is named real-app-name-staging (and coincides with the staging branch). Heroku apps can be created by:

	heroku create <app_name>
	heroku create heroku_example
	heroku create heroku_example_staging
	
By default, Heroku will recognize that you're running a Rails app, and will use the cedar stack. Although the cedar stack should be used with Rails apps, if you wanted to change the stack, you could pass the argument:

	heroku create <app_name> --stack <stack_name>
	
8) `heroku create` will instantiate a git remote name heroku. Heroku will be a rather useless name if you're using the staging-production convention, so you should rename your remotes:

	git remote rm heroku
	git remote add staging git@heroku.com:heroku-example-staging.git
	git remote add production git@heroku.com:heroku-example.git
	
9) Now your remotes should look something like:

	git remote -v
	origin	git@github.com:brettshollenberger/heroku_example.git (fetch)
	origin	git@github.com:brettshollenberger/heroku_example.git (push)
	production	git@heroku.com:heroku-example.git (fetch)
	production	git@heroku.com:heroku-example.git (push)
	staging	git@heroku.com:heroku-example-staging.git (fetch)
	staging	git@heroku.com:heroku-example-staging.git (push)
	
10) To publish your app, `git push` it to production or staging:

	git push staging staging:master
	
This slightly more complex take on the `git push` command works as follows:

	git push <remote_repo_name> <local_branch>:<remote_branch>
	
Heroku will ignore any pushes to branches other than master, which is why we maintain separate apps for staging and production; we can't push to a staging branch on Heroku. Make sure you're always pushing to Heroku master.

11) Heroku will require you to migrate your database in order for the changes to be reflected in staging or production. Heroku can run shell commands via `run`. Since you have multiple apps in the same directory (your staging and production apps) you'll also need to pass the `--app APP` argument to allow Heroku to know which app you intend to run `rake db:migrate` on:

	heroku run rake db:migrate --app heroku-example-staging
	
_That's it! Your app is now live via the magic of Heroku! Check out a few additional Heroku tips and tricks below to continue along your path to true Heroku enlightenment._
	
#### Heroku Tips and Tricks

* Check logs with Heroku

		heroku logs
	
* Run Rails console via Heroku (useful for managing your production database):

		heroku run rails console