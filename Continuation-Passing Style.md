# Continuation Passing Style

Continuation passing style is an approach to programming single-threaded systems in a non-blocking manner. In CPS, functions do not return their results to their caller--in fact they don't return to their caller ever. Instead, their caller registers a callback, which is passed the "return value":

	Blocking:
	function id(x) {
		return x;
	}
	
	Continuation-passing style:
	function id(x, cb) {
		cbv(x);
	}
	
	CPS Factorial:
	function factorial(n, then) {
		if (n === 0) {
			then(1);
		} else {
			factorial(n-1, function(result) {
				then(n*result);
			});
		}
	}
	
	CPS Tail Factorial:
	function tail_factorial(n, r, then) {
		if (n === 0) {
			then(r);
		} else {
			factorial(n-1, n*r, then);
		}
	}
	
	function factorial(n, then) {
		return tail_factorial(n, 1, then);
	}
	

	