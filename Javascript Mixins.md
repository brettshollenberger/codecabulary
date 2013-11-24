# Javascript Mixins

In a language like Ruby, when we create a mixin, we actually hijack the inheritance hierarchy, and sandwich our mixed in module between a particular class and its parent class, as a type of pseudo-parent class.

	class Person
		include Enumerable
		def each
			yield @name
			yield @email
		end
		
		def initialize(name, email)
			@name = name
			@email = email
		end
	end

This gives our class all the enumerable methods that are present in a class like Array or Hash, allowing us to say something like:

	person.map { |property| property + "very good!" }

In this type of pattern, the inheritance hierarchy is important. If we define something in our parent class, and then define the same property in our mixin, the mixin property will win. Likewise, if we override the property in the class itself, it will beat out the mixin's version of the property. 

Many Javascript libraries include a function called `extend` which is meant to mimic the mixin pattern. For example, both Angular and Lodash include such a function:

	angular
	  	.module('app', [])
	  	.factory('Person', ['mixin', function(mixin) {
		    var returner = {cool: false};
		    angular.extend(returner, mixin);
		    return returner;
	  	}])
	  	.provider('mixin', [function() {
	    		this.$get = function() {
		      		return {
		        			cool: true
		      		};
	    		};
	  	}]);
	  	
	  it('overwrites the cool property', function() {
	      expect(main.cool).toEqual(true);
	   });
	   
As we see in the example above, mixins don't work quite the same way in Javascript, at least not in these popular libraries. Properties defined on the mixed in module will overwrite properties defined in the objects that inherit from them. This is neither the functionality we'd expect or desire. So today we're going to talk about creating a mixin function that provides functionality similar to Ruby. 

The simplest way to do this is just to check whether or not the receiving object already contains a property of the same name:

	function mixin(receiver, giver) {
		for (var i in giver) {
			if (!receiver.hasOwnProperty(i)) {
				receiver[i] = giver[i];
			}
		}
	}
	
However, a problem that we confront here is that this pattern still doesn't allow us a lot of control over how those properties are defined on our child object. We can see a good example of the problems this creates if we were to create an Enumerable mixin similar to the one found in Ruby. First we would declare the 'each' method on our array, since the Enumerable mixin requires us to implement such a method, basically telling the mixin how to enumerate over the properties of the object:

	Object.defineProperty(Array.prototype, 'each', {
		enumerable: false,
		value: function(cb) {
			for (var i in this) {
				cb(this[i]);
			}
		}
	});
	
Now if we were to create an Enumerable mixin that simply defines functions:

	var enumerable = {
		all: function(cb) {
			var truth = true;
			this.each(function(i) { console.log(i); if (!cb(i)) truth = false; });
			return truth;
		}
	}
	
	mixin(Array.prototype, enumerable);
	
We'll consistently run into errors because we notice that the `all` method we mixed in is being counted as one of the properties on our array. 

	[1, 2, 3].all(function(num) { return num > 0; });
	> false
	
Since Javascript doesn't know how to compare our function with zero, we get the answer false, even though we'd expect it to be true that 1, 2, and 3 are greater than zero. Again, this isn't the functionality we'd expect. What we need is more fine-grained control over how our properties get mixed in. So what we'll do is come back to the Object.defineProperty method, and notice that the last argument it receives is a definition object that provides us with this fine-grained control. We can use this to our advantage when we're writing our mixin function:

	function mixin(receiver, giver) {
		for (var i in giver) {
			if (!receiver.hasOwnProperty(i)) {
				Object.defineProperty(receiver, i, giver[i]);
			}
		}
	}
	
Now in our mixin module, we can define each property with more control:

	var enumerable = {
	  	all: {
	    		enumerable: false,
	    		value: function(cb) {
	      			var truth = true;
	      			this.each(function(i) { if (!cb(i)) truth = false; });
	      			return truth;
	    		}
	  	},
	  	any: {
	    		enumerable: false,
	    		value: function(cb) {
			      var truth = false;
			      this.each(function(i) { if (cb(i)) truth = true; });
			      return truth;
	    		}
	  	},
	  	collect: {
	    		enumerable: false,
	    		value: function(cb) {
			      var returner = [];
			      this.each(function(i) { returner.push(cb(i)); });
			      return returner;
	    		}
	  	},
	  	first: {
	    		enumerable: false,
	    		value: function(num) {
			      var returner = [];
			      var num = num || 1;
			      var count = 1;
			      this.each(function(i) { if (count <= num) returner.push(i); count++; });
			      return returner;
	    		}
	  	},
	  	include: {
	    		enumerable: false,
	    		value: function(what) {
			      var truth = false;
			      this.each(function(i) { if (i == what) truth = true; });
			      return truth;
	    		}
	  	}
	};
	
Mixing these properties in now provides the functionality we'd expect:

	mixin(Array.prototype, enumerable);