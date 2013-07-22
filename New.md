# Yeezy

Yeezy is an in-browser test suite that extends RSpec-style functionality to Javascript.

# Use

Write your tests using the RSpec expect syntax:

	  testObject = 1;
	  expect(testObject).toEq(1);

#### toRespondTo

Returns true if the test object responds to a particular method:

	  expect(testObject).toRespondTo('constructor');
	  > true
	  expect(testObject).toRespondTo('randomMethod');
	  > false

#### toNotRespondTo

Returns true if the test object does not respond to a particular method. From this point forward, assume all positive methods (toRespondTo) have an equivalent negative method (toNotRespondTo).

	  expect(testObject).toNotRespondTo('randomMethod');
	  > true

#### toInclude

Returns true if object includes the passed argument:

	  testObject = [1, 2, 3];
	  expect(testObject).toInclude(1);
	  > true

#### toHavePrototype

Returns true if the object has a given prototype:

	  testObject = {};
	  expect(testObject).toHavePrototype(Object);
	  > true

	  testObject = [];
	  expect(testObject).toHavePrototype(Object);
	  > false // Primitive datatypes are not of type Object

#### toEq

Performs == operation

#### toNotEq

Performs != operation:

	expect(testObject).toNotEq(2);

#### 

