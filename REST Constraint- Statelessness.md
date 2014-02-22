# REST Constraint: Statelessness

One of the constraints of RESTful architecture is statelessness. HTTP is necessarily stateless, which means that each request to the server must contain all necessary information for completing the request.

This improves visibility on the server: the server need not look beyond a single request to understand how to fulfill it.

This improves reliability on the server: it eases the task of recovering from partial failures.

This improves scalability on the server: since the server need not manage application state, or resources across requests, it can quickly free resources and scale to manage many requests.

Statelessness also reflects a trade-off in architecture: since the server cannot manage application state or resources across requests, each client must properly implement application semantics in order to succeed. 