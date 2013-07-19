# Javascript Prototype

In Javascript, the concept of prototype is split into two sub-concepts, the prototype property and the prototype attribute; let's examine the first:

#### Prototype Property
Javascript functions have properties--variables that are defined on them. By default, every Javascript function comes with a prototype property, which is used for inheritance.

If you add methods and properties to a function's prototype property, they are available to instances of that function.

	function HouseCat(name, meow) {
	  this.name = name;
	  this.meow = meow;
	}
	
At this point, the prototype property is empty:

	HouseCat.prototype
	> HouseCat {}
	
But if we add to the prototype:

	HouseCat.prototype.makeMeow = function() {
	  console.log(this.meow);
	};
	
We now see:

	HouseCat.prototype
	> HouseCat {makeMeow: function}
	
And we can call `makeMeow()` on instances of HouseCat:

	var scout = new HouseCat("Scout", "mrow mrow");
	scout.makeMeow();
	> mrow mrow
	
#### Prototype Attribute:
The prototype attribute is a characteristic of an object that specifies who the object's parent is. The prototype attribute is also called the prototype object (similar to a class in other languages); the prototype object is set automatically when an new object is instantiated.

In many other languages, we establish classes that establish variables and methods for instances of the class, and in these languages, a constructor function often comes free with definition of the class.

For example, in Ruby:

	class HouseCat
	  attr_reader :name, :meow
	  
	  def initialize(name, meow)
	    @name = name
	    @meow = meow
	  end 
	  
	  def makeMeow
	    print @meow
	  end
	end
	
We define a class in the manner above. We haven't defined a constructor, but Ruby gives us one for free:

	scout = HouseCat.new("Scout", "mrow mrow")
	
And when we call `scout.class`, we get `HouseCat`.

In Javascript, to perform the same basic functionality, we define a constructor function, which includes the object's properties and methods, as in a more traditional class-based system. We've seen an example above. So when we call:

	var scout = new HouseCat("Scout", "mrow mrow");
	
We set `scout`'s prototype object to `HouseCat.prototype`.

Or if we just use the Object constructor:

	var scout = new Object();
	
We set `scout`'s prototype object to `Object.prototype`

This is the same as if we'd done this in Ruby:

	scout = Object.new
	scout.class
	> Object
	
As in Ruby or any other high-level language, instantiating a new object from `Object.prototype` in Javascript is poor form. It's based on the assumption that the object is a one-off, and that no other object in the future will be like it, so it doesn't require a constructor (or developer-defined class). You can do it, but it's not exactly future-proof. 

#### Prototype-Based Inheritance is Like Class-Based Inheritance
Javascript's prototype functionality is akin to class-based inheritance in other languages:

	function Cat() {
	  this.meow = "meow";
	  this.makeMeow = function () {
	    console.log(this.meow);
	  };
	};
	
You could define a prototype in this way in Javascript, which is akin to creating an Abstract Base Class in some other language. You don't expect Cat itself to be instantiated, though it could be. In that case, it's best to set reasonable defaults, such as `this.meow = "meow"`. This default is akin to a hook method--one that children can override if they see fit. For example:

	function Tiger() {
	  this.meow = "growl";
	}
	
Tiger doesn't yet have a way to growl (makeMeow), but it will if we establish Tiger's prototype:

	Tiger.prototype = new Cat();
	
It's best not to require arguments for the Cat constructor, so as to allow for the greatest flexibility among base classes.

Now:

	var tony = new Tiger();
	tony.makeMeow();
	> growl
	
But, we could also accept the reasonable default "meow" in our HouseCat constructor:

	function HouseCat(name) {
	  this.name = name
	};
	
	HouseCat.prototype = new Cat();
	
	var scout = new HouseCat('scout');
	
	scout.makeMeow();
	> meow
	
Notice that the HouseCat constructor made no mention of either meow or makeMeow; our prototypal Cat constructor took care of that. In this way, prototypal inheritance is _very_ similar to class-based languages. 

Note that property and method lookup works the same way in Javascript as it does in truer object-oriented languages. First the JS runtime checks whether or not `makeMeow` is defined on the object itself--Scout, and then it moves to HouseCat, the object's prototype. If it isn't found on the object's prototype (HouseCat), it moves to the prototype object's prototype (Cat), and so on. 

Properties and methods defined on Object.prototype will eventually be found there. Scout can implement hasOwnProperty() (defined on Object.prototype), since it is within the prototype chain. 
	