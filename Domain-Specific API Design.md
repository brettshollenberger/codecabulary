# Domain-Specific API Design

Although not widely implemented, standards do exist to improve the machine-readability of web APIs; in particular, hypermedia formats, and predictable formatting as in Collection+JSON can greatly improve the ability of applications to drive other applications. But even if these standards were widely adopted, there would be another, larger challenge to API design: conveying the semantics of a domain in a manner predictable enough for a machine to read. 

To begin designing domain-specific APIs in machine-readable ways, it helps to view the commonalities between many of today's APIs. Today's APIs:

* Deal with problems too complex to be understood all at once, so they're split into multi-step processes
* Begin the client at the same first step
* After each step in the process, the server provides the client with a number of possible next steps
* At each step, the client decides which step to take
* The server knows what counts as success, and when to stop

Since the client is ideally a layer built atop the server, it should use these characteristics to call down to the server, without coupling itself to the server's implementation details. This can be achieved by designing a domain-specific format representing the problem domain that incorporates hypermedia controls, which provide the potential next steps to the client and inform the browser of how to structure its HTTP requests, what response to expect, and suggesting how the client should incorporate each step into its workflow.

### Stealing Application Semantics

Without hypermedia controls, resources become the "prize at the end of the maze" of hypermedia controls. They're dead-end resources that don't suggest other related resources to move to next. 