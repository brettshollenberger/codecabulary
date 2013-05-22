# Test-Driven Development

_For implementation notes, see [RSpec.](https://github.com/brettshollenberger/ruby_wiki/blob/master/RSpec.md)_

Good TDD focuses on an object's behavior--what it _does_ as opposed to what it _is_. 

Highly reusable objects have a _single responsibility_ (see [Single Responsibility Principle](http://google.com)); in TDD, we test that objects fulfill their responsibility, not _how_ they fulfill it. 

Focus on behavior over implementation allows a developer to change the implementation of the behavior at any time without changing the [public interface](http://google.com)--e.g. How others interact with the object. 

By definition, if an object responds to its public methods (those it expects other objects to call), and responds correctly, the object isn't broken, but tests that are tightly coupled to a particular implementation will suddenly break if the implementation changes. Tightly-coupled tests are brittle, and others will stop relying on them.

#### Writing Good Tests

* The name of the test should focus on what we expect the object to _do_, not how we expect it to be done. If we start describing what how we expect it to be done, or what the _test_ will do, instead of what the _object_ will do at this point, we already know we need to shift our thinking.
		
		# Good 
		describe CreditCard do
			it "can make purchases"
			
		# Bad
		describe CreditCard do
			it "queries the Postgres database to see if the user has money, looks in the "funds column"...
* a



The development cycle of TDD is fail-implement-pass (and possibly refactor), or red-green-refactor.

If you don't know what a solution will even look like, start this cycle with a spike. The general shape of a solution will help in writing the test.