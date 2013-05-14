# Rspec

#### Getting Started:
* When generating a new Rails project, add the flag ``--skip-test-unit``
* In the Gemfile, add:

		group :development, :test do
			gem 'sqlite3', '1.3.5'
			gem 'rspec-rails', '2.9.0'
		end
* The development mode RSpec files ad RSpec-specific generators
* Test mode includes files to run the tests
* RSpec is a dependency of RSpec-Rails, so we don't need to include it.