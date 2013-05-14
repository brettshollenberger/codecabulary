# RSpec Methods

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
		
