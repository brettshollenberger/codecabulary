# Document-Oriented Databases

Developers tend to be familiar with relational databases--they're the ones we learn about in school and the ones that have powered our big projects. You've likely also heard about document-oriented databases or NoSQL databases like MongoDB and wondered what the hype was about and what the differences were between traditional relational databases and nonrel databases.

#### 1) No Normalization / Richer Object Modelling

In our object-oriented languages, we have full, rich representations of objects that we serialize into storable data. Non-relational databases still serialize data, but single objects are represented as single complex data structures in NoSQL databases; in other words, we don't normalize the data.

Normalization is the process of decomposing objects into simpler data-types that have a fairly standardized structure. We can't know how many comments a blog post will have, so we might break a comment out into its own table with a standardized set of properties (commenter and text), and we add on `post_id`, the primary key of the post on which the comment exists. This allows a comment to follow a predictable structure, and for it to still be assignable to a comment. 

