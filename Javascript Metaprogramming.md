# Javascript Metaprogramming

	Object.prototype.defineMethod = function(methodName, methodBody) {
		Object.defineProperty(this, methodName, {
			enumerable: true,
			configurable: true,
			value: methodBody
		});
	}


	Object.prototype.aliasMethodChain = function(methodName, featureName) {
		this.defineMethod(methodName + "Without" + featureName.capitalize(), this[methodName]);
		this.defineMethod(methodName, this[methodName + "With" + featureName.capitalize()]);
	}

	function Doge() {
		this.defineMethod("bark", function() {
			return "woof!";
		});
	
		this.defineMethod("barkWithLogging", function() {
			console.log("Bark called!");
			return this.barkWithoutLogging();
		});
	
		this.aliasMethodChain("bark", "logging");
	}