## [The 7 Stages of Scaling Web Apps](/blog/2008/9/23/the-7-stages-of-scaling-web-apps.html)

    

    

By John Engales CTO, Rackspace. Good presentation of the stages a typical successful website goes through:  

*   Stage 1 - The Beginning: Simple architecture, low complexity. no redundancy. Firewall, load balancer, a pair of web servers, database server, and internal storage.
*   Stage 2 - More of the same, just bigger.
*   Stage 3 - The Pain Begins: publicity hits. Use reverse proxy, cache static content, load balancers, more databases, re-coding.
*   Stage 4 - The Pain Intensifies: caching with memcached, writes overload and replication takes too long, start database partitioning, shared storage makes sense for content, significant re-architecting for DB.
*   Stage 5 - This Really Hurts!: rethink entire application, partition on geography user ID, etc, create user clusters, using hashing scheme for locating which user belongs to which cluster.
*   Stage 6 - Getting a little less painful: scalable application and database architecture, acceptable performance, starting to add ne features again, optimizing some code, still growing but manageable.
*   Stage 7 - Entering the unknown: where are the remaining bottlenecks (power, space, bandwidth, CDN, firewall, load balancer, storage, people, process, database), all eggs in one basked (single datacenter, single instance of data).

    