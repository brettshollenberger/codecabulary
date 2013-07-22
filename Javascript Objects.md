# Javascript Objects

#### Javascript Data Types
There are six data type in Javascript. Five are primitive, immutable data types--string, integer, boolean, null, and undefined, which should be familiar from other languages. The sixth, object, is a complex, mutable data type consisting of unordered key-value pairs of primitive data types, as in a hash (Ruby) or dictionary (Python).

#### The Object Data Type
These key-value pairs are called properties in Javascript; if the properties are functions, they're called methods.

	var cat = {
	  name: "Scout",  // Property
	  age: 2,
	  meow: function() {  // Method
	    console.log("meow");
	  }
	};

#### Naming Object Properties
Property names in Javascript can be strings or numbers. String-typed names can be accessed via the dot syntax or the bracket syntax, which may be familiar from other languages:

	cat.name 
	> Scout
	cat['name']
	> Scout
	
Number-typed names must be accessed via the bracket notation:

	cat.4
	> Syntax Error
	cat['4']
	> undefined

#### Reference Data vs Primitive Data
Basic variables store primitive data types:

	var cat = "Scout"
	
Primitive data types are stored directly on the variable, so if you make a copy of the data, you make a full copy:

	var cat2 = cat
	cat = "Ruby"
	cat2
	> Scout
	
As we see in the example above, changing the value of the primitive String datatype on cat has no effect on the copy, since the copy's data was stored directly onto a new variable.

Objects' properties, on the other hand, store a reference data type, which is distinct from the primitive data type in that it stores a reference, not data directly onto the variable.

	var cat = { name: "Scout" }
	var cat2 =  cat
	cat.name = "Ruby"
	cat2.name
	> Ruby
	
We see here that updating the value of a reference data type updates all references to that data. 

#### Properties Have Attributes
Objects' data properties (aka not methods) consist of a key-value pair and three default attributes:

	* Configurable: Specifies whether the property can be deleted or changed
	* Enumerable: Specifies whether the property can be returned in a for/in loop
	* Writable: Specifies whether the property can be changed

#### How to Create Objects:
Objects are created in two ways:

1) Object Literals
Object literals are defined using the curly bracket notation:

	var cat = {};
	var house = {
	  address: '248 Brown Street',
	  city: 'Waltham',
	  state: 'MA'
	};
	
2) The Object Constructor
To call the constructor, use the `new` keyword:

	var cat = new Object();
	cat.name = "Scout";
	cat.meow = function() {
	  console.log('meow');
	};
	
#### Patterns for Creating Objects

Object-oriented programming relies on the ability to create classes, which you can think of like taxonomy. A class is a codification of the properties of an essential type of thing; taxonomists work to get as precise as possible regarding the differences between different species, so that a lion is fundamentally different from a house cat. When you instantiate a class, you create a single member of that essential type (an instance); if you create the house cat "Scout", you can also create the house cat "Ruby" from the same essential house cat class.

#### 1) Constructor Pattern

Javascript doesn't support "classes" in the traditional object-oriented sense, but we can create constructor functions (like `new Object`) to define something like classes in Javascript:

	function HouseCat (name, meow) {
	  this.name = name;
	  this.meow = function() {
	    console.log(meow);
	  };
	};
	
	var scout = new HouseCat ("Scout", "mrow mrow mrow!");
	scout.name
	> Scout
	scout.meow
	> mrow mrow mrow!
	
Now if you want to change the HouseCat function, you can do so in a centralized location. To describe new instances of HouseCat, you only need describe their differences (their name and meow), not the myriad ways in which they are functionally similar. 

#### Prototype Pattern

The problem with the constructor pattern is that methods are created once for each instance. The prototype pattern solves this by leaving the constructor empty, and defining properties on the prototype directly. The prototype is actually more akin to 

