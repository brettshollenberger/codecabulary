# Writing Acceptance Tests in Capybara

Capybara comes with a [Domain-Specific Language (DSL)](google.com) for writing descriptive [acceptance tests](google.com). A domain-specific language is a language created to solve a particular set of problems--in the case of specs, the DSL describes the expected functionality of an application in various situations. 

There are several DSLs dedicated to writing specs in Ruby, including [RSpec](https://github.com/brettshollenberger/ruby_wiki/blob/master/Setting%20Up%20RSpec.md) and Test Unit. Capybara's DSL solves an even more granular problem: Writing acceptance tests.

For that reason, Capybara's DSL extends the most popular testing DSLs, Cucumber, [RSpec](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20Specs%20in%20RSpec.md), Test Unit, and Mini Test. 

Since Capybara's DSL is an extension, the underlying spec DSL can also be used in writing acceptance tests, although the Capybara language will be more useful for writing acceptance tests. 

A traditional Capybara "paragraph" would be laid out like this:

		feature "Sign up" do
		  background do
		    User.make(:email => 'user@example.com', :password => 'caplin')
		  end
		
		  scenario "Signing in with correct credentials" do
		    visit '/sessions/new'
		    within("#session") do
		      fill_in 'Login', :with => 'user@example.com'
		      fill_in 'Password', :with => 'caplin'
		    end
		    click_link 'Sign in'
		    page.should have_content 'Success'
		  end
		  
`feature` is in fact just an alias for `describe ..., :type => :feature`, `background` is an alias for `before`, and `scenario` for `it`.

Using the Capybara acceptance testing language allows us to assert that the Capybara tests test features, as opposed to more the granular object-functionality approach of unit tests. 

Being that the Acceptance Testing DSL is really just an extension of (and in many ways aliasing of) the RSpec language, readers can get a more detailed look at writing acceptance tests in [Writing Specs in RSpec](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20Specs%20in%20RSpec.md).

For a more detailed look at Capybara, check out the [Capybara Cheet Sheet](https://github.com/brettshollenberger/ruby_wiki/blob/master/Capybara%20Cheat%20Sheet.md).