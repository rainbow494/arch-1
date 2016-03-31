## [Facebook: An Example Canonical Architecture for Scaling Billions of Messages](/blog/2011/5/17/facebook-an-example-canonical-architecture-for-scaling-billi.html)

    

    

![](http://farm5.static.flickr.com/4145/5038714561_34183a0639_m.jpg)

What should the architecture of your scalable, real-time, highly available service look like? There are as many options as there are developers, but if you are looking for a general template, this architecture as described by Prashant Malik, Facebook's lead for the [Messages](/blog/2010/11/16/facebooks-new-real-time-messaging-system-hbase-to-store-135.html) back end team, in [Scaling the Messages Application Back End](https://www.facebook.com/notes/facebook-engineering/scaling-the-messages-application-back-end/10150148835363920#), is a very good example to consider. 

Although Messages is tasked with handling 135+ billion messages a month, from email, IM, SMS,  text messages, and Facebook messages, you may think this is an example of BigArchitecture and doesn't apply to smaller sites. Not so. It's a good, well thought out example of a non-cloud architecture exhibiting many qualities any mom would be proud of:

1.  **Layered** - components are independent and isolated. 
2.  **Service/API Driven** - each layer is connected via well defined interface that is the sole entry point for accessing that service. This prevents nasty complicated interdependencies. Clients hide behind an application API. Applications use a data access layer.
3.  **Distributed** - layers are transparently distributed across clusters of machines
4.  **Separate Application Logic** - Application logic is encapsulated in application servers that provide an API endpoint. Application logic is implemented in terms of other services. The application server tier also hides a write-through cache as this is the only place user data is written or retrieved, it is the perfect spot for a cache.
5.  **Stateless** - State is kept in a database tier, caching tier, or other services, not in applications or stuffed in local pockets.
6.  **Scalable Component Services** - Other services that themselves are scalable and highly available are used to implement this service. Messages also uses: Memcache, User index service, and [HayStack](http://haystacksearch.org/).
7.  **Full Stack Ops** - Tools were developed to manage, monitor, identify performance issues and upgrade the entire service, up, down and across the stack.
8.  **Celled** - Messages has as the basic building block of their system a cluster of machines and services called a **cell**. If you need more power captain, then cells are added in an incremental fashion. A cell consists of [ZooKeeper controllers](/blog/2008/7/15/zookeeper-a-reliable-scalable-distributed-coordination-syste.html), an application server cluster, and a [metadata store](https://www.facebook.com/note.php?note_id=454991608919). Cells provide isolation as the storage and application horsepower to process requests is independent of other cells. Cells can fail, be upgraded, and distributed across datacenters independent of other cells.

Qualities 1-7 are well known and fairly widely acknowledge and deployed in some form or another. The _cell_ idea isn't as widely deployed as it ratchets up the abstraction level to _11_.

Salesforce has a similar idea called a [pod](http://www.salesforce.com/dreamforce/DF09/pdfs/BKSP005_Moldt.pdf)_._ For Salesforce each pod consists of 50 nodes, Oracle RAC servers, and Java application servers. Each pod supports many thousands of customers. We've seen other products bundle shards in a similar way. A shard would consist of a database cluster and storage that hides a master-slave or a master-master configuration into a highly available and scalable unit. [Flickr](http://highscalability.com/flickr-architecture) is an early and excellent example of this strategy. 

The really interesting bits of the article is how cluster management within a cell is handled and how the management of the cells as a unit is handled. That's the hard part to get right.

In a cluster of machines, nodes and thus the services on those nodes can twinkle in and out of existence at any time. Configuration can change also change at any time? How do you keep all systems in the cell coordinated? ZooKeeper. ZooKeeper is used for high availability, sharding, failover, and services discovery. All essential features for a cluster of fallible machines. Within a cell application servers form a consistent hash ring and users are shard across these servers.

What about cross cell operations, how are they handled? Messages has a _Discovery Service_ that is a kind of User DNS, it sits above the cells and maintains a user to cell mappings. If you want to access user data you have to find the correct cell first using the service.  In the cell a _distributed logic client_ acts as the interface to the Discovery Service and processes ZooKeeper notifications.

The [article](https://www.facebook.com/notes/facebook-engineering/scaling-the-messages-application-back-end/10150148835363920#) is very well written and has a lot of detail. Well worth reading, especially if you are trying to figure out how to structure your own service.

## Related Articles         

*   [Facebook's New Real-Time Messaging System: HBase To Store 135+ Billion Messages A Month](/blog/2010/11/16/facebooks-new-real-time-messaging-system-hbase-to-store-135.html)
*   [ZooKeeper - A Reliable, Scalable Distributed Coordination System ](/blog/2008/7/15/zookeeper-a-reliable-scalable-distributed-coordination-syste.html)
*   [Facebook Articles on HighScalability](http://highscalability.com/blog/category/facebook)
*   [A Look Inside the Force.com Infrastructure](http://www.salesforce.com/dreamforce/DF09/pdfs/BKSP005_Moldt.pdf) by Claus Moldt

    