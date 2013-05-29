# Factory Girl

Factories allow you to create valid instances of your Rails [model classes](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20a%20Rails%20Model.md) to reuse. Factories are commonly used to [DRY](google.com) out [unit tests](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20Specs%20in%20RSpec.md).

#### Setting Up Factory Girl with Rails

1) Add to Gemfile in :test

		group :test do
			gem "factory_girl_rails"
		end

2) In spec_helper.rb

		require "factory_girl_rails"
		
3) Create a support or factory folder in your spec folder to automatically have factories loaded

		> spec/support
		> spec/factories
		
#### Writing a Factory with Factory Girl

1) Each factory has a name and a set of attributes:

		FactoryGirl.define do
			factory :classname do
				attribute "Valid"
				attribute2 "Valid"
			end
		end
		
2) By default, the factory can be named anything, but the name is used to guess the class of the object it intends to create a factory from. Thus, most commonly, the factory's name is the same as that of the model. If the name is different, the model class can be set explicitly:

		FactoryGirl.define do
			factory :admin, :class User do
			...
			
3) In the spec file, build the factory and assign it to a variable to pass around.

		let(:admin)
			FactoryGirl.build(:admin)
		end
		
FactoryGirl offers a number of build strategies:

		FactoryGirl.build(:admin)	# Returns, but doesn't save, an instance of User
		
		FactoryGirl.create(:admin) # Creates and saves an instance of User, and returns the saved instance
		
		FactoryGirl.attributes_for(:admin) # Returns a hash of attributes that can be used to build a valid User instance
		
		FactoryGirl.build_stubbed(:admin) # Returns an object with all defined attributes stubbed out
		
No matter which build strategy you use, you can override the attributes defined in the factory by using a hash as a build argument

		FactoryGirl.build(:admin, first_name: "Brett")

4) Now it's ready to use in your specs:

		it "is totally valid" do
			expect(admin).to be_valid
		end

#### Recommendations:

1) Define one factory per class that provides the simplest set of attributes necessary to create an instance of that class. If you're creating Active Record objects, that means you only provide attributes that:

* Are required via [validations](https://github.com/brettshollenberger/ruby_wiki/blob/master/Active%20Record%20Validations.md)
* Do not have default values

2) Factories can be defined anywhere, but will be automatically loaded if they're kept in any of the following locations:

		test/factories.rb
		spec/factories.rb
		test/factories/*.rb		# Aka any Ruby file in the factories folder
		spec/factories/*.rb
		
