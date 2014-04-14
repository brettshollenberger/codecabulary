# Underscore Templating

Underscore, a prominent Javascript utility library, contains a microtemplating function that provides a good illustration of how templating works in many frameworks.

1) First, we write an HTML template utilizing Underscore's special syntax for interpolation (`<%= interpolated.data %>`), HTML escaping (`<%- html.escaped.data %>'), and arbitrary script evaluation (`<% %>`).

We could either write our template directly in our Javascript, or between `<script>` tags that we can grab the HTML out of using jQuery. The only result we need from this step is raw HTML with the appropriate Underscore tags:

	# app.js
	var rawHTML = "<p><%= person.name %></p>";
	
	OR
	
	# index.html
	<script type="text/template" id="name-template">
		<p><%= person.name %></p>
	</script> 
	
	# app.js
	var rawHTML = $('#name-template').html();
	
2) Now that we have raw HTML, we can utilize Underscore's `_.template` function to transform the template into a templating function. A templating function compiles the HTML we laid out above into a script that's ready to fill in the blanks in our template (but hasn't done so yet). Later, we'll be able to evaluate this function by passing in the data the template requires to become fully-formed:

	# app.js
	var template = _.template(rawHTML);
	
3) Let's go ahead and fill in the template:

	var troy = template({person: {name: "Troy"}});
	>> <p>Troy</p>
	
	var abed = template({person: {name: "Abed"}});
	>> <p>Abed</p>
	
That's pretty easy, right? 

4) We can use this template whenever we want to add a new person to our page, and script its entry into the page with jQuery:

	function addPerson(person) {
		$('#people').append(person);
	}
	
	$('#new-person').on('submit', addPerson);

http://jsfiddle.net/8E3wD/5/