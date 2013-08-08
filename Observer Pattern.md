# Observer Pattern

The observer pattern is a solution to the challenges of creating an integrated system--one where the parts are aware of one another's state, as well as the state of the whole. This problem is particularly challenging to solve in a way that doesn't inexorably couple the classes together in a mess of spaghetti code, and the observer pattern's solution to that challenge is to factor out the observation code into its own distinct concern.

#### Publishers and Subscribers

So what exactly does the observer pattern look like? In the days of Facebook and Twitter, it's fairly easy to conceive of a system of publishers and subscribers wherein all your friends or followers receive updates whenever your push them out, and vice versa. 

Under this pattern, a publisher class (often called the "subject" class) has a three primary responsibilities: 1) Manage an extensible list of observers, 2) Provide a simple interface to add or remove observers, and 3) Provide a simple interface to update observers. 

1) A publisher class should manage an easily extensible list of observers:

	def initialize
		@observers = []
	end
	
2) And provide an interface for adding and removing observers:

	def add_observer(observer)
		@observers << observer
	end
	
	def remove_observer(observer)
		@observers.delete(observer)
	end
	
3) And provide a clean interface between the news source and consumers of that news:

	def update_observers
		@observers.each { |observer| observer.update(self) }
	end
	
The Gang of Four defined the observer pattern as being this clean interface between the news source and the consumers, and it's the part of the implementation to focus most on. 

#### Clean News Interface

I would argue that to provide a truly clean implementation of the observer pattern, your updates should be atomic processes--they should all succeed or fail together, and observers should only be updated when they entire process has been successful:

