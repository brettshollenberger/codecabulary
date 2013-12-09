# Patterns of Enterprise Architecture

#### Architecture

1) Is defined differently across the software space
2) Often refers to the shared understanding expert developers have about a project, and the way its major components interact
3) Refers, specifically to the things that are difficult to change

> If it's easier to change than you thought, it isn't architecture

#### Enterprise Applications

Enterprise applications tend to deal with:

1) Complex, persisted data
2) Business logic (logic related to the enterprise)

The nature of enterprise applications causes several distinct architectural problems to face:

1) Over time, many different applications and types of users will access the same data
2) Over time, the systems that access data will need to change
3) Over time, the data schemas will be forced to change
4) In distributed systems, many concurrent users may access and interact with data at the same time, which can cause inconsistencies
5) Different users require different views and levels of detail about information
6) We have to integrate our systems with other systems in the enterprise
7) Integration efforts are often incomplete or messy
8) Different users perceive the model in subtly different ways, or talk about the models in different ways in their departments
9) Various systems require data to be in various formats
10) Business logic is often illogical, but difficult to change without significant political effort
11) Business logic can contradict itself

#### Organizing Domain Logic:

Fowler presents three methods for organizing domain logic:

1) Transaction script - Procedural, simple business transactions. Can share code via subroutines, but often results in duplication and exponentially increased complexity as domain logic complexity increases. Suffers from a lack of discernible organization.
2) Domain model - Organization of the nouns within the domain into classes that interact with a persistence layer, often via Row Data Gateway or Table Data Gateway. Though the initial cost in understanding domain model is the highest of the three, it offers the simplest, most linear scalability as domain logic complexity increases (and it always will).
3) Table module - Where domain model has one instance per db record, table module has one instance (the table). This works with a record set, and formalizes some bits of the transaction script model, while still primarily using procedural code. 

The basic advice of Fowler is that if a team knows domain model well, it's usually the best choice. If working in an environment where a lot of tools are geared towards working with record sets, like .NET, then table module can be a good choice, but otherwise it doesn't offer benefits over domain model.