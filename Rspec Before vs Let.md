#RSpec before vs let

In RSpec, there are two ways to DRY up tests (before and let) that share an intersecting purpose: to create variables that are common across tests. For common variable instantiation, the Ruby community prefers _let_, and while _before_ is often used to perform actions common across tests, the purpose of this article is to explore the differences between before and let for common variable creation in order to explain why let is preferred. 

#### before creates instance variables; let creates lazily-evaluated local variables

Let variables don't exist until called into existence by the actual tests, so you won't waste time loading them for examples that don't use them. They're also memoized, so they're useful for encapsulating database objects, due to the cost of making a database request. 

	let(:valid_user) { User.find_by_email(email) } # Let queries the database once, and then saves the valid_user object locally
	
	before { @valid_user = User.find_by_email(email) } # Before queries the database before each spec. 

Before statements are run _before each test_, and increase load times. When you are going to `visit root_path` before a number of tests anyway, a before statement makes sense: you'd be calling the visit method the same number of times and before DRYs up your tests. For variables, you end up using more processing power unnecessarily.

#### instance variables default to nil

Instance variables spring into existence (if they weren't previously defined) as _nil_. That means typos in before blocks can be nefarious, allowing certain types of tests to pass when they shouldn't.

	before(:each) do
		@user = User.find(username: "belleandsebastian")
		@user.logout
	end
		
	it "should log the user out" do
		expect(@usr).to be_nil
	end

In this example, the actual test has a typo:

	expect(@usr).to be_nil
	
Since @usr wasn't previously defined, the test will pass, but not because @user is logged out, but rather because @usr wasn't previously instantiated, and so _is nil_ by default.

The same test using let would raise NameError because usr isn't defined.

	let(:user) { User.find(username: "belleandsebastian" }
	before { user.logout }
	
	it "should log the user out" do
		expect(usr).to be_nil
	end
	
Here, the test fails, as we expected, and Ruby alerts us of our typo"

	NameError: undefined local variable or method 'usr'

Notice we still utilized a before statement in this example, since we needed to call the logout method on user. Before statements have a purpose, but variable definition is better served by let. 



