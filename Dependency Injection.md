# Dependency Injection

Since good classes are singly-responsible--they perform one task and do it well--classes will necessarily need to interact with one another in order to accomplish more complex work. 

When behavior is implemented in one object, and we need access to that behavior in another, we say that the class that needs the behavior has a dependency on the one that implements it. The class that needs it fails as soon as it cannot access the right behaviors.

Dependencies are a major reason to care about code style because poor dependency management can turn minor code tweaks into major overhauls, with the results of the minor tweaks cascading throughout the application, forcing changes in other objects--the users of the object that's changing.

Although some degree of dependency is inevitable among any objects that must communicate, unnecessary and time-consuming dependencies can result from an uninformed coding style. Lots of developers have made these mistakes before us, and fortunately we can learn from them. Sandi Metz describes the most common and unnecessary dependencies as:

* An object knows the name of another class. 
* An object knows the name of a message it intends to send to an object other than itself.
* An object knows the arguments that a message to another object requires.
* An object knows the order of those arguments.

Each of these dependencies creates a chance that the object using the other will be forced to change when the implementing object makes a change to its public interface. 

#### How to Inject Dependencies

Let's assume we have a payroll department at a school tracking its teachers' pay rates:

	class Payee
		attr_accessor :tax_rate
		
		def initialize(tax_rate)
			@tax_rate = tax_rate
		end
		
		def post_tax_pay
			Teacher.new(name).salary - (Teacher.new(name).salary * @tax_rate)
		end
	end
	
Here the Payee's post-tax pay relies on calling an instance of the Teacher class. It can't interact with any other type of employee, even though other employees would respond to the `salary` method. Our class is unnecessarily attached to this static type, when more general duck typing would serve us better. 

	class Payee
		attr_accessor :tax_rate, :employee
		
		def initialize(tax_rate, employee)
			@tax_rate = tax_rate
			@employee = employee
		end
		
		def post_tax_pay
			@employee.salary - (@employee.salary * @tax_rate)
		end
	end
	
	Payee.new(33.3, Employee.new("Margaret Persons"))
	
The technique shown above is very subtly different, but much more reusable and less susceptible to unnecessary change. The Payee now knows much less about the Employee class--to use it, we still need to know how to instantiate the Employee class, but this reduces the number of areas we need to make future changes, and opens the Payee class to being reusable in other applications with different classes that respond to a `salary` method. Dependencies should be concise, explicit, and isolated. 

#### How to Isolate Dependencies

Okay, so we've injected the instantiated class, and that's a good start, but now let's focus on the fact that objects must send messages to objects other than self. We didn't inject this dependency only to not use it, right? Right. 

In the class we've already created, there are already two references to Employee's salary method. In the payroll department, we'll likely have dozens more, but what if the employee class decides to change the name of its `salary` method to `base_rate`? In a modern text editor, it wouldn't be much of an issue to perform a global find and replace, but our code will be much easier to change if we isolate the dependency on the foreign method to a local implementation:

	class Payee
		attr_accessor :tax_rate, :employee
		
		def initialize(tax_rate, employee)
			@tax_rate = tax_rate
			@employee = employee
		end
		
		def salary
			@employee.salary
		end
		
		def post_tax_pay
			salary - (salary * tax_rate)
		end
	end
	
Now the side-effects of changing the implementation of the Employee class are isolated, and the intentions of the Payee class are explicit: the Payee's salary is always the salary of the injected employee object. No more digging through complex methods to make changes whenever the Employee class decides to release a new public interface. Just change our local implementation of the salary method, and we're done.

#### How to Overcome Argument Order

We know that we need to instantiate our Payee class with a tax rate and an instance of the Employee class, and we also know what order we need to pass those arguments in. But that's a lot for a potential user of our Payee class to have to know, and just imagine if the Payee class grows!

One way to overcome this challenge is to prefer instantiation via a single object--a hash of options, which we can make even more secure by explicitly defining defaults in the event a certain value isn't passed:

	class Payee
		attr_accessor ;tax_rate, :employee
		
		def initialize(args)
			@tax_rate = args[:tax_rate] || 33.3
			@employee = args[:employee] || Employee.new
		end
	end
	
	Payee.new({tax_rate: 33.3, employee: Employee.new({name: "Margaret Persons"}))

Now we provide ourselves with explicit documentation about the needs of the object we're instantiating, and instead of relying on order, we rely on the names of the arguments themselves. 

			