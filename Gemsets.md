# Gemsets

Different Ruby projects require different functionality, and often, functionality you write for one project will be useful in another project. In Ruby, you can package up individual bits of functionality to reuse as [gems](https://github.com/brettshollenberger/ruby_wiki/blob/master/Gems.md) (a cute little name that plays off of the name _Ruby_). There are gems that pluralize words, gems that retrieve HTML from a provided link, and gems that set up an entire framework for an application (the most popular version of such a gem for Ruby is [Rails](http://www.google.com)). 

For each project, though, you won't want to load all the gems you've ever written. That would take forever. Instead, you'll use _gemsets_. 

Gemsets are little libraries for individual projects, where each gem used in the project is stored.

You tell Ruby which gems you need for a project, and it stores them in a gemset so you can quickly switch project to project and have all the gems you need (and only the gems you need for each project). After telling Ruby which gems you want in a project, you no longer need to remember which gems the project needs: Ruby will do all the remembering for you. 

Well, not _Ruby_ per se: [Ruby Version Manager (RVM)](http://google.com). RVM can [sandbox](http://google.com) your Ruby setup for individual projects (including both the gemset your project needs, _and_ the version of Ruby your project runs on, hence the name Ruby Version Manager). 

The rest of this article describes briefly how to create gemsets and specify a default gemset and Ruby version for a project. Click here for a more detailed look at [Ruby Version Manager Command Line Interface Commands](https://github.com/brettshollenberger/ruby_wiki/blob/master/RVM%20CLI.md).  

#### To create a gemset:
		rvm gemset create <project_name>
		
#### To use a gemset:
		
		rvm gemset use <project_name>
#### To set a gemset as the default gemset
		
		rvm gemset --default use <project_name>
#### To list all of your gemsets
		
		rvm gemset list
		
To automatically use a version of Ruby and gemset with a particular project, you'll add a .ruby-gemset file and .ruby-version file to the [Rails root](https://github.com/brettshollenberger/ruby_wiki/blob/master/Rails%20Root.md).

In the command line type:

		subl .ruby-gemset	# This says "Open .ruby-gemset in Sublime"
		
(`subl` is a commonly-used [symlink](https://github.com/brettshollenberger/codecabulary/blob/master/generalterms/symlink.md) to [Sublime Text 2](https://github.com/brettshollenberger/codecabulary/blob/master/sublime/sublime.md), but you can use any [text editor](http://google.com) to create a `.ruby-gemset` file)
		
In the file `.ruby-gemset`, type the name of gemset to use and save the file.

		my-project 			# You should use the name of the project as the name of the gemset
		
Then use the command line to open `.ruby-version` in Sublime:
		
		subl .ruby-version	# This says "Open .ruby-version in Sublime"
		
In `.ruby-version` specify the version of Ruby to use and hit save:
		
		ruby-1.9.3-p392	# This says "Use Ruby version 1.9.3p-392"