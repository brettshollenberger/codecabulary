# Backbone Views

Backbone views are different from Rails views in that they don't contain the HTML markup for your application. Instead, they contain the logic required to present the model data to the user, and take on some of the role of the controller in traditional MVC frameworks. 

The work of the traditional view layer is handled instead by Javascript templating engines, like the Underscore microtemplates that are a dependency of Backbone, Mustache, jQuerytmpl, and others. 

A view's `render` method can be bound to `change()` events, which allow for the asynchronous page updates that make working with a front-end framework like Backbone so wonderful.

#### Creating Backbone Views

Creating Backbone views is much like creating Backbone models; the first step is to extend Backbone.View

	var TodosView = Backbone.View.extend({
		tagName: 'li';
	});
	
#### el, A Backbone Convention

el is a reference to a DOM element, and there are two ways to associate a DOM element with a new view.

1) A new element can be created for the view, and subsequently added to the DOM, as in create actions in traditional RESTful applications. 

2) An reference can be made to an existing element in the DOM, as in show or delete actions in traditional RESTful applications.

#### Creating a new DOM element with el

To create a new DOM element, set any combination of the following properties: tagName, id, or className. Backbone will create a new element for you, and `el` will reference the new element. If nothing else is specified, tagName will default to `div`. 

	var TodosView = Backbone.View.extend({
		tagName: 'ul', // defaults to div if unset
		className: 'container', // supports traditional multiple assignment, e.g. 'container homepage'
		id: 'todos', //optional
	});

This code creates the following new element, but doesn't append it to the DOM.

	<ul id="todos" class="container"></ul>
	
#### Changing a DOM element width el

If the element already exists on the page, you can set `el` to a CSS selector that matches the element:

	el: '#footer'
	
You can also set `el` when creating the view:

	var todosView = new TodosView({el: $('#footer')});
	
