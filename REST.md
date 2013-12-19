# REST

Representational State Transfer (REST) is a big phrase that describes a very simple set of concepts:

1) Getting and setting values on persisted data (usually in a database)
2) Within distributed programs (via HTTP)
3) Across various, unknown protocols and data types (whatever the client, server, and database decide to use)
4) In predictable ways (where HTTP verbs map to CRUD verbs, and presentation resources are separated from data resources)

### Addressability

Every resource can be referenced from a canonical URL (and maybe others as well). If users are likely to refer to an entity in isolation (post, author, comment), then that item is a resource and should be uniquely addressable.

### Hypermedia as the Engine of Application State

Various web media (hypertext, images, video, etc) are used to send the server requests to change the state of the resource. The server responds with a new resource, and if presentation code lives on the server, a new application state to update the page with. The principle is also known as "connectedness," because hypermedia entities reveal a menu of actions the user may wish to perform, and these options connect the application to other application states, and future application states, as they update resources, create new resources, and delete old ones.

### Challenges & Problems

Web APIs lag behind the protocols and design patterns for implementing them, because these protocols are not often followed:

* Clients cannot reliably understand APIs
* Non-human clients need to write individual parsers for specific APIs instead of tapping into reusable libraries that know how to parse APIs--we don't have a social contract yet for APIs
* URLs are not always self-descriptive about the resources that can be found behind them

These are the HTTP-specific challenges that are readily solvable by using:

* The right HTTP verb for the job
* Collection+JSON
* Self-descriptive URLs

There's also the "semantic challenge," which is not readily solvable by following a set of standards. The semantic challenge describes the domain-specific language of the problem domain, which is not predictable, though could be parseable through a protocol-constrained set of hypermedia entities.

### Resources and Representations

REST is not a protocol, format or framework. It's a set of constraints:

* Statelessness - The server does not care what state the client is in between requests
* Hypermedia as the Engine of Application State - We must use the languages of the web to build web applications, so we must necessarily use hypermedia to send requests to the server to alter the state of resources.

We call these the Fielding constraints. They describe the decisions behind the design of the web. 

Architecture of the World Wide Web describes the technologies that came out of those decisions, namely URL, HTTP, and HTML. Underlying these technologies are two key concepts (resources and representations). Resources are the persisted, canonical versions of entities, which are themselves representations of real-world entities. A "resource" could be a database record for an individual car on a sales lot. It could be a picture of that car, or it could be the HTML page that displays the "Buy it now" button. Resources can be anything that people want to talk about in and of itself; the only constraint is: a resource must have a unique URL.

At that unique URL, the server returns a representation of that resource. In the case of the photo or the HTML, the representation looks the same as the canonical resource. In the case of the database record, it may be a JSON representation, an XML representation, the SQL insert query used to create the record--or something else. It's up to the server and client to agree on a representation they both can understand and deliver to the user, but the client is inherently limited to this collection of web technologies. 

Representations are transferred back and forth between the client and server. They may send significantly different representations of resources. For instance, in the collection+JSON format, the client will send a filled in template to create a new record, and the server may return all records of that type in a longer JSON collection, or it may return the individual, created record representation. Even in the same format, these two representations may look different, but refer to the same resource. By sending these representations, however, the client and server are truly engaging in representational state transfer. The client sends representations of what it would like a resource to look like, and the server sends back what the resource actually looks like. It may have chosen to accept the representation created by the client, to update that representation using its own logic, or to reject a request to change. That's REST--the manipulation and creation of CRUD-style records using representations of those resources. 