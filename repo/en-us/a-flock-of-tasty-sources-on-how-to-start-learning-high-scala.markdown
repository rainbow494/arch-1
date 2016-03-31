## [A Flock of Tasty Sources on How to Start Learning High Scalability](/blog/2014/11/24/a-flock-of-tasty-sources-on-how-to-start-learning-high-scala.html)

    

    

_This is a [guest repost](http://leandromoreira.com.br/2014/11/20/how-to-start-to-learn-high-scalability/) by [Leandro Moreira](http://www.linkedin.com/in/leandromoreira)._

    

        ![distributed systems](http://s.hswstatic.com/gif/dns-rev-3.jpg)        

When we usually are interested about scalability we look for links, explanations, books, and references. This mini article links to the references I think might help you in this journey.

> **DISCLAIMER:**

> You don’t need to have _N_ machines to build/test a cluster/high scalable system, currently you can use [Vagrant](https://github.com/leandromoreira/loja-virtual-puppet/blob/master/Vagrantfile) and up _N_ machines easily.

**THE REFERENCES:**

Now that you know you can empower yourself with virtual servers, I challenge you to not only read these links but put them into practice.

*   First of all, motivate yourself by watching this [tutorial using nodejs + nginx + applying static caching + load balancing + testing](https://www.youtube.com/watch?v=FJrs0Ar9asY), all this in 7 minutes.
*   Add these words and their meaning to your vocabulary: [scalability](http://en.wikipedia.org/wiki/Scalability), [failover](http://en.wikipedia.org/wiki/Fault_tolerance), [single point of failure (SPOF)](http://en.wikipedia.org/wiki/Single_point_of_failure), [sharding](http://en.wikipedia.org/wiki/Shard_(database_architecture)), [replication](http://en.wikipedia.org/wiki/Replication_(computing)) and [load balancing;](http://en.wikipedia.org/wiki/Load_balancing_(computing)) even if you don’t understand them completely.
*   In order to have a general overview and the reasons/whys about scalable systems, I strongly recommend you to read [Scalable Web Architecture and Distributed Systems](http://aosabook.org/en/distsys.html). This is a great introduction.
*   After you get the general idea you can move on to understand how to use a [load balancer](http://nginx.org/en/docs/http/load_balancing.html) and what [decisions](https://help.utk.edu/kb/index2.php?func=show&e=1699) and [problems](http://dev.housetrip.com/2014/01/14/session-store-and-security/) you will face. And then you can try to run a [haproxy](https://www.digitalocean.com/community/tutorials/how-to-use-haproxy-to-set-up-http-load-balancing-on-an-ubuntu-vps) and make it not a [single point of failure too.](http://www.howtoforge.com/high-availability-load-balancer-haproxy-heartbeat-debian-etch)
*   Dare yourself to [serve 3 million requests per second](http://dak1n1.com/blog/10-3-million-http-cluster) but for this task you’ll need to [generate 3 million requests](http://dak1n1.com/blog/14-http-load-generate), [fine tune your web server](http://dak1n1.com/blog/12-nginx-performance-tuning) and finally [scale and test it](http://dak1n1.com/blog/13-load-balancing-lvs).
*   Your application is already scalable, now you need to scale your databases. They are very important part of your application, here I recommend you to read at least how MongoDB scales with [sharding](http://docs.mongodb.org/manual/core/sharding-introduction/) and [replication](http://docs.mongodb.org/manual/core/replication-introduction/) and Cassandra with its almost [linear scalability](http://techblog.netflix.com/2011/11/benchmarking-cassandra-scalability-on.html) and the ease of [adding nodes to the cluster.](http://www.datastax.com/documentation/cassandra/2.0/cassandra/operations/ops_add_node_to_cluster_t.html)
*   Since your application and database are scalable and fault tolerant, it’s good to [save your servers unnecessary workload](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching?hl=en) and also make the responses to the [user faster](http://chimera.labs.oreilly.com/books/1230000000545/ch13.html#CACHE_RESOURCES). Learn that a good request is the one that never reached the “real server”.
*   Let’s assume we’re deploying the whole infrastructure within a single data center, now we have another SPOF. Since all servers are in the same space, some natural disaster might happen or even the simple power outages. Good news is that [Cassandra have support to multiple data center out of the box](http://www.datastax.com/documentation/cassandra/2.0/cassandra/initialize/initializeMultipleDS.html) and you can see how [google face this issue](http://highscalability.com/blog/2009/8/24/how-google-serves-data-from-multiple-datacenters.html). If your user is on Brazil, don’t make him travel longer than he needs and remember even with the best situation [we still have latency](http://chimera.labs.oreilly.com/books/1230000000545/ch01.html#SPEED_FEATURE).

**Good questions to test your knowledge:**

*   Why to scale? how people do that usually?
*   How to deal with user session on memory RAM with N servers? how LB know which server is up? how LB knows which server to send the request?
*   Isn’t LB another SPOF? how can we provide a failover for LB?
*   Isn’t my OS limited by 64K ports? is linux capable of doing that out of the box?
*   How does mongo solves failover and high scalability? how about cassandra? how cassandra does sharding when a new node come to the cluster?
*   What is cache lock? What caching policies should I use?
*   How can a single domain have multiple IP addresses (ex: $ host [www.google.com](http://www.google.com))? What is BGP? How can we use DNS or BGP to serve geographically users?

**Bonus round:** sometimes simple things can achieve your goals of making even an [AB test](http://viget.com/extend/split-test-traffic-distribution-with-nginx).

    Please let me know any mistake, I’ll be happy to fix it.    

    

    