# Setting Up RSpec

#### In Plain Ruby:

Conventionally, you'll establish the following file structure:

		> application_root
			- .rspec
			- Gemfile
			>> lib
				- app.rb
			>> spec
				- app_spec.rb

The .rspec folder can contain this setting to colorize the output in the traditional red/green style:

		--color
		
That's it. It doesn't look like much, but that command will start your RSpec off on the right foot, and is the only command you really need to get started with.

The Gemfile also only need include a single line:

		gem 'rspec'
		
Fairly straightforward so far, eh? Let's move on to the app_spec.rb file:

At the top of the document, make sure to include:

		require "rspec"
		require_relative "../lib/app"
		
Where app is the name of the app.rb file. Now your spec file is linked to your app file, and ready to rock and roll. Make sure, if you haven't already got the [gem](https://github.com/brettshollenberger/ruby_wiki/blob/master/Gems.md) in the current [gemset](https://github.com/brettshollenberger/ruby_wiki/blob/master/Gemsets.md) that you run:

		bundle
		
To download RSpec and get moving. Next in your course of action, we recommend you check out [Writing Specs in RSpec](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20Specs%20in%20RSpec.md), and then move on to [RSpec Methods](https://github.com/brettshollenberger/ruby_wiki/blob/master/RSpec%20Methods.md).