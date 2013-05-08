# Gemsets

Ruby Version Manager (RVM) is used to sandbox Ruby setups for individual projects, which may require different versions of Ruby, and different gems to operate properly. 

#### Gemsets (the gems required for a particular project) can be created using:

		rvm gemset create <project_name>
		
		rvm gemset use <project_name>
		
		rvm gemset --default use <project_name>
		
		rvm gemset list
		
#### To automatically use a version of Ruby and gemset with a particular project, add a .ruby-gemset file and .ruby-version file

		subl .ruby-gemset

		shmoogle 			# Whatever the name of the project/gemset is
		
		subl .ruby-version
		
		ruby-1.9.3-p392	