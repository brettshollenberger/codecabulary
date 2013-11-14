# Javascript Inheritance

	function Parent(name) {
		this.name = name;
	}
	
	Parent.prototype.say = function() { return this.name; }
	
	function Child() {};
	
	function inherit(C, P) {
		C.prototype = new P();
	}
	
	var bert = new Child('bert');
	
	bert.say();
	>> 'bert'
	
Drawbacks: most of the time you don't want the own properties because they're specific to an instance.