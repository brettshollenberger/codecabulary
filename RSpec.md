# Rspec

#### Getting Started:
* When generating a new Rails project, add the flag ``--skip-test-unit`` to supress creation of the test directory associated with the default Test::Unit framework.
* In the Gemfile, add:

		group :development, :test do
			gem 'sqlite3', '1.3.5'
			gem 'rspec-rails', '2.9.0'
		end
		
* The development mode RSpec files ad RSpec-specific generators
* Test mode includes files to run the tests
* RSpec is a dependency of RSpec-Rails, so we don't need to include it.
* Run this snippet to configure Rails to use RSpec in place of Test::Unit
		
		rails generate rspec:install
		
* If the system complains about a lack of Javascript runtime (mine didn't), visit the execs page at GitHub for a list of possibilities (Hartl recommends Node).