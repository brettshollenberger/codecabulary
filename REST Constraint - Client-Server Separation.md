# REST Constraint: Client-Server Separation

Client-server separation is one of the foundational constraints in RESTful networking architecture. 

The server provides services upon request. Usually the server is a non-terminating process that provides these services to many clients. 

The client is a triggering mechanism that allows a user to request services. The client sends the request to the server, and the server replies with either a refusal to complete the request or the results of the request.

The primary reason for client-server separation is separation of concerns. This separation allows the server to remain focused and scalable, and usually moves the entire user interface to the client. Separation also allows the client and server to evolve independently, provided the server's interface does not change. 