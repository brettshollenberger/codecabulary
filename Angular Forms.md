# Writing Angular Forms

Angular allows us to write custom form validations, and provides us with a number of directives to tap into.

1) The HTML tag form is actually overridden in an Angular app by the form directive, which instantiates FormController, which keeps track of the form's controllers and the state of each field (whether it's been filled out, is valid, invalid, etc). If we add a name for this form, it the form constructor will be published to the current scope, so we can access it easily:

	<form name="MyGreatForm" novalidate>
	
Now form attributes can be accessed like:

	MyGreatForm.name
	
And errors for a particular field can be accessed via the $error object:

	MyGreatForm.name.$error
	
You'll also notice we've added the `novalidate` attribute to the form field to disable HTML5's default validations, and to allow us to validate via Angular.
	
2) To handle validations, we can use a number of directives; the two most common, however, are `ng-submit`, which is usually used to check whether all form fields are valid on form submission, and `ng-blur`, which is usually used to check individual form fields after a user navigates away from it. 

From a UX standpoint, `ng-blur` is the preferred behavior. It performs validations directly after a user navigates away from a form field; not as they're typing or after they've submitted. It allows a user to declare they're finished with a field, and to receive feedback immediately; filling out the same form twice can cause frustration, and decrease the number of forms users fill out.

Unfortunately, `ng-blur` is only available on the bleeding edge versions of Angular, not the latest stable release. You can, however, create your own directive to perform the same functionality (https://github.com/brettshollenberger/FacultyUI/tree/master/directives/ngBlur).

Whichever directive you choose to handle validation, the basic format is to pass the directive the function invocation as a string. Angular's `$parse` service will convert this expression to a function, and call it to handle validations:

	<input type="text" ng-blur="validateName()">
	
