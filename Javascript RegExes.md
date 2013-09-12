# Javascript Regular Expression Testing

In Javascript, Regular Expression objects have a `test` method, which allows us to test a string:
	
	/\d{1, }/.test("Jenny, don't change your number");
	> false
	
	/\d{1, }/.test("Jenny, don't change your number 867-5309");
	> true
	
Regexes have many uses regardless of the language we're using. In Angular, we might use a regex to test whether or not we're at an Admin URL, and to display the appropriate navbar if we are:

	$scope.onAdmin = /\/admin/.test($location.path());
	
