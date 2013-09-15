# The MEAN.io Setup

#### The Model Layer

In the MEAN.io setup, the model layer lives in `app/models/modelName.js`. Each model uses Mongoose to define its schema, set validations, and create static properties (class methods) before using the built-in Mongoose.model function to return a valid Mongoose model object.

The Model object is compiled using a Mongoose schema object. The Model is a fancy constructor used to instantiate documents in MongoDB, a document-oriented database. 

	// Require all necessary files
	var mongoose = require('mongoose'),
		config = require('../../config/config'),
		Schema = mongoose.Schema;
		
	// Define our schema
	var ArticleSchema = new Schema({
		created: {
			type: Data,
			default: Date.now
		},
		title: {
			type: String,
			default: '',
			trim: true
		} ...
	});
	
	// Perform validations
	ArticleSchema.path('title').validate(function(title) {
		return title.length;
	}, 'title cannot be blank');
	
	// Define static methods aka class methods
	ArticleSchema.statics = {
		load: function(id, cb) {
			this.findOne({
				_id: id
			}).populate('user').exec(cb);
		}
	};
	

	
	