# Automate Tests with Guard

Guard is a command line tool that watches a file structure and automates events when changes are made in those files. While Guard has many uses, this article explores how we can use RSpec and Guard together to automate tests.

#### Installing Guard 

Once you have [RSpec setup](https://github.com/brettshollenberger/ruby_wiki/blob/master/Setting%20Up%20RSpec.md) in your Rails project, you're ready to add Guard.

1) Add guard-rspec to the Gemfile

		group :development, :test do
			gem 'rspec-rails'		# These are set up from installing RSpec and Capybara
			gem 'capybara'
			gem 'guard-rspec'
		end
		
2) Add other :test group gems

		group :test do
			gem 'rb-fsevent'
			gem 'growl'
		end

3) `bundle`

4) `bundle exec guard init spec`

5) Edit the Guardfile to add:

		require 'active_support/core_ext'
		
		guard 'spec, :all_after_pass => false do
		
6) To run:

		bundle exec guard