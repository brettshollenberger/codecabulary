# Truthy and Falsy

Little in the "real" world is black and white, but in the world of boolean logic, computers will find a way to evaluate any statement, object, or variable as true or false. As humans, we can conceive of such statements as:

	1 < 2 			## true
	ships.sunk?		## if true, a game of Battleship is over
	
But what about statements like:

	if chair		## is a chair inherently true or false?
	unless 1		## what about the number 1?
	
These statements would sound more than a little crazy in natural language, but in computer science they often have a sound purpose: to check whether an error has occurred, whether a user has entered some value, or whether something unexpected has happened.

For instance, we might say:

	if @user
	
To determine whether or not a user object exists. This statement will evaluate to true if a user is signed in, and false if a user is not. 

_But Boolean logic varies across languages._ Let's look at some of the differences between Ruby and Javascript:

| Type           | Language              | Boolean Value                              | 
| --------------  |:------------------------:|:--------------------------------------------|
| Nil/ null       | Ruby/Javascript    | False                                            |
| Undefined  | Ruby                     | Does not exist                               |
| Undefined  | Javascript             | False                                              |
| Boolean     | Ruby/Javscript      | True/False                                      |
| Number     | Ruby                     | True                                                |
| Number     | Javascript             | 0 and NaN are false; else: true      |
| String        | Ruby                     | True                                                |
| String        | Javascript             | True unless empty string                |
| Object      | Ruby/Javscript      | True                                                |
 
Knowing the truthiness and falsiness of various types in your languages of choice is important--you can see it's easy to get tripped up when transitioning!