# REST

Representational State Transfer, or REST, is the predominant model for web APIs, and it's a mouthful. But it doesn't have to be. Let's take a look:

Real things don't live in the internet; concepts do. You purchase the idea of a movie ticket on Fandango and you create the concept of a friendship on Facebook. The site itself is an interface that presents to you a _representation_ of the _state_ of that data--you have 500 friendships and 2 movie tickets for _Pacific Rim_. You know you have these things, and the interface shows you that you have these things, but the interface is just a means of interacting with abstract concepts; the Internet doesn't have any movie tickets or friendships living inside it, as hard as I try to believe otherwise. 

When you interact with the interface--by clicking the "Purchase Tickets" button--you request a change to the state of the represented object. You want two of the tickets in the theatre to now belong to you. The interface doesn't change the state, though; it sends a request to the server to ask the database to make the state change. After the change is made (or perhaps as the change is being made, in the case of AJAX), a good interface will present you with the new representation of your data--you have two new movie tickets. 

Since RESTful APIs are really just a means of interacting with a database, they must specify CRUD operations to support. 

| Resource                                                     | GET                                                                                                  | PUT                                                                            | POST                                   | DELETE  |
| -------------------------------------------------------  |:---------------------------------------------------------------------------------------:|:---------------------------------------------------------------------:|:--------------------------------------:| :-------------------------------------------------------|
| Collection URI (example.com/resources)   | List the URIs, and possibly the details of the collection's members | Replace the entire collection with another collection | Create a new entry in the collection  | Delete the entire collection |
|Element URI, such as http://example.com/resources/item17| Retrieve a representation of the addressed member of the collection, expressed in an appropriate Internet media type. | Replace the addressed member of the collection, or if it doesn't exist, create it. | Not generally used. Treat the addressed member as a collection in its own right and create a new entry in it.| Delete the addressed member of the collection.
