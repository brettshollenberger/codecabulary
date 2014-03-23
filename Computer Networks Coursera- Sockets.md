# Sockets

Sockets are a simple abstraction allowing applications to talk to one another across networks.

Sockets let apps attach to the network at different ports, which provides a form of addressing that allows us to multiplex applications on a single host.

The socket API consists of a few steps allowing clients to speak with servers:

1) Client sends a `connect()` request

2) Server echoes `connect()`, and calls `bind()`, which establishes an address for the connection endpoint.

3) Server calls `listen()`, declaring that it is prepared to accept connections from the other side.

4) Server calls `accept()`, and then waits for a connection from the other side.

5) Client calls `connect()`, establishing the connection.

6) Server calls `receive()`, and waits for messages to be received, like someone sitting by the telephone waiting for it to ring.

7) Client calls `send()`, with a request.

8) Client calls `receive()`, waiting for a reply.

9) Server receives the request, performs some processing, and then `send()`s some response.

10) Client receives the response and acts appropriately.

11)  Steps 6-10 repeat during the course of the program.

12) Client and server call `close()` to close the connection. \

