# Best Practices for Backbone Templating

1) Don't set the `el` on the view. Allow the view to instantiate its own `el`:

	// Wrong
	Backbone.View.extend({
		el: '#container'
	});
	
	// Right:
	Backbone.View.extend({});
	
When you set the `el` explicitly, each new rendering will be given the same `el`, and will overwrite the previous instance:

	// View
	render: function() {
		var html        = "<p><%= book.title %> by <%= book.author %></p>";
		var template = _.template(html);
		var rendered = template({book: this.model.attributes});
		
		$(this.el).html(rendered);
		
		return this;
	}

	// Controller, Route, etc.
	_.each(models, function(model) {
		var view = new UserView({model: model}).render();
	});
	
	Results in just one view being rendered.
	
Instead:

	// View
	Same as above
	
	// Controller, Route, etc.
	_.each(models, function(model) {
		var view = new UserModel({model: model});
		$('#container').append(view.render().el);
	});