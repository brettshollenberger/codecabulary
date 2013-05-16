# Gemsets

Different Ruby projects require different functionality. In Ruby, reusable bits of functionality are called [gems](https://github.com/brettshollenberger/ruby_wiki/blob/master/Gems.md) (a cute little name that plays off of the name _Ruby_), which are widely distributed so different developers don't have to reinvent the wheel when they go to solve a problem that's already been solved.

Since you'll have many Ruby projects in the course of your life as a Rubyist, you'll likely have many gems. But you won't want to load _all_ of them for every project you ever write; that would take forever.

That's where _gemsets_ come in. Gemsets are little libraries for individual projects, where each gem you need is a book. 

You tell Ruby which gems you need for a project, and it stores them in a gemset so you can quickly switch from one gemset to the next as you switch from project to project. After telling Ruby which gems you want in a project, you no longer need to remember which gems the project needs: Ruby will do all the remembering for you. 

Well, not _Ruby_ per se: Ruby Version Manager (RVM). RVM can sandbox your Ruby setup for individual projects (including both the gemset your project needs, _and_ the Ruby version your project needs, hence the name Ruby Version Manager). 

The rest of this article describes briefly how to create gemsets and specify a default gemset and Ruby version for a project. Click here for a more detailed look at [Ruby Version Manager Command Line Interface Commands](https://github.com/brettshollenberger/ruby_wiki/blob/master/RVM%20CLI.md).  

#### To create a gemset:
		rvm gemset create <project_name>
		
#### To use a gemset:
		
		rvm gemset use <project_name>
#### To set a gemset as the default gemset
		
		rvm gemset --default use <project_name>
#### To list all available gemsets
		
		rvm gemset list
		
#### To automatically use a version of Ruby and gemset with a particular project, add a .ruby-gemset file and .ruby-version file to the [Rails root](https://github.com/brettshollenberger/ruby_wiki/blob/master/Rails%20Root.md).

In the command line type: (`subl` is a commonly-used [symlink](https://github.com/brettshollenberger/codecabulary/blob/master/generalterms/symlink.md) to [Sublime Text 2](https://github.com/brettshollenberger/codecabulary/blob/master/sublime/sublime.md), but you can use any [text editor](http://google.com) to create a `.ruby-gemset` file) :

		subl .ruby-gemset	# This says "Open .ruby-gemset in Sublime"
		
In the file `.ruby-gemset`, type the name of gemset to use and save the file.

		my-gemset 			# You should use the name of the project as the name of the gemset
		
Now, open `.ruby-version` in Sublime:
		
		subl .ruby-version	# This says "Open .ruby-version in Sublime"
		
And specify the version of Ruby to use:
		
		ruby-1.9.3-p392	