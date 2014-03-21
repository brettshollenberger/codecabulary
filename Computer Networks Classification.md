# Classification of Computer Networks

Computer networks are classified both by the types of transmission technology they use and their size.

## Classification by Transmission Technology

In widespread use, there are two types of general transmission technologies, point-to-point links and broadcast links.

Point-to-point links connect individual pairs of machines. To send a message from a source to a destination across a network made up of point-to-point links, short messages called packets are often relayed across a series of intermediate point-to-point links. Because these messages are often relayed across links, multiple routes of differing lengths are often possible when routing a message from a source to a destination, so finding efficient paths is an important piece of point-to-point networking. 

Point-to-point transmission with one receiver and one sender is sometimes called _unicasting_.

In contrast, on broadcast networks, the communication channel is shared by all machines on the network. Packets sent by any machine on the network are received by all other machines on the network. An address field within each packet specifies the intended recipient. When machines receive the packet, they process the address field, and if the packet is intended for the machine, the machine processes it; otherwise, the packet is ignored. 

Wireless networks are a common example of broadcast link networks, with communication shared over a coverage region that depends on the wireless channel and the transmitting machine. An analogy of a broadcasted message is someone standing in an office and shouting "Watson, come here!" While a range of people in the office may hear the message, only Watson should respond. The others will just ignore the message.

Broadcast systems also allow the possibility of addressing a packet to all destinations using a special code in the address field. When a packet with this code is transmitted, it is received and processed by every machine on the network. This method of operation is called _broadcasting_. Some broadcast systems also support transmission to a subset of the machines, which is called _multicasting_. 

## Classification by Scale

Networks can also be classified by their rough physical size.

| Interprocessor Distance | Processors located in the same     | Example | 


| Interprocessor Distance | Processors located in the same | Example                     |
| -------------------------------  |:-------------------------------------------|:-----------------------------|
| 1 m                     | Square meter                   | Personal area network       |
| 10 m                    | Room                           | Local area network          |
| 100 m                   | Building                       | Local area network          |
| 1 km                    | Campus                         | Local area network          |
| 10 km                   | City                           | Metropolitan area network   |
| 100 km                  | Country                        | Wide area network           |
| 1000 km                 | Continent                      | Wide area network           |
| 10,000 km               | Planet                         | The Internet                |
| Massive                 | Interplanetary                 | The Interplanetary Internet |

### Personal Area Networks

PANs (Personal Area Networks) allow devices to communicate over the range of a person. A common example is a wireless network that connects a computer with its peripherals. Bluetooth is a short-range wireless network that connects components without wires. In their simplest form, Bluetooth networks use the master-slave paradigm, in which the system unit (the PC) is usually the master, talking to slaves (e.g. a mouse or keyboard). The master tells the slaves what addresses to use, when they can broadcast, how long they can transmit, what frequencies to use, and so on. PANs can also be built with other technologies that transmit over short ranges, such as RFID on smart cards and library books.

### Local Area Networks

LANs (Local Area Networks) are privately-owned networks that operate within and nearby a single building like a home or office. LANs are widely used to connect personal computers and consumer electronics and let them share resources like printers and exchange information. When LANs are used by companies they are called _enterprise networks_.

Wireless LANs are very popular, especially in homes, older office buildings, cafeterias, and other areas where it is too much trouble to install cables. In these systems, every computer has a radio modem and an antenna that it uses to communicate with other computers. In most cases, each computer talks to a device called an AP (Access Point), wireless router, or base station. The AP relays packets between the wireless computers, and also between each computer and the Internet. 

There is a standard for wireless LANs called IEEE 802.11, popularly known as WiFi, which has become widespread.

The topology of many wired LANs is built from point-to-point links. IEEE 802.3, popularly called Ethernet, is by far the most common type of wired LAN. Each computer speaks the Ethernet protocol, and connects to a box called a switch with a point-to-point link. Each switch has multiple ports, each of which can connect to one computer. The job of the switch is to relay packets between computers that are attached to it, using the address in each packet to determine which computer to send it to. 

### Metropolitan Area Networks

MANs (Metropolitan Area Networks) cover a city. The best-known examples of MANs are the cable television networks available in many cities. These systems grew from earlier community antenna systems used in areas with poor over-the-air television reception. 

When the Internet began attracting a mass audience, the cable TV network operators began to realize that with some changes to the system, they could provide two-way Internet service to unused parts of the spectrum. At that point, the cable TV system began to morph from simply a way to distribute television to a metropolitan area network. 

### Wide Area Networks

WANs (Wide Area Networks) span large geographical areas, like countries and continents. A WAN could be a network connecting offices in Perth, Melbourne, and Brisbane. Each of these offices could contain computers intended for running applications; following traditional usage, we could call these machines hosts. The rest of the network connecting these hosts is then called the communication subnet, or just _subnet_ for short. The job of the subnet is to carry messages from host to host, just as the telephone system carries words from speaker to listener.

In most WANs, the subnet consists of two distinct components: transmission lines and switching elements. Transmission lines move bits between machines. They can be made of copper wire, optical fiber, or even radio links. Most companies don't have transmission lines lying about, so they lease the lines from a telecommunications company. Switching elements, or just switches, are specialized computers that connect two or more transmission lines. When data arrive on an incoming line, the switching element must choose an outgoing line on which to forward them. The name router is most commonly used for switches. 

Here the term subnet means a collection of routes and communication lines, though the term has an alternate meaning discussed later. 

Routers can connect different kinds of networking technology, like switched Ethernet and SONET links. This means that many WANs will in fact be internetworks, or composite networks made up of more than one network. 

Subnets can connect individual machines, as in the case of LANs, or they can connect entire LANs. This is how larger networks are built from smaller ones. As far as the subnet is concerned, it does the same job. 

Some WANs may, instead of leasing dedicated transmission lines, decide to use the Internet. This allows connections to be made between offices as virtual links, using the underlying capacity of the Internet. This arrangement is called a VPN (virtual private network). Compared to the dedicated arrangement, a VPN has the usual advantage of virtualization, in that it provides flexible reuse of a resource (Internet connectivity). VPNs also have the usual disadvantage of virtualization, a lack of control over the underlying resources. With a dedicated line, the capacity is clear. With a VPN, your mileage may vary with your internet service. 

Another variation is that the subnet may be run by a different company, known as a network service provider. The subnet operator will also connect to other networks that are a part of the Internet, making the subnet operator an ISP (Internet Service Provider). 

### Internetworks

A collection of interconnected networks is called an internetwork or internet. These terms refer to interconnected networks in a generic sense, in contrast to the worldwide Internet (one specific internet). 

Subnets, networks, and internetworks are often confused. The term "subnet" makes the most sense in the context of a wide area network, where it refers to the collection of routers and communication lines. 

A network is formed by the combination of a subnet and its hosts. 

An internetwork connects distinct networks. Connecting a LAN and a WAN or two Lans is the usual way to form an internetwork. 

To go deeper, we need to talk about how two different networks can be connected. The general name for a machine that makes a connection between two or more networks and provides the necessary translation, both in terms of hardware and software, is a _gateway_. Gateways are distinguished by the layer at which they operate in the protocol hierarchy. Imagine that higher layers are more tied to applications, such as the Web, and lower layers are more tied to transmission links, such as ethernet. 

Since the benefit of forming an internet is to connect computers across networks, we don't want to use too low-level a gateway, or we will be unable to make connections between different kinds of networks. We do not want to use too high-level a gateway either ,or the connection will only work for particular applications. The level in the middle that is "just right" is often called the network layer, and a router is a gateway that switches packets at the network layer. We can now spot an internet by finding a network that has routers. 
