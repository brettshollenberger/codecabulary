# Stacks and Queues

Stacks and queues are fairly simple ideas, but can come across as complicated topics if you've never approached data structures before. My intent is to help you along by working backwards from our definitions. First off, stacks and queues dynamic sets. What does that mean?

## What is a set?

A set in computer science is an _abstract data structure_ that stores an unordered, discrete (non-repeating) group of elements. It is the computational implementation of a mathematical _finite set_, which also describes an unordered, discrete group of elements that is finite (non-infinite). 

## What is a mathematical set?

A set could be a group of anything--numbers, words, cities, sports mascots. It could also mix and match from these groups; it's not limited to ideologically contiguous items. More formally, a set is considered finite if there exists some bijection:

	f: S -> { 1, ..., n }
	
For some natural number `n`. A bijection is a function between the two sets that pairs each element in one set with each element in the other; some bijection exists for a set of numbers `1...n`, then the first set `S` must be finite, since the second set is inherently countable. If this language is too formal for you--consider: if each element in the first set has a pair in the second, and there are no remaining items from either list, then both sets have the same number of items. Since the numbers in the series 1 to any number must be countable, the set `S` must consist of a non-infinite number of elements.

## What is an abstract data structure?

An abstract data structure is a mathematical model for a class of data, where each member of the class shares a similar structure. An abstract data type is defined, indirectly, by the types of operations that may be performed on it, by the mathematical constraints on the effects, and possibly by the cost of these operations.

Abstract data types are purely theoretical. A set is the "idea" of an unordered, discrete group of items. An array is an ordered, non-discrete list. A hash or dictionary is the "idea" of a mapping between keys and values. Each of these ideas are useful as we describe algorithms; they provide a basis for us to talk about mathematical concepts that are important to a solution to a given problem.

## Dynamic Sets

A dynamic set is typically one that:

* Allows creation of a new, empty set structure
* Allows addition of new elements to the set (with the constraint that they are not already members of the set)
* Allows removal of elements from the set (with the constraint that the element is already present)
* Allows for querying of the size of the set (the number of items it holds)

Notice that, as we would with any abstract data structure, we talked about a dynamic set in terms of the operations it may perform and the mathematical constraints on these operations. In this case, we're choosing not to discuss the cost of these operations, since they'll vary from implementation to implementation, and our discussion is implementation agnostic.

## Back to Stacks

Stacks and queues change up the mathematical constraints of sets a bit. First: stacks and queues are described as sets because their elements are required to be unique, but they are ordered, not unordered, like sets usually are. Second: the order in stacks and queues matters because each pre-specifies which item will be deleted by the delete operation, when performed (with generic sets, the delete operation can specify a value to remove):

Stacks implement a last-in, first-out (LIFO) policy. In a stack, the element deleted from the set's delete operation is the one most recently inserted. Metaphorically stacks are often compared to a spring-loaded stack of cafeteria plates: the plates are popped off the stack in the reverse of the order they were put on the stack. A plate at the bottom of the stack may stay there forever, if people continue adding plates back before anyone takes the bottom plate. 

Queues implement a first-in, first-out (FIFO) policy. The element deleted from the queue is always the one that has been in the set for the longest time. 

## Implementing Stacks

Stacks use the language of "pushing" and "popping" elements, where a pushed element is _pushed_ onto the top of the stack, and the popped element is _popped_ off the top of the stack. The top of the stack is always the last plate we put on, which we'll want to increment as we add new plates, and decrement as we remove the most recent plate. The stack is empty when it contains zero elements. We say the stack _underflows_ when we try to pop an element off an empty stack, and it _overflows_ when `S.top` exceeds `n`.

We could write our own basic implementation of stacks using Ruby arrays like so:

	class Stack < Array
		attr_accessor :top
		def initialize(elements)
			@top = -1
			elements.each { |e| ss_push(e) }
		end
		
		def ss_push(element)
			@top += 1
			self << element
		end
		
		def ss_pop
			throw "Underflow" if stack_empty?
			@top -= 1
			self[@top+1]
			self.delete_at(@top+1)
		end
	end