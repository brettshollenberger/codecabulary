# Mongoose Callbacks

Mongoose provides Javascript object modeling for Node.js/MongoDB applications utilizing Mongo's query interface. Mongoose extends this query interface to include multiple methods for querying, but the one I see most used in APIs is the immediate execution syntax, which requires a callback. 

Javascript's event-driven, asynchronous callbacks are an infamous solution to the issue of single-threading--allowing time-sucking scripts like database queries to be non-blocking by providing a function for the script to execute when it's finished taking its sweet time. 

Mongoose callbacks work the same way--the query executes immediately and we provide a callback to execute when the query is done (database queries are, after all, one of the primary reasons callbacks were invented). The structure of a Mongoose callback is always the same, too, so let's look at that:

	callback(error, results) { };
	
Mongoose will automatically populate the error and results parameters based on the results of the query. One of the two will be `null`--the `error` parameter if the query is successful, and the `results` parameter otherwise. The non-null parameter will be a Mongoose document--an instance of a Mongoose model class--or a collection of Mongoose documents, supposing the query is for all documents matching certain parameters. As a Mongoose document, this object will feature special functionality added by Mongoose for us to tap into.