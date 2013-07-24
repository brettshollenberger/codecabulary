# Faking Methods w/ Call() and Apply()

Wouldn't it be cool if we could just _push_ onto an object like we could in an array? Turns out, we can!

Since call() and apply() allow us to specify the value of _this_, we can write a little ditty like this:

	var obj = {
	  push: function(el) {
	    Array.prototype.push.call(this, el);
	  }
	};
	
Here we hook into the push method via the Array prototype, but call it on _this_, the current object. Meaning if _obj_ were a constructor, or we added the push method to another prototype, all instances could also benefit from the franken-push method. 