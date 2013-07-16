# Backbone Models

Backbone models hold application data and logic around that data, as in Rails. As in other languages, Backbone models declare a framework for our data.

#### Model Creation:

To create a model, extend Backbone.Model

	var Todo = Backbone.Model.extend({
		defaults: {
			title: "",
			completed: false
		}
	});
	
In keeping with programming best practices, we should set reasonable defaults for our object. In Backbone, this is done through the defaults hash.

#### Model Instantiation:

Model instantiation is straightforward:

	var todo1 = new Todo({
		title: "Learn Backbone",
		completed: false
	});
	
#### Getters & Setters

To retrieve a single attribute, use the Model.get() method

	todo1.get('title');
	> "Learn Backbone"

To retrieve all attributes, use Model.toJSON. Note, the toJSON method returns a JSON object--not a standard JSON string.

	todo1.toJSON();
	> Object {title: "Learn Backbone", completed: false}
	
Model objects also expose an attributes attribute, which usually returns a JSON object, as above:

	todo1.attributes;
	> Object {title: "Learn Backbone", completed: false}
	
To set an attribute, use the Model.set() method:

	todo1.set({title: "Learn about Getters and Setters in Backbone", completed: false});
	
Model.set() can also take a validate option, which runs validations:

	todo1.set({title: "Learn about Getters and Setters in Backbone"}, {validate: true});

By default, Model.set() does not run validations, though Model.save() does, ensuring users do not persist bad data.

#### Model Validations

To add model validations, simply add a validate method, which we've already seen called previously:

	var Todo = Backbone.Model.extend({
		...
		validate = function(attrs, options) {
			if (!attrs.title) {
				return "Needs a title";
			}
		}
	});
	
If validation succeeds, there is no return value, and save() will proceed. Otherwise, save() will not proceed, and the Model object's validationError property is set to the value returned by the validation. A validation error will also trigger the "invalid" event.

	todo1.validationError
	> "Needs a title"
	
#### Adding Event Listeners

While Backbone has a Backbone.Model and a Backbone.View, it does not have a Backbone.Controller; controller actions tend to be handled instead by EventListeners. A convenient place to listen for model changes is in the initialize function:

	var Todo = Backbone.Model.extend({ 
		initialize: function() {
			this.on('change', function() {
				console.log("Values have changed");
			});
		}
	});
	
To add attribute-specific listeners is trivial:

	this.on('change:title', function() {
		console.log("Title has been updated");
	}
	
Since validations trigger the invalid event, we can hook into that event here as well:

	this.on('invalid', function() {
		console.log(this.validationError);
	}
	
#### Unset

The unset method is self-explanatory, but with a well-defined model, with an invalid function as written above, visualizing validation errors is simple:

	todo1.unset('title', {validate: true});
	>  "Needs a title"


