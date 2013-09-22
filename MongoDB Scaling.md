# Scaling MongoDB

The easiest way to scale most databases is vertically--increasing the power of their hardware. Vertical scaling is simple, reliable, and to a point, cost-effective. But what happens when you can't scale vertically any longer and you still need greater computing power? You begin scaling horizontally (or maybe you've been doing this the whole time) -- you can begin to distribute the database across multiple machines. Horizontal scaling mitigates the consequences of failure and reduces costs, since cheaper hardware can be used.

MongoDB manages horizontal scaling for you via auto-sharding, a process that manages the distribution across nodes. MongoDB handles the addition of shard nodes as well as automatic failover. 