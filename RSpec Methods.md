# RSpec Methods

>The RSpec sentence is: "DESCRIBE AnObject: IT "does something we humans can understand;" EXPECT it TO function in that way by demonstrating x.

>Alternately, IT SHOULD function in a certain way. 

		expect(page).to have_content('Sample App')		# is the same as
		page.should have_content('Sample App')			# but expect is more expressive

#### Require a module in the same path
		require_relative "module_name"
		
Equivalent to:

		require "./module_name"
		
#### Describe statements

		describe PrimeFactorization do
		
All statements under such a header will describe functionality related to a particular object. In this case, it describes instances of the class PrimeFactorization.

Describe statements are merely a means of organization. They can be named anything, but should be named after their objects. Associated functionality should test functionality on only that object.

#### It statements

		it 'returns the prime factors of 2' do
		
Everything under the it statement will test whether or not the object functions as expected. For instance:

#### Expect statements

		expect(PrimeFactorization.new(2).list).to eql([2])
		
#### Let function - creates a variable corresponding to its argument

	let(:base_title) { "Ruby on Rails Tutorial Sample App" }
	...
	expect(page).to have_selector('title', "#{ base_title } | Contact")
		
