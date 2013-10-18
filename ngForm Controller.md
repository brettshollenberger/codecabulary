# ngForm Controller

The `<form>` directive instantiates ngFormController, an API for working with Angular forms to set their validity or determine whether they've been interacted with by a user. Let's step through the code to see how it works.

Angular first determines the form element that the controller refers to, and checks to see whether or not the form element is nested (you can nest form elements as long as the nested elements are named `mg-form`). If a form element is nested, it's added as a control to the parent form controller, so that the parent form can check the validity of the entire form. 

Angular also establishes an empty errors hash, controls array, and the convenience method toggleValidCss, which takes a signature of (isValid, validationErrorKey). Angular uses this convenience class to build custom CSS classes using your validationErrorKey (e.g. valid-required or invalid-numericality). It also assigns these classes to the form based on the boolean isValid. 

## Adding Controls

The `ng-model` directive calls $addControl from ngForm's public API in order to add the ngModelController to the formController's controls array, and to add the field as a property to ngFormController, provided the property does not already exist. $addControl expects the control passed to be an ngModel object, since it expects the object to implement a $name property, which it uses to name the property on ngFormController. ngFormController does no fancy checking to see whether or not a control of the same name already exists on it; if the control already exists, it is overwritten. It is no longer considered in the form's validity or pristine/dirty state. It's important that each of your input fields have separate names. 

This can require a good sense of schema design if, for instance, you need to perform an ng-repeat to add controls. Not only must you add controls with unique names, but the `ng-model` of your controls must be a property on an object. Arrays of primitives (strings, numbers, etc) won't work out because `ng-repeat` creates a child scope for each iteration, and primitives offer value-based inheritance while objects offer reference-based inheritance. Value-based inheritance means copying the value of the parent property to a new primitive on the child scope, meaning that updates to the child scope won't affect the parent property--the model. Reference data on the other hand contains a pointer to an object on the heap; the reference is copied to the child scope, and anything updated on the reference merely points to the actual object on the heap and updates it. For instance, a design like this one won't allow changes made on the email input to be reflected in user.email:

	<div ng-repeat="email in user.emails">
		<input ng-model="email">
	</div>
	
Each `ng-model` in this paradigm becomes isolated, and isolation breaks `ng-model`. The best practice here is to remember to set both the `ng-model` attribute and a unique name attribute to each input, and remember for `ng-model` to be a property on an object, since objects are passed around referentially (as pointers to an object on the heap), and therefore each property refers to the property on the same object, regardless of scope. 

	$scope.applicants = [
          	{name: "Brett", email: "brett.s@gmail.com", references: [{name: "Corey", email: "corey.p@gmail.com"}, {name: "Tag", email: "tag@gmail.com"}]}
        ];
        
        <h2>Application</h2>
        <div ng-repeat="applicant in applications">
          	<h4>Personal Info</h4>
          	<input name="applicant.name" ng-model="applicant.name" required test-dir>
          	<input name="applicant.email" ng-model="applicant.email" required test-dir>
          	<h4>References</h4>
          	<div ng-repeat="reference in applicant.references">
            		<input name="reference.name" ng-model="reference.name">
            		<input name="reference.email" ng-model="reference.email">
          	</div>
        </div>
For more information, check out http://javascriptissexy.com/javascript-prototype-in-plain-detailed-language/, and to learn more yourself, you can always check out scope.__proto__ to see its prototype property (same as its $parent property, which is exposed on its API). When designing your models, it's important to remember this fact about the language you're working in; if you're using an array type to store values, that array should store objects or objectIds, not primitives or strings. 

