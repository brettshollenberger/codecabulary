#Konacha

Jamine & Konacha for unit testing of Javascript

You've have to write object-oriented Javascript to unit test it effectively

ejs & konacha gems for testing

spec_helper.js
//= require applications
//= require_tree .vendor

form_spec.js

	beforeEach(function() {
		$('body').html(JST['templates/comments/form']()
		
Use JavaScript templates as a way to save layouts to mess with during testing; fixture 

Look at HandlebarsJS for Javascript Templating

app/views/comments

	KonachaTest = {};
	KonachaTest.Views = {};
	KonachaTest.Views.Comments = {};
	KonachaTest.Views.Comments.Form = function(options) {
		this.el = options.el;
		this.form = 
		
Open up a Konacha server

	konacha.serve

Sinan.js -> open up server
