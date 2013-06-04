# Setting Up RSpec

This article describes the RSpec setup process in both Ruby and Rails. After setting up in whichever environment you'll be developing in, we recommend you check out [Writing Specs in RSpec](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20Specs%20in%20RSpec.md), and then move on to [RSpec Methods](https://github.com/brettshollenberger/ruby_wiki/blob/master/RSpec%20Methods.md).

#### In Plain Ruby:

* Conventionally, you'll establish the following file structure:

		> application_root
			- .rspec
			- Gemfile
			>> lib
				- app.rb
			>> spec
				- app_spec.rb

* The .rspec folder can contain this setting to colorize the output in the traditional red/green style:

		--color
		
* The Gemfile also only need include a single line:

		gem 'rspec'
		
* In the app_spec.rb file, at the top of the document, make sure to include:

		require "rspec"
		require_relative "../lib/app"
		
Where app is the name of the app.rb file. Now your spec file is linked to your app file, and ready to rock and roll. Make sure, if you haven't already got the [gem](https://github.com/brettshollenberger/ruby_wiki/blob/master/Gems.md) in the current [gemset](https://github.com/brettshollenberger/ruby_wiki/blob/master/Gemsets.md) that you run:

		bundle
		
To download RSpec and get moving. 

#### In Rails

* When generating a new Rails project, add the flag ``--skip-test-unit`` to supress creation of the test directory associated with the default Test::Unit framework.
* In the Gemfile, add:

		group :development, :test do
			gem 'rspec-rails'
		end
		
* The development mode RSpec files add RSpec-specific generators
* Test mode includes files to run the tests
* RSpec is a dependency of RSpec-Rails, so we don't need to include it.
* Run this snippet to configure Rails to use RSpec in place of Test::Unit
		
		rails generate rspec:install
		
* If the system complains about a lack of Javascript runtime (mine didn't), visit the execs page at GitHub for a list of possibilities (Hartl recommends Node).
* Run bundle:

		bundle
		
#### Retroactively Adding RSpec to a Rails App

Use the `-s` flag to signal spec generation and set the `--migration` flag to `false` so you don't create a duplicate model. 

		rails g model ModelName -s --migration=false
