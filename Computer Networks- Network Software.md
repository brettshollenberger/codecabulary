# Network Software

The first computer networks were designed with hardware as the main concern, and software as an afterthought. This strategy no longer works. Network software is now highly structured.

### Protocol Hierarchies

To reduce their design complexity, most networks are organized as a stack of layers or levels, each built upon the one below it. The number of layers, the name of each layer, the contents of each layer, and the function of each layer differ from network to network. The purpose of each layer is to offer certain services to the higher layers, while shielding those layers from the details of how the offered services are actually implemented. 

When layer _n_ on one machine carries on a conversation with layer _n_ on another machine, the rules and conventions used in this conversation are collectively known as the layer _n_ protocol. Basically, a protocol is an agreement between communicating parties on how communication is to proceed. The entities comprising corresponding layers on different machines are called peers.

In reality, data are not directly transferred from layer _n_ on one machine to layer _n_ on another machine. Instead, each layer passes data and control information o the layer immediately below it, until the lowest layer is reached. Below layer 1 is the physical medium, through which actual communication occurs. Between each pair of adjacent layers is an interface. The interface defines which primitive operations and services the lower layer makes available to the upper one. Clear-cut interfaces make it simpler to replace one layer with a completely different protocol or implementation because all that is required of the new protocol or implementation is that it offer the same set of services to its upstairs neighbor as the old one did. 

A set of layers and protocols is called a _network architecture_. The specification of an architecture must contain enough information to allow an implementer of an architecture to write a program or build the hardware for each layer so that it will correctly obey the appropriate protocol. Neither the details of the implementation nor the specification of the interfaces is part of the architecture because these are hidden away inside the machines and not visible from the outside. A list of the protocols used by a certain system, one protocol per layer, is called a _protocol stack_. 

## Design Issues for the Layers

Some of the key design issues that occur in computer networks will arise in layer after layer. 

Reliability is the design issue of making a network that operates correctly even though it is made up of a collection of components that are themselves unreliable. 

A second design issue concerns evolution of the network. Over time, networks grow larger and new designs emerge that need to be connected to the existing network. The structuring mechanism used to support change by dividing the overall problem and hiding information details is protocol layering. Many other strategies work as well. 