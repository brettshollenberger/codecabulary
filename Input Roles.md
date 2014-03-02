# Input Roles

When considering how to collect input for a method, we're thinking about how to map the inputs that are available to the roles the method would ideally interact with. Collecting input isn't just about finding needed inputs--it's about determining how lenient to be in accepting many types of input, and whether to adapt the method's logic to suit the received collaborator types. 

We have several strategies for ensuring a method has collaborators which can be relied upon to play their assigned roles:

1) Coerce objects into the roles we need them to play
2) Reject unexpected values which cannot play the needed roles
3) Substitute known-good objects for unacceptable inputs 

# Guard the Borders
 Like customs checkpoints at national boundaries, we want to guard the inputs coming in at the borders of our code, which allows the rest of our methods to speak confidently about how they interact with the various objects in our code. If we think of objects in terms of neighborhoods, we can rely on just a few "interfacer" objects to act as gatekeepers for our code. Once objects pass through this border guard, they can be implicitly trusted to be good neighbors.

