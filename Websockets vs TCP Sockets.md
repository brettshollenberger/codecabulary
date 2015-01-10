# Websockets vs TCP Sockets

A TCP socket a Unix file descriptor that allow programs to speak to one another. One of types of TCP sockets, SOCK_STREAM, allows for full-duplex (bidirectional) communication between programs on different machines over the internet. 

That sounds pretty good. So we've got a browser and we've got a server somewhere--we can just connect them with SOCK_STREAM and get on with creating our real-time chat app, right? Almost.

Some programs work great in very similar ways. SSH, Telnet--what could possibly go wrong? Well these well-known applications have well-known ports reserved for them on most machines (22 and 23 respectively). When you SSH onto a remote host, you can pretty much guarantee that the host `sshd` accepted traffic on port 22.

That's all well and good, but _your application does not have a well-known port_. But there are well-known ports that browsers generally use to connect with servers--the HTTP and HTTPS ports (80 and 443).

Good, so let's use one of those. The only small problem is: those ports use the HTTP protocol, so we can't even say, "Hello Mr. Server, I'd like to initiate a TCP socket connection with you," unless we dress up our message in all the right headers.

That's where Websockets come in. They help us initiate a connection over HTTP, request a TCP socket, and then communicate as we would using sockets normally.