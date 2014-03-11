# Computer Networks: Parts of a Network

Computer networks have applications in business, home, and entertainment, and are primarily used for resource sharing, whether those resources be physical (like a printer shared by an office), or intellectual (like a database of customers shared across a company). Networks are composed of nodes, links, and apps, and more specifically, nodes can be divided into hosts (which support applications) and routers (which pass messages between nodes). 

## Hosts

Hosts are also called end-systems, edge devices, sources, and sinks, and their primary function is supporting applications. Hosts are desktop, laptop, or mobile computers, whose job it is to receive requests to an application via HTTP, and to return responses. 

## Routers

Routers are also called switches and hubs, and their primary job is to relay messages between other nodes. Your home modem is an example of a router, which may pass messages from your computer to other nodes before reaching the node you're looking for (usually a host, which serves up an application).

## Apps

Apps are the users of the network. They're built to provide some service, like telecommunication (Skype) or eCommerce (Amazon), and provide a host as an interface into the application. 

## Types of Links

Most links are full-duplex (bi-directional). Messages can be sent between nodes in either direction, and can be sent at the same time by both sides. Some links are half-duplex; these are still bi-directional links, but they can only send information in one direction at a time. A wireless link is an example of a half-duplex link. Very few nodes are simplex, or unidirectional links, which can only relay messages in a single direction.

### Wireless Links

Wireless links broadcast messages, which are received by all nodes within range. Wireless links to not necessarily create a full message (connecting every node to every other node in a network), because some nodes may not be in range of all other nodes. 

## Networks by Scale

Networks are often described by their scale.

### Vicinity-scale Networks

PANs (Personal Area Networks) connect devices in a small range, like Bluetooth. 

### Building-scale Networks

LANs (Local Area Networks) connect devices on a building scale, using technologies like Ethernet and WiFi.

### City-scale Networks

MANs (Metropolitan Area Networks) connect devices within a city, using technologies like DSL.

### Country-scale Networks

WANs (Wide Area Networks) connect devices on the scale of entire countries, such as a large scale Internet Service Provider.

### Planet-scale Networks

The Internet is the network of networks (connecting many other smaller networks). The Internet has massive value, since the value of a network is often described by the number of nodes squared, since that's the number of possible connections available in the network.