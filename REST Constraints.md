# REST Constraints

Representational State Transfer (REST) is an architectural approach to building distributed hypermedia systems (e.g. those that work on the internet). You've likely heard the buzzword around--the basic concept is that a server maintains canonical versions of resources (data and metadata for things like a library's stock or a company's mailing list), and servers and clients can pass back and forth representations of these resources (JSON, XML, and many others). A client can also post requests to create new resources, update resources, and destroy resources; those familiar with CRUD will recognize REST as akin to "CRUD for HTTP."

The guiding principle of REST as opposed to SOAP and other architectural styles is that the constraints provided by HTTP are sufficient to act as constraints for CRUD interactions. HTTP and REST were both developed by Roy T. Fielding, so this may be unsurprising. REST is therefore described as a _derived_ architectural style--its constraints evolved out of the constraints of HTTP.

## Client-server Separation Constraint

One of the tenants of HTTP and thereby REST is the separation of clients and servers. A server's job in RESTful interactions is to provide services for interacting with resources; a client's job is therefore to make requests upon those services, and to present the results of those requests in human-readable ways. The benefits of client-server separation are that many clients can be created to interact with the same services, and a server's job is simplified, and therefore better able to scale.

## Statelessness Constraint

HTTP is stateless; from one request to another, a server does not maintain session information for a client. Clients must send all necessary data to fulfill each request, and a server must only respond by fulfilling the request or rejecting it. On the server, this improves visibility, reliability, and scalability. Visibility improves because a server has all necessary details to fulfill a request or it doesn't; it knows all the information at any given time and is able to act upon that information appropriately Reliability improves because recovering from partial failures is simpler with stateless interaction. Scalability improves because the server is not required to manage session information (that's the client's job), so the server can quickly free resources and move on to additional requests.

One of the trade-offs of statelessness is that each client that needs to interact with the server must properly implement application semantics in order to fulfill requests properly.

## Caching Constraint

Since HTTP is stateless, in order to improve efficiency, each response must either be implicitly or explicitly marked by the server as cacheable or non-cacheable. Cacheable resources allow the client to reuse previously unearthed resources for later, equivalent requests. 

Caching can eliminate certain interactions, and improve the overall user-perceived performance of the client. It can also decrease reliability if certain cached resources significantly differ from data that would have been retrieved had a request been made directly of the server.
 ## Uniform Interface

The primary feature that distinguishes REST from other network-based architectural styles is an emphasis on a uniform interface. A uniform interface is achieved through four additional constraints: 1) Identification of resources, 2) Manipulation of resources through representations, 3) Self-descriptive messages, and 4) Hypermedia as the engine of application state. A uniform interface optimizes for large-grained hypermedia data transfer, and improves reliability and predictability, at the expense of application-specific needs.

### Identification of Resources Constraint

## REST Architectural Elements

REST ignores the details of component implementation and protocol syntax. It focuses instead on the roles of components, the constraints on their interaction with other components, and their interpretation of data elements.

### Data Elements

The nature and state of an application's data elements is central to REST.

When a link is selected, information needs to be moved from the location where it is stored to the location where it will be used.

REST allows the client and server to possess a shared understanding of data types with metadata, but limits the scope of what is revealed to the client to a standardized interface.

REST components communicate by transferring representations of resources in a format from an evolving set of standard data types, selected based on the desires of the recipient (the client). Whether the retrieved representation is the same format as the canonical resource, or is derived from the source, remains abstracted away behind the interface.

#### REST Data Elements

| Data Element                    | Modern Web Example                                                      |
| ---------------------------------  |:-------------------------------------------------------------------------- |
| resource                            | intended conceptual target of a hypermedia reference    |
| resource identifier             | URL, URN                                                                         |
| representation                  | HTML document, JPG image                                            |
| representation metadata  | media type, last-modified-since                                         |
| resource metadata           | source link, alternates                                                        |
| control data                      | if-modified-since, cache control                                          |

 