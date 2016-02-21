## [LiveJournal Architecture](/blog/2007/7/9/livejournal-architecture.html)

    

    

A fascinating and detailed story of how LiveJournal evolved their system to scale. LiveJournal was an early player in the free blog service race and faced issues from quickly adding a large number of users. Blog posts come fast and furious which causes a lot of writes and writes are particularly hard to scale. Understanding how LiveJournal faced their scaling problems will help any aspiring website builder.

Site: http://www.livejournal.com/

## Information Sources

*   [LiveJournal](http://danga.com/words/2007_yapc_asia/yapc-2007.pdf) - Behind The Scenes Scaling Storytime*   [Google Video](http://video.google.com/videoplay?docid=-8953828243232338732)*   [Tokyo Video](http://tokyo2007.yapcasia.org/sessions/2007/02/tbd.html)*   [2005 version](http://www.danga.com/words/2005_mysqlcon/mysql-slides-2005.pdf)  

    ## Platform

    *   Linux*   MySql*   Perl*   Memcached*   MogileFS*   Apache  

    ## What's Inside?

    *   Scaling from 1, 2, and 4 hosts to cluster of servers.*   Avoid single points of failure.*   Using MySQL replication only takes you so far.*   Becoming IO bound kills scaling.*   Spread out writes and reads for more parallelism.*   You can't keep adding read slaves and scale.*   Shard storage approach, using DRBD, for maximal throughput. Allocate shards based on roles.*   Caching to improve performance with memcached. Two-level hashing to distributed RAM.*   Perlbal for web load balancing.*   MogileFS, a distributed file system, for parallelism.*   TheSchwartz and Gearman for distributed job queuing to do more work in parallel.*   Solving persistent connection problems.  

    ## Lessons Learned

    *   Don't be afraid to write your own software to solve your own problems. LiveJournal as provided incredible value to the community through their efforts.  

    *   Sites can evolve from small 1, 2 machine setups to larger systems as they learn about their users and what their system really needs to do.  

    *   Parallelization is key to scaling. Remove choke points by caching, load balancing, sharding, clustering file systems, and making use of more disk spindles.  

    *   Replication has a cost. You can't just keep adding more and more read slaves and expect to scale.  

    *   Low level issues like which OS event notification mechanism to use, file system and disk interactions, threading and even models, and connection types, matter at scale.  

    *   Large sites eventually turn to a distributed queuing and scheduling mechanism to distribute large work loads across a grid.    