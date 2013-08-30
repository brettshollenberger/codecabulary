# Conditionally Apply Classes with ng-class

`ng-class` allows you to set CSS on a particular HTML element dynamically by data binding an expression that represents all classes to be added:

	<div ng-class="{expression}">
	
The Angular docs on this one kind of suck, but you can use conditional syntax like Ruby's shorthand statement modifiers:

	applyClass if true # An if statement modifying whether or not to apply the class, as in plain English
	
	applyClass unless true # And an unless statement doing the same
	
Angular allows us to use the `:` operator to perform the equivalent of an `if` modifier:

	<div ng-repeat="project in projects" ng-class="{ 'clearfix' : $index % 3 == 0 }">
	
`ng-repeat` tells us which collection to iterate over, and `$index` is a built-in referring to the number of the current iteration. In my markup, I'll need to put every third project on a new line so they format correctly. Using the modulus operator, we can determine whether or not the number of the current iteration is divisible by three, and apply the classic clearfix (`clear: both;`) if it is.