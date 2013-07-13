# Backbone.js MVC Architecture

Like other front-end frameworks, Backbone uses the Front Controller design pattern, which layers the MVC stack behind a single point of entry. The Front Controller handles routing, pairing HTTP requests with controller actions, and passing off the request to the appropriate controller.

Similar to Rails & Django, the controller action then interacts with the model to get the data it requires, loads the appropriate view, injects the model data into the view, and returns the HTTP response to the browser. 