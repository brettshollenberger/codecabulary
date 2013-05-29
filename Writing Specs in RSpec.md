# Writing Specs in RSpec

RSpec is used to write [unit tests](google.com), which focus on the granular details of software. 

If acceptance tests tend to focus on features in a program ("I can sign into the program"), the unit tests will focus on the details, of which there are likely many more ("I can sign into a program if I provide all the correct credentials"; "I can't sign in if my user name is already taken;" etc.). 

RSpec is a [Domain-Specific Language (DSL)](google.com) for describing the behavior a developer _expects_ to see under given _circumstances_, and each test sets up the circumstances and declares the expected outcome. If the outcome is different from the expectation, the developer sees an error, signaling them that they need to refactor their code. 

An example RSpec sentence reads:

		describe AnObject do
			it "does something we humans can understand;"
			expect(AnObject).to prove_this_somehow

RSpec also has an older style of sentence (which is not yet deprecated), which uses _should_ instead of _expect_. For example, the following are equivalent:

		expect(1).to eql(1)		# is the same as
		1.should eql(1)			# but expect is more expressive
		
To write an RSpec test unit, start with a describe block:

#### Describe blocks

		describe PrimeFactorization do
		
All statements under such a header will describe functionality related to a particular object. In this case, it describes instances of the class PrimeFactorization.

Describe statements are a means of organization. They can be named anything, but are usually named after the objects they describe, with the object's public functionality being tested in the block.

#### Before(:each) statements

Describe blocks will have many it blocks underneath them; each it block describes a particular function of the object being described.

Often, to test functionality of an object, we'll start with many of the same methods, such as instantiating the object and customizing it. 

If we're finding our it blocks repeating themselves, we can DRY out our code by moving these statements up to the before(:each) block, which will be run before each it block as its name implies. 

		before(:each) do
			prime_factorization = PrimeFactorization.new
		end

Now the prime_factorization instance is available to all of our it blocks. 

#### It blocks

	it 'returns the prime factors of 2 if given 2 as an argument' do
		expect(prime_factorization.prime_pairs(2)).to eql([[1, 2]])
	end

Now that we have an instance of the object we want to test, we'll test each of its [public methods](google.com) in it blocks.

The it statement starts again with a human-readable description: prime_pairs is a method that returns the prime factors of 2 if given 2 as an argument. Our it blocks don't have to be quite this literal, but they do serve as documentation of the methods we intend to be public for our class. 

Although the article on public methods goes into further depth on what public methods are, we should state here that public methods are the methods for your objects that you are publicly stating other developers can rely on. If another dev wants to use or extend your class, your specs will act as a contract with them that states "You may use these methods and be certain that the results you obtain through them will not change, although the way that the method achieves this result may change at any time."

Testing only public methods allows you the flexibility to refactor your code whenever you want. So it's very important, as you're learning to write industry-ready tests that you learn which methods should be public and which shouldn't. The public methods article offers an introductory discussion that is best followed up by reading [Sandi Metz' Practical Object-Oriented Design in Ruby](http://www.amazon.com/Practical-Object-Oriented-Design-Ruby-Addison-Wesley/dp/0321721330/ref=sr_1_1?ie=UTF8&qid=1369765590&sr=8-1&keywords=sandi+metz).

#### Expect statements

Expect statements are where you define how a test will pass. In the prime_pairs example above, we said that the prime_pairs method would work if the prime pairs of two were returned as an array of arrays--but it only returned the single array [1, 2] within another array. 

Another dev might guess, since we've stated this is how we expect prime_pairs to function in our test, that prime_pairs returns an array of arrays because it will usually return more than one set of pairs. But we should be more explicit about how we expect prime_pairs to function by fleshing out the method with additional examples so other developers know how we expect it to work in different circumstances.

		it "returns the prime factors of 4 if given 4 as an argument" do
			expect(prime_factorization.prime_pairs(4)).to eql([[1, 4], [2, 2]])
		end
		
That's probably how the other dev expected it would work, but now they know. We can also add our edge cases and cases in which prime_pairs receives arguments that will trip it up (aka the "non-happy path", being that we often call the "happy path" the path where a user does everything as we expect them to). Since we know users won't, we ought to display how prime_pairs will behave under other circumstances so other devs know what to expect. 

Expect statements offer a wide range of possibilities for testing--not just equality testing. For a list of RSpec methods (which are primarily expect methods), check out [RSpec Methods.](google.com)


		
		
		
