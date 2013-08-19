# By Value versus By Reference

In JavaScript, among other languages, the syntax distinguishes between values that are stored or accessed by the value itself, or by a reference to that value.

The distinction is important: values that are accessed by the value itself are the value you expect them to be:

	var five = 5;
	five
	>> 5
	
By contrast, JavaScript reference values store a pointer to an object on the heap. If you perform an assignment statement like this:

	var obj1 = new CrazyObject();
	obj2 = obj1
	
You aren't making a copy of obj1, you're storing a second reference of the pointer to the object stored on the heap. As opposed to copying, you're actually aliasing obj1 as obj2. So when you change a property or method, you change it on both aliases:

	obj1.insanity = 1;
	obj2.insanity
	>> 1
	
#### Primitive Types are Accessed by Value; Object Types are Accessed by Reference

As shown above, complex data in JavaScript (of which there's only one type--the object type), is accessed by reference (by pointer), and thus is not copied. The primitive JavaScript datatypes (number, boolean, string, null, and undefined) are accessed by value, so they are in fact copied:

	var five = 5;
	var six = five;
	six
	>> 5
	six = 6;
	six
	>> 6
	five
	>> 5
	
