# Javascript this

_this_ is one of the most widely used and confused idioms in Javascript. But it doesn't have to be:

#### What is this?

_this_ is similar to the way we use pronouns in English. In English, pronouns are most often _reflexive pronouns_, as in the sentence:

> Kendra went to the store because she needed milk.

In the sentence above, _she_ refers back to Kendra. Often we think we use reflexive pronouns because it sounds better, but there's another good reason; consider the following sentence:

> Kendra went to the store because Kendra needed milk.

In the sentence above, we're left with a number of ways to interpret its meaning, one of which includes "There are two people named Kendra, and one is getting milk for the other." If the speaker is a rational, native-English speaker, we may actually think this is the most likely reading of the sentence. 

> In this case, the reflexive pronoun helps to clear up the meaning of the sentence. The case that uses "she" makes it clear that Kendra is getting milk for herself.

#### This in Javascript

In Javascript, we can use _this_ in a similar way. jQuery widely uses this paradigm:

	$('header').hover(function() {
	  this.callBack();
	};
	
In this case, the callback function is called on the header object. This use of _this_ can be even more useful, identifying the object _this_ refers to at runtime, which we see particular use of in MV* Javascript frameworks. 

#### Why this is Confusing

The example above is not the only use of _this_, though it is the most common, particularly among developers coming to Javascript from object-oriented backgrounds. 

> The value of this changes based upon the way the function is invoked.

There are four ways to invoke functions in Javascript, which we'll detail further below, but the big takeaway is the following chart:

| Invocation Method | Value of This                                | 
| ------------------------  |:-------------------------------------------:|
| As a function           | Window object                              | 
| As a method           | The object the method belongs to |
| As a constructor     | New empty object                         |
| Via apply() or call() | The first argument of the method | 

1) As a function - The most obvious way to invoke a function is as a function (e.g. not as an object method, constructor, or via apply() or call()). Despite its evidence as a means of function invocation, we might actually consider the function as a function invocation one quickly headed for obscurity, and for good reason:

> One of the primary gripes about Javascript is that everything is declared in the same global namespace. As applications grow larger (and MV* frameworks have been introduced to Javascript), many developers have taken to namespacing entire applications under a single object, often called app.

	app = {};
	app.AppView = Backbone.View.extend({ ...
	
In these namespaced applications, functions are not usually called directly. Instead, what would constitute a function in the global namespace now constitutes a method of the app object. As such, the value of _this_ passed to the function is actually the app object, not the window object, as it would be if it were truly declared as a function.

This gotcha is something to be aware of if you're developing Javascript applications following the latest best practices. However, assuming the function is actually invoked as a function in the global namespace, the value of _this_ is the window object:

	function globalFunction() { return this }
	globalFunction();
	> Window {top: Window, window: Window, location: Location, external: Object, chrome: Object…}
	// Note: this is the value returned in Chrome; it will contain different properties given the browser you're working in

2) As a method - This one should be all too familiar to developers from object-oriented backgrounds, but let's be clear: a method is a function that belongs to an object, as in the example above. For the green behind the ears:

	cat = {
	  meow: function() {
	    console.log('meow');
	  }
	};
	
In this example, the meow "function" is bound to cat as a method (a function as an object property), and can only be invoked via the cat object. If we tried to invoke the method in the global namespace:

	meow();
	> ReferenceError: meow is not defined
	
	// However:
	
	cat.meow();
	meow
	undefined // Note, undefined is returned from the method, since there is no explicit return
	
As noted above, the value of _this_ in a method invocation is what object-oriented programers will have come to expect of a property like _self_--the object itself:

	cat = {
	  objectMethod: function() {
	    return this;
	  }
	};
	
	cat.objectMethod();
	> Object { objectMethod: function }
	
As we see above, calling an object's method passes the object itself as the value of _this_.

3) As a constructor - Constructors are used in Javascript for inheritance, in a fashion object-oriented programmers will be familiar with; classes in more traditional object-oriented languages come with a constructor function for free that instantiates the class (creates an instance of the object):

	function Cat() {
	  this.objectMethod = function() { return this }
	};
	
	cat = new Cat();
	cat.objectMethod();
	> Cat { objectMethod: function }
	
As we see above, the difference in the use of a constructor function from "function as function" invocation or method invocation is--its invocation. To invoke as function as a constructor, we use the _new_ keyword, followed by the name of the constructor, which is capitalized, like class names in other languages.

While the value of this may appear to be similar to the value of this in a method invocation, it's subtly different. A constructor invocation passes a new _empty_ object as _this_ to the function. If there's no explicit return statement in the constructor, the constructor returns the newly instantiated object, which we see as the value of this when it's called later on the cat object. To get a sense of the initial _empty_ state of the _this_ object when it's passed to the constructor, we can examine the following code snippet:

	function Cat() {
	  return this;
	  this.someMethod = function() { };
	}
	
	cat = new Cat();
	> Cat { }
	
	cat.someMethod();
	> TypeError: Object #<Cat> has no method 'someMethod'
	
Here we see the empty cat object returned. When we try to call the someMethod method on cat, we get a TypeError (no method), because the someMethod part of construction was never evaluated--we returned the value of this first. It may be obvious, but to be more explicit:

	function Cat() {
	  this.someMethod = function() { };
	  return this
	  this.someOtherMethod = function() { };
	};
	
	cat = new Cat();
	> Cat { someMethod: function };
	
As we see above, the constructor adds to the empty object in the order of construction. Since we declare someMethod before returning the object, someMethod has been added to the empty object upon return, although someOtherMethod is never evaluated, or added, to the new object.

4) Via apply() or call()

Call() and apply() give us immense power when invoking functions, because they allow us to define the value of _this_. Call() and apply() are very similar: 

	someFunction.apply(objectToBeThis, [array, of, arguments]);
	
	someFunction.call(objectToBeThis, arguments, to, iterate, over);
	
The following example should elucidate this concept further:

	a = { };
	b = [1, 2, 3];
	
	function Adder() {
	  holder = [];
	  for (i=0; i < arguments.length; i++) {
	    holder.push(arguments[i]);
	  };
	  this.newArray = holder;
	};
	
	Adder.apply(a, b);
	a.newArray
	> [1, 2, 3]
	
The example above is extraordinary trivial, but we can see already how much power we have in our ability to make anything _this_. At the end of our Adder() function, we see that we create a newArray property on whatever object is _this_, in this case, the a object, which allows us to add properties dynamically.

#### Conclusion

As stated earlier, the following chart should be the big takeaway from this article. With it, you'll never need to be confused about the value of _this_ again, as long as you remember that the value of this is directly tied to the means of method invocation:

| Invocation Method | Value of This                                | 
| ------------------------  |:-------------------------------------------:|
| As a function           | Window object                              | 
| As a method           | The object the method belongs to |
| As a constructor     | New empty object                         |
| Via apply() or call() | The first argument of the method | 
	
	


	

