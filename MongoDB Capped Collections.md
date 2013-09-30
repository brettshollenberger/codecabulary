# Capped Collections in MongoDB

Mongo implements capped collections for high-performance logging, ensuring fast reads and writes at the expense of ephemeral data. In situations like logging, persisted data may not be desirable, and capped collections prevent you from having to prune the collection manually to include only recent data; once a capped collection reaches its maximum size, new inserts will overwrite the least-recently-inserted documents for you.

By default, capped collections don't feature an index on the `_id` field be design. No indexing means faster write times, and capped collections are made with performance in mind. No indexing also means sequential processing in either natural order (order of insertion) or reverse-natural order:

	db.log.actions.find().sort({$natural: -1});
	
When creating capped collections, you have to specify that the collection is capped, as well as its size:

	db.createCollection(name, {capped: true, size: 1024});