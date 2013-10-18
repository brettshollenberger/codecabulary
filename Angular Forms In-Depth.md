# Angular Forms In-Depth

Angular forms consist of three layers:

* The directive layer - Angular provides directives that override HTML elements, like `<form>` and `<input>`. These extensions provide a declarative syntax for defining the form. They are used to declare the name of the form, its inputs, and its validations. They should be used to declare a user interface for updating model information, and provide unfortunate code duplication by way of validations that should be improved upon (in my personal opinion). An improved version of Angular would allow model classes to both represent information and provide an interface for persisting that information, which forms would hook into; forms in this improved Angular would ask the model attributes for their validators, and declare HTML classes for validity based on those model attributes.
* The data-binding layer - Angular hooks into instances of model objects, and provides getters and setters to update the model objects and the views as a user types in information. This binding is two-way, meaning updates in the view are immediately propagated to the model instance, and vice versa. This is achieved through the `ng-model` directive and model objects exposed on`$scope` objects, and is an area where Angular shines in comparison to other Javascript frameworks.
* The directive-controller layer - Angular's directives (`ng-model`, `ng-form`) provide an interface for working with them via directive controllers (`ngModelController`, `ngFormController`). `ngModelController` and `ngFormController` work together to declare this API. 


#### The Directive-Controller Layer

`ngModelController` presents four types of methods and properties for interacting with the data-binding. The first type verifies whether the user has interacted with the input (`$pristine`, `$dirty`), and allows us to set the value dirty or pristine (`$setPristine`, `$setDirty`); the second type parses information from the view into the format used in the model (`$parsers`), and formats the model data for display in the view (`$formatters`); the third type verifies the validity of the input (`$valid`, `$invalid`), and allows us to set whether or not the input is valid ourselves (`$setValidity`); and the fourth type allows us to retrieve the view or model value (`$viewValue`, `$modelValue`) and to set these values (`$setViewValue`).

`ngFormController` creates the form, and allows us to check the dirty, pristine, valid, or invalid state of the form as a whole. When the form is created initially, it is empty (it contains no inputs); `ngFormController` exposes the public methods `$addControl` and `$removeControl` that `ngModel` hooks into. When `ngModel` is declared on an input, that input's `ngModelController` is added to the `ngFormController`'s controls array, and is exposed as a property on the form object. When put together, the form object contains both its public interface, and the public interface of each control, allowing us to work with the form and its fields. One way this system could be improved upon is by setting the inputs on a fields attribute on the form object in order to separate them from the form's public interface, and allow them to be iterated over without iterating over the rest of the interface. `ngForm` establishes event listeners for each of its controls in order to set the validity and dirty state of the form as a whole. 

#### The Directive Layer

The directive layer allows us to declare forms, which inputs belong to them, and which validations exist on specific inputs. 

	<input type="email" ng-model="user.email" required>
	
Establishes an input that hooks into its parent form and the `$scope` object's model instance via the `ng-model` directive and `input` directive. The type="email" directive establishes a validator that validates whether or not the user has entered a valid email address (and accordingly sets the validity of the input and its parent form), and the required directive operates in the same manner, establishing a validation that the user has input text into the field.

The input and `ng-model` directives should be a part of this layer, whereas according to the ActiveRecord design pattern, the model validations should be handled by the model class and hooked into via the `ngModelController`. 

#### The Data-Binding Layer

`ngModel` watches the view value and model value and updates each accordingly. It verifies the validity of inputs based on the directives defined on them, and adds CSS classes `ng-dirty` and `ng-pristine` accordingly. `ngModel` also uses the `validationErrorKey` (`required`, `email`) to set specific classes, like `ng-invalid-email`, which can be hooked into to define specific feedback to the user. The inputs and form also expose an `$error` object which tracks these errors, and can be used for presenting error messages to the user. 
