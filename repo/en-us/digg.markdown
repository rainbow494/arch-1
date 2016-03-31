## [Digg - Looking to the Future with Cassandra](/blog/2009/10/29/digg-looking-to-the-future-with-cassandra.html)

    

    

Digg has been researching ways to scale our database infrastructure for some time now. We’ve adopted a [traditional vertically partitioned master-slave](http://blog.digg.com/?p=213) configuration with MySQL, and also investigated sharding MySQL with [IDDB](http://blog.digg.com/?p=607). Ultimately, these solutions left us wanting. In the case of the traditional architecture, the lack of redundancy on the write masters is painful, and both approaches have significant management overhead to keep running.

Since it was already necessary to abandon data normalization and consistency to make these approaches work, we felt comfortable looking at more exotic, non-relational data stores. After considering HBase, Hypertable, Cassandra, Tokyo Cabinet/Tyrant, Voldemort, and Dynomite, we settled on [Cassandra](http://incubator.apache.org/cassandra/).

Each system has its own strengths and weaknesses, but Cassandra has a good blend of everything. It offers column-oriented data storage, so you have a bit more structure than plain key/value stores. It operates in a distributed, highly available, peer-to-peer cluster. While it’s currently lacking some core features, it gets us closer to where we want to be than the other solutions.

[continue...](http://blog.digg.com/?p=966) 

    