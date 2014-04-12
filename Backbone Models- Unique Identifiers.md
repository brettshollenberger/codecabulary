# Backbone Models: Unique Identifiers

Backbone features 3 fields for unique attributes (`id`, `cid`, and `idattribute`), which might seem totally perplexing at first. Why don't we just need _one_ unique attribute? We'll examine the problems that unique ids deal with in general, and the problems client-side modeling deals with to properly answer this question. Client-side modeling often deals with 4 problems, among others:

1) Interacting with a RESTful API--retrieving, creating, updating, and destroying data.

2) Caching data on the client, and only requesting data when a cached version is invalid because it is too old (as dictated by the server), or when the client doesn't have the information cached. There are a number of ways that web APIs indicate that a resource can be cached, and for how long, including the HTTP headers `Cache-Control` and `Expires`, and it's up to our client library to parse these.

3) Representing the relationships between relational data.

4) Consuming and responding to hypermedia.

Each of these four client-side modeling problems requires the use of unique identifiers--guaranteed-unique strings or integers that ensure we're always talking about the correct model instance, whether we're figuring out which instance to send to the backend, which to cache, or which instance belongs to another.

Specifying unique identifiers can cause us to encounter a number of issues on the client (and a few that crop up on the server as well):

1) Uniquely identifying un-persisted data. Databases traditionally handle unique key creation, and until we save a model, we won't have a unique key. 

Some frameworks, like Ruby on Rails, simply do not use a unique key until a model instance has been persisted. This tends to work well on backend clients due to the stateless nature of HTTP; servers do not keep track of application state between requests, so the client must specify all the necessary details of each request. Even if it's information the client was just talking with the server about, the server will have forgotten the conversation, and the client will have to recount all the necessary details for the server to perform its operations. Since the server can safely forget about models that have not been persisted, a framework like Ruby on Rails can safely use the nil id field; if we're building nice, RESTful APIs, we'll either never send an un-persisted representation from the server (we'll send a template, maybe), or we'll just hand that representation off to the client, and the client will either make a request to create a new resource, or it won't. The server doesn't care which, but it knows what to do with a new POST request.

The client, however, must manage application state. What if we were creating a client for a Battleship game, and we wanted our user to be able to move around 5 new ships before deciding they were satisfied with the position for each? We'd have to manage the location of each ship, and the unique keys for each so that we could trigger move events on the different game pieces. 

Backbone solves this problem with the `cid` field. It's a little convenience that Backbone affords us while we have un-persisted data. `cid` stands, appropriately, for `client id`, and the field is totally unnecessary once the model instance has a true `id` field.

2) Uniquely identifying persisted data. Backbone (and clients like Ruby on Rails) standardize the key that represents unique data--they always use the `id` field. This convention over configuration helps us as users of the framework; the abstraction of a unique key is simple, and predictable, even though our database schema may not be. Which leads us to our next problem:

3) Managing legacy database schemas. Many older books on SQL and database design recommend--as a best practice--that the name of the field that uniquely identifies a row in a table should be named `table_name_id` (e.g. a record in the Books table would feature a field called `book_id`, and a record in the Authors table would feature a field called `author_id`). This syntax certainly made complex JOINs read more clearly, but in abstractions above SQL, like Object-Relational Mappers, this abstraction becomes noisy and unpredictable. 

Still, we may have to work with tables designed this way (they may be multi-million dollar tables, after all). Ideally, we'd like to still present our users with the `id` key abstraction, so they don't have to worry about the details of mapping to the correct unique key for a given model. That's where the `idAttribute` key comes from: it's the field on the database that Backbone will read as the unique key. As long as we specify the field in the model, we can continue using the nice `id` abstraction. 