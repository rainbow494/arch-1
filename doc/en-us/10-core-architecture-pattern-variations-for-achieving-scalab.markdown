## [10 Core Architecture Pattern Variations for Achieving Scalability](/blog/2011/11/7/10-core-architecture-pattern-variations-for-achieving-scalab.html)

    

    

![](http://farm7.static.flickr.com/6039/6300004742_01772c950a_o.jpg)

Srinath Perera has put together a [strong list of architecture patterns](http://srinathsview.blogspot.com/2011/10/list-of-known-scalable-architecture.html) based on three meta patterns:  distribution, caching, and asynchronous processing. He contends these three are the primal patterns and the following patterns are but different combinations:

1.  **LB (Load Balancers) + Shared nothing Units**. Units that do not share anything with each other fronted with a load balancer that routes incoming messages to a unit based on some criteria.
2.  **LB + Stateless Nodes + Scalable Storage**. Several stateless nodes talking to a scalable storage, and a load balancer distributes load among the nodes.
3.  **Peer to Peer Architectures (Distributed Hash Table (DHT) and Content Addressable Networks (CAN))**. Algorithm for scaling up logarithmically.
4.  **Distributed Queues**. Queue implementation (FIFO delivery) implemented as a network service.
5.  **Publish/Subscribe Paradigm**. Network publish subscribe brokers that route messages to each other.
6.  **Gossip and Nature-inspired Architectures**. Each node randomly pick and exchange information with follow nodes.
7.  **Map Reduce/ Data flows**. Scalable pattern to describe and execute Jobs.
8.  **Tree of responsibility**. Break the problem down recursively and assign to a tree, each parent node delegating work to children nodes.
9.  **Stream processing**. Process data streams, data that is keeps coming.
10.  **Scalable Storages**. Ranges from Databases, NoSQL storages, Service Registries, to File systems.

Please read the original article, [List of Known Scalable Architecture Templates](http://srinathsview.blogspot.com/2011/10/list-of-known-scalable-architecture.html), for more details.

    