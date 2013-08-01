# Backbone/Marionette on Rails

_This article assumes a good knowledge of Rails, Coffeescript, Javascript, and jQuery, but little-to-no knowledge of Backbone and Marionette. We'll be moving quickly over concepts in other libraries (and often deferring to your own judgement about their setup), so if you need help, please refer to the following resources:_

Rails:

* [Rails Guides (free) ] (http://guides.rubyonrails.org)
* [Obie Fernandez' _The Rails 3 Way_ ](http://www.amazon.com/Rails-Way-Addison-Wesley-Professional-Ruby/dp/0321601661/ref=sr_1_1?s=books&ie=UTF8&qid=1375036006&sr=1-1&keywords=rails+4+way)

CoffeeScript:

* [Mark Bates' _Programming in CoffeeScript_] (http://www.amazon.com/Programming-CoffeeScript-Developers-Library-Bates/dp/032182010X)
* [Trevor Burnham's _Accelerated CoffeeScript Development_](http://www.amazon.com/CoffeeScript-Accelerated-Development-Trevor-Burnham/dp/1934356786)

Javascript: 

* [Marijn Haverbeke Eloquent Javascript (free) ](http://eloquentjavascript.net)

* [John Resig's _Secrets of the Javascript Ninja_](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X)

jQuery: 

* [jQuery Learning Center (free) ](http://learn.jquery.com)
* [jQuery API Documentation (free) ](http://api.jquery.com)

Alright, without further ado, let's get this party started: 

#### The Piping:

1) Generate a new Rails application; I prefer these settings for Backbone/Marionette

	$ rails new appName -T --database=postgresql
	// Skip test unit and establish a postgres database
	
2) Setup some default gems and environment. The only gem I recommend in our case is `thin`, because we'll be deploying to Heroku, and it's best to use the same server in our development and production environments.

Notice we won't be using any special gems to integrate Backbone and Marionette with Rails. We'll be doing it by hand to get maximum control over our environment, and also so we learn the right way to do it.

3) So let's do it by hand! Add Backbone and its dependencies:

Your Javascript file structure should look like:

	> javascripts
		> lib
			- backbone.js
			- marionette.js
			- underscore.js
			
For each of these files, we can copy the development versions from their respective webpages. Thanks to the magic of the Rails asset pipeline, they'll be mashed together, minified, and uglified for us in production.

4) Speaking of the asset pipeline, let's add these files. Remember, load order matters: Underscore is a dependencies of Backbone and Backbone is a dependency of Marionette. So let's load them in that order:

	//= require jquery
	//= require lib/underscore
	//= require lib/backbone
	//= require lib/marionette
	
You'll notice we've removed jquery-ujs; strictly speaking it's not necessary for the app we're building. 

5) We'll be writing a single page application, so the Rails router won't have to do a lot of heavy lifting. We'll establish just a root route, and then hand off further requests to Backbone/Marionette routers:

	root to: "application#index"
	
6) Add a generic index action to the Application Controller.

	def index  		# This is sufficient for our purposes
	end
	
7) In the view, setup the regions you'll want to hook into via Backbone and Marionette. For instance, if we want a header, footer, and main-region, we would define those:

_views/application/index.html.erb_

	<div id="header-region"></div>
	<div id="main-region"></div>
	<div id="footer-region"></div>
	
Here we can also add the Javascript that will start our DemoApp. Next, we'll be showing you how to write a Marionette.Application object that will hang off the window, so in this script, we'll just tap into that object's start method:

_views/application/index.html.erb_

	<%= javascript_tag do %>
		$(function() {
		  DemoApp.start();
		});
	<% end %>
	
#### Getting to the Backbone:
	
Now it's finally time to write some Backbone! Let's get started:
	
8) Add a Backbone folder in you Javascripts folder. This is where we'll namespace the rest of our Backbone application. So under this folder, the first thing we'll want to establish is an `app.js.coffee` file

	> javascripts
		> backbone
			- app.js.coffee

9) And of course, load this file in the asset pipeline _after_ we've loaded our dependencies:

		//= require backbone/app
		
#### The app.js file:
		
10) So what are the responsibilities of this high-level `app.js.coffee` file? 

* Establish an IIFE -- an immediately-invoked function expression -- to perform setup of the Marionette.Application object and hand it back to the DemoApp variable. IIFEs ensure that all setup gets handled as soon as the function expression is evaluated and then it's never run again. Precisely the right type of function to perform setup. 

* Add region objects to the DemoApp that correspond to the jQuery selectors for the regions we established in `application/index.html.erb`. This way we can update these regions dynamically throughout the rest of the application.
* Tap into the `initialize:after` event to start Backbone.history. Backbone.history uses (what else?) the HTML History API to track hash change events and build up a savable _state_ of our application. This API is particularly useful in SPAs, where a user might build a shopping cart, and wish to share the cart with a family member; tacking these hash change events onto the URI, and handling for them in our application's API will allow our user to work with our application in predictable, and savable ways. Click here for a sweet introduction to the [History API](http://diveintohtml5.info/history.html). 

Alright, that was a lot of talking, and not a lot of code. So what does this look like? 

	@DemoApp = do (Backbone, Marionette) ->
	
		App = new Marionette.Application
		
		App.addRegions
			headerRegion: "#header-region"
			mainRegion: "#main-region"
			footerRegion: "#footer-region"
			
		App.on "initialize:after", ->
			if Backbone.history
				Backbone.history.start()
				
		App  // Use CoffeeScript's Ruby-like implicit return to hand the App object we've setup back to the this.DemoApp object

Okay, now we have a Marionette.Application object. That object has several region objects that are hooked up to our HTML. The History API is listening for hash change events, and we can reference our DemoApp object anywhere. Now what the heck do we do with it?

#### App in Your App So You Can App While You App

11) The 2013 "modern" architecture for building apps is to use modules. Lots of modules. Wherein each module is an independently function app that other apps can interact with via its API. The app is singly-responsible and hugely reusable, which should sound pretty familiar to our object-oriented friends. 

So that's the way we'll build each piece of our application. The header, footer, and body will be independently-function apps namespaced under our primary application object. So what does the architecture look like for each app-within-an-app?

	> javascripts
		> backbone
			> apps
				> footer
					- footer_app.js.coffee
					> show
						- show_controller.js.coffee
						- show_view.js.coffee
					> templates
						- show.js.eco
					
#### Organize Yourself

12) The credo of editors is consistency, consistency, consistency. There are a hundred right ways to write the same thing (and a million potential organizations of the same material), but the best way to ease the reading of _your_ material is to pick one of each and to stick to it. 

The organization I offered above follows two rules:

1) Top-level/action/template separation. The `footer_app.js.coffee` lives in the top level; each action has a folder with a corresponding controller and view; and the templates folder contains a template for each action.
2) RESTful actions. Though our "footer_app" isn't something we'd traditionally conceive of as a resource (I can't conceive of it having more than a show action), when we begin working with more robust resources, we'll see that this organization will help us to stay mentally organized, as in Rails. 

#### What's in an App?

13) What are the responsibilities of the top-level footer_app.js? 

* Define the module
* Define an API to interact with the world at large
* Define lifecycle events for the app to respond to

		@Demo.module "FooterApp", (FooterApp, App, Backbone, Marionette, $, _) ->
			API =
				showFooter: ->
					FooterApp.Show.Controller.showFooter()
					
			FooterApp.on "start", ->
				API.showFooter()
				
Here we've achieved a simple version of all three responsibilities--we've set up our FooterApp to respond to `start` events by calling the showFooter() method on the API, which calls the show controller's showFooter() method. 

Note the namespacing of the Controller module--underneath Show and not the other way around (as in Rails). This arrangement is just one possible organization among many, but one that allows us to organize each app's folder structure into RESTful actions and views. Naturally, we'll assume the Show.Controller will only implement a show controller action, and an Index.Controller will only implement an index controller action. 

So when does the "start" action get called on a module? By default, all modules namespaced under our main `app.js.coffee` will be called when we call `Demo.start()`, but this is not very explicit or expressive for newcomers to our codebase. One way to call out the default behavior is to first silence it in our modules:

	@startWithParent = false
	
In the context of the module, `this` refers to the module itself, so `this.startWithParent = false` tells our module not to start when the `Demo.start()` action is called. Instead, we can tap into the initialization process of `app.js.coffee` to make this call directly:

	App.addInitializer ->
		App.module("FooterApp").start()
		
Now, if you were following the thread of the application initialization, it would be much more obvious what was going on.

14) Implement the `Show.Controller.showFooter()` action:

	Demo.module "FooterApp.Show", (Show, App, Backbone, Marionette, $, _) ->
		Show.Controller = 
			
			showFooter: ->
				footerView = @getFooterView()
				App.footerRegion.show footerView
				
			getFooterView: ->
				new Show.Footer
				
Here we define a module namespaced under FooterApp called "Show", and we're explicitly defining a module under that--Controller. The Show.Controller module features the showFooter method implementation that we call in the FooterApp's showFooter method, and it calls the getFooterView method to instantiate a new Show.Footer view. It then loads the view into the app's footerRegion object that we previously mapped to the "footer-region" div in our `application.html.erb` file. 

15) Next we need to declare that Show.Footer view module:

`show_view.js.coffee`
	
	Demo.module "FooterApp.Show", (Show, App, Backbone, Marionette, $, _) ->
		
		class Show.Footer extends Marionette.ItemView
			template: JST["path/to/template"]
			
16) Since we've previously declared that we'll use Embedded CoffeeScript (eco) for templating, now we need to include it in our Gemfile:

	group :assets do
		gem 'eco'
	end
	
17) In order to set a default template path for our templates, we can create a `config` file for Marionette:

	> backbone
		> config
			> marionette
				- renderer.js.coffee
				
Renderer is a class inside of Marionette that we'll be overriding, so we'll namespace a `renderer.js.coffee` file underneath `config/marionette`. Don't forget to tell the Rails asset pipeline to load the `config` folder before loading the rest of the application. Here's our renderer code, which taps directly into Marionette:

	Backbone.Marionette.Renderer.render = (template, data) ->
		path = JST["backbone/apps/" + template]
		unless path
			throw "Template #{template} not found"
		path(data)
		
Here we override the Renderer function's render method, which by default takes a template object and a data object (see the Marionette documentation for more details). Then we're implementing the default behavior, while overriding the default path to be `backbone/apps/`, so that we don't have to type that in for each template. Neat, no?

18) Back in our Show.Footer view:

	template: "footer/show/templates/show_footer"
	
Note of course that the path provided is relative to `backbone/apps/`, and that the template path is being wrapped in a JST[] call for us. 

By default, the `show_footer` template will be wrapped in a `<div>` element (which we can see if we inspect the page that loads). If we want to override this implementation, in our view file, we can simply tap into the `tagName` property:

	tagName: "li"
	
By adding in our template, we've added a new property to the footerRegion object--currentView. CurrentView in this case holds a reference to our Footer view, but we could pass in any number of views to this object to update the content within. When we call `App.footerRegion.show`, Marionette checks which view is currently being held, and updates the view as appropriate, removing the old view if necessary. There's no need for us to explicitly remove any old views. 

19) In our template, we can now add the view we want displayed in the footer, and it will be shown when the app starts. 

At this point, we've seen default arrangements for adding many smaller "apps" to make our bigger app, and we can work off these arrangements to create our entire app. To save the repetition, I won't show the process of adding our headerRegion and mainRegion, because you're now prepared to handle that on your own.

Congratulations! You're now ready to build your first Backbone app. Time to get cracking!


		


	
