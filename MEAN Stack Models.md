# Hooking up Models in MEAN Stack

1) Using Mongoose, you can create the schema on your MongoDB, including requirements and defaults:

	// mongoSchema.js
	schema.project = new mongoose.Schema({
		id: mongoose.Schema.Types.ObjectID,
		name: {
			type: String,
			required: true
		},
		description: {
			type: String,
			default: "My Great Project"
		}
	});
	
At Faculty Creative, we establish a generic RESTful API, into which we pass each schema object as we turn it into a model object. Check out Mongoose for technical details.

2) Add the API:

	// projects.js
	
	module.exports = function(app, db) {
		app.post('/api/project', function(req, res) {
			var data = req.body;
			
			db.project.create({
				name: data.name,
				description: data.description
			}, function(err, model) {
				... generic error callback ...
			});
		});
	
		... other RESTful services ...
		
