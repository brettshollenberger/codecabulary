# Javascript Integration Tests

Testing Javascript interactions is notoriously painful. There are 1) Implicit challenges with testing Javascript because of its asynchronous nature, and 2) painful existing APIs for performing integration tests in the Javascript itself (unit tests are another story; Jasmine is quite good). 

The solution to the first challenge is: Use a testing framework. 

The solution to the second challenge is: Probably not a Javascript framework. 

Angular/Protractor is the noteworthy exception I've had the pleasure of working with, but its API is tied to Angular, so if you aren't using Angular, you're arguably out of luck with Javascript testing frameworks (feel free to argue otherwise in the comments; my main gripe is they're pretty painful and read poorly). Instead, you could configure some other server-side testing framework; my pick is Capybara.

## Capybara

Capybara is a flexible, Ruby-based integration testing framework with a wonderful API. Although by default it works with Rack-based Ruby applications like Rails and Sinatra apps, you can use another driver like Selenium, and configure the framework to hit a remote host to perform testing, or even your own loopback interface (e.g. `localhost`) to perform tests locally. 

### Configuration

Let's look at a possible configuration for your application.

1) First you'll want a `Gemfile`, a Ruby-based file that specifies Ruby dependencies using RubyGems. Your Gemfile could include the following:

	source 'https://rubygems.org'
	ruby '2.0.0'
	
	# the Ruby "make" utility, which we'll use to configure our test task
	gem 'rake'
	
	# the driver we'll use to run asynchronous tests
	gem 'selenium-webdriver'
	
	# one of several Ruby DSLs you can use for writing tests
	gem 'rspec'
	
	# the capybara dependency itself
	gem 'capybara'
	
2) Now we can write a `Rakefile`, a Ruby-based file to create command line tasks. This is the default configuration for Rspec that will allow us to run tests via the `rake` command or the `rake spec` command:

	require 'rspec/core/rake_task'
	
	RSpec::Core::RakeTask.new(:spec)
	
	task :default => :spec
	
3) Next we want to create a `spec/spec_helper.rb` file to configure both Rspec and Capybara:

	require "rspec"
	require "capybara"
	require "capybara/dsl"
	
	# Requires supporting ruby files with custom matchers and macros, etc,
	# in spec/support/ and its subdirectories.
	Dir[File.expand_path("./spec/support/**/*.rb")].each { |f| require f }
	
	RSpec.configure do |config|
	  config.mock_with :rspec
	  config.include Capybara::DSL
	  
	  # include helpers from our `spec/support` directory, namely the Requests module
	  config.include Requests, :type => :feature
	end
	
	Capybara.configure do |config|
	  config.run_server        = false
	  config.default_wait_time = 5
	  config.default_driver    = :selenium
	  config.javascript_driver = :selenium
	  
	  # the URL where your application is hosted
	  config.app_host          = "http://localhost:3030"
	end
	
4) Now we can configure helpers for our tests in `spec/support`. All Ruby files under this directory will be automatically included in our `spec_helper`, and we can include them in the `RSpec.configure` block. Here's an example file that I've used in our applications that allows us to store our passwords in a file called `config/secrets.yml` like in Rails 4.1, to login as a specified user, and to logout:

	require("yaml")
	require("ostruct")
	
	module Requests
	  @config = OpenStruct.new
	
	  YAML.load(ERB.new(File.read("./config/secrets.yml")).result).each do |secret, value|
	    @config.send("#{secret}=", value)
	  end
	
	  def self.config
	    @config
	  end
	
	  # Accepts a username or user type, and password
	  #
	  # If a user type is provided, will map that user to a particular user of the correct type
	  def login_as(username="district_admin", password=Requests.config.password)
	    visit "/"
	    fill_in "Email", :with => username.to_user_type.to_name
	    fill_in "Password", :with => password
	    click_on "Login"
	  end
	
	  def logout
	      click_on "Logout"
	  end
	
	  class UserType
	    def self.types
	      {
	        :school_admin => :annebehel,
	        :district_admin => :wbgesadmin
	      }
	    end
	
	    def initialize(type)
	      @type = type.to_sym
	    end
	
	    def to_name
	      UserType.types[@type] || @type
	    end
	  end
	end
	
	class String
	  def to_user_type
	    Requests::UserType.new(self)
	  end
	end
	
	class Symbol
	  def to_user_type
	    Requests::UserType.new(self)
	  end
	end
	
5) If you want to configure a `config/secrets.yml` file to follow along with the example, you could have something as simple as:

	password: <%= ENV["MY_APPLICATION_DEFAULT_PASSWORD"] %>
	
And of course you would configure an environment variable with the name `MY_APPLICATION_DEFAULT_PASSWORD` via:

	$ export MY_APPLICATION_DEFAULT_PASSWORD=password
	
Make sure your `config/secrets.yml` file is not included in source control. If you use Git, you could configure the following:

	# .gitignore
	config/secrets.yml
	
6) Write your integration tests under the `spec/features` folder. Here's an example test case that describes logging in as various user types:

	require 'spec_helper'

	describe "Logging in", :type => :feature do
	  describe "As a verified user" do
	    describe "As a school admin" do
	      before(:each) do
	        login_as :school_admin
	      end
	
	      it "redirects the user to their school's members page" do
	        expect(page).to have_content("Forest Hills Elementary")
	      end
	    end
	
	    describe "As a district admin" do
	      before(:each) do
	        login_as :district_admin
	      end
	
	      it "redirects the user to their district's members page" do
	        expect(page).to have_content("Charleston County School District")
	      end
	    end
	
	    describe "With the wrong password" do
	      before(:each) do
	        login_as :school_admin, :wrong_password
	      end
	
	      it "gives the user an error message" do
	        expect(page).to have_content("There was an error logging in. Please try again.")
	      end
	    end
	  end
	end
	
7) Finally, run your tests with `rake`. If you've configured everything correctly, you'll see your tests run, and identify failures as they occur. Happy hacking :) 






