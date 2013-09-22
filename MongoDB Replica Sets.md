# MongoDB Replica Sets

MongoDB replica sets are much like the classic master/slave relationship in relational databases. 

* They're used to maximize reliability of database resources by replicating the primary database to one or more secondary databases whenever a write is performed (which can only be performed to the primary database). 
* They allow horizontal scaling for high read/low write & update environments. A load balancer can be used to route requests to appropriate slaves to improve read performance as an application scales.
* One unique quality: Mongo replica sets automate failover. When the primary database fails, the cluster will automatically promote a secondary resource to primary. When the old primary comes back online, it will do so as a secondary resource. Automated failover is still potentially contentious because it's hard to get right, and there are parts of the application stack that DevOps teams will be hesitant to automate (failover being a big one). 