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

#### Workflow

* Generate the integration test. This generates the static_pages_spec.rb in the spec/requests directory.

		rails generate integration_test static_pages
		
* Write the test:

		require 'spec_helper'
		describe "Static pages" do
			describe "Home page" do
				it "should have the content 'Sample App'" do
					visit '/static_pages/home'
					page.should have_content('Sample App')
				end
			...

We specify the page we're describing (Home page), and in the "it" statement write whatever we want to describe the test. RSpec doesn't care what we call it--like any other function or method. 

The body of the spec describes the actions to take: visit the homepage at the specified URL. If it has the content "Sample App," it passes the test.

		bundle exec spec spec/requests/static_pages_spec.rb
		
``bundle exec`` ensures that RSpec runs in the environment specified by the Gemfile. Then pass the path to the spec file. 


		