# Rspec

#### Workflow

* Generate the integration test. This generates the static_pages_spec.rb in the spec/requests directory.

		rails generate integration_test static_pages
		
* Write the test:

		require 'spec_helper'
		describe "Static pages" do
			describe "Home page" do
				it "should have the content 'Sample App'" do
					visit '/static_pages/home'
					expect(page).to have_content('Sample App')
				end
			...

We specify the page we're describing (Home page), and in the "it" statement write whatever we want to describe the test. RSpec doesn't care what we call it--like any other function or method. 

The body of the spec describes the actions to take: visit the homepage at the specified URL. If it has the content "Sample App," it passes the test.

		bundle exec spec spec/requests/static_pages_spec.rb
		
``bundle exec`` ensures that RSpec runs in the environment specified by the Gemfile. Then pass the path to the spec file. 


		