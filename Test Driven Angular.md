# Angular Test Driven Development

Test-driven development is an approach to building software where you write tests for features that don't yet exist, and then implement the feature so that it passes the tests. It's an assertive way to describe how your software is supposed to function, and serves as documentation for other developers. At Faculty Creative, we write software for clients, and at any given point in time our clients' needs are going to change--we know that--so it's important for me to document the features that I implement so my fellow devs know that when they add new features, or change a feature's implementation, that it still works as expected.

For front-end development, we use the Angular.js, with Karma & Jasmine for unit tests and Protractor for integration tests. Integration tests describe the high-level functionality of a webpage; they allow us to assert what happens when a user fills in a form, clicks a button, or navigates to a specific page as an admin, user, or other role. Unit tests assert fine-grained functionality about the way a particular module, directive, or service works. We use an outside-in approach to software development, meaning we write integration tests first, then unit tests, and finally the code itself. While this approach requires a bit more overhead up front, it saves us time in the long run by allowing our developers to know that they haven't introduced new bugs, and allowing our devs that review pull requests to see that the software as a whole still functions as expected. 

The point of this post is to introduce you to the testing suites that we use in house, and to show you how to set the up to test your own code. First and foremost, let's take a look at the different tools that exist, and what they aim to do:

#### Test Runners

Test runners run your tests, usually from the command line. For unit tests, we use Karma, which is test-framework and assertion library agnostic. Karma lets you use Jasmine, Mocha, QUnit, or any other library you'd like to use to write the tests (although for other test libraries, you'll have to write your own adapter to get Karma to support it). Karma allows you to execute your tests in real browsers, locally during development, and on every save, so that you can be certain at all times that your code still functions as expected. 

Karma is a Node package, meaning you'll need to install Node first. From the command line:

	git clone git://github.com/ry/node.git
	cd node
	./configure
	make
	sudo make install
	
Next you'll need npm, Node's package manager. To get it from the command line:

	curl https://npmjs.org/install.sh | sh
	
Now you can install Node packages, like Karma:

	npm install -g karma
	
The `g` flag in the script above will install Karma globally. 

