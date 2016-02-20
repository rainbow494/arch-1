## [How Twitter Uses Redis to Scale - 105TB RAM, 39MM QPS, 10,000+ Instances ](/blog/2014/9/8/how-twitter-uses-redis-to-scale-105tb-ram-39mm-qps-10000-ins.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, September 8, 2014 at 9:05AM</span></div>

<div class="body">

<iframe align="right" width="220" height="200" src="//www.youtube.com/embed/rP9EKvWt0zo?rel=0" frameborder="0" allowfullscreen=""></iframe>[Yao Yue](https://twitter.com/thinkingfish) has worked on Twitter’s Cache team since 2010\. She recently gave a really great talk: [Scaling Redis at Twitter](https://www.youtube.com/watch?v=rP9EKvWt0zo). It’s about Redis of course, but it's not just about Redis.

Yao has worked at Twitter for a few years. She's seen some things. She’s watched the growth of the cache service at Twitter explode from it being used by just one project to nearly a hundred projects using it. That's many thousands of machines, many clusters, and many terabytes of RAM.

It's clear from her talk that's she's coming from a place of real personal experience and that shines through in the practical way she explores issues. It's a talk well worth watching.

As you might expect, Twitter has a lot of cache.

<div id="_mcePaste">Timeline Service for one datacenter using Hybrid List:</div>

<div id="_mcePaste">

*   ~40TB allocated heap
*   ~30MM qps
*   > 6,000 instances

</div>

<div id="_mcePaste">Use of BTree in one datacenter:</div>

<div id="_mcePaste">

*   ~65TB allocated heap
*   ~9MM qps
*   >4,000 instances

</div>

You'll learn more about BTree and Hybrid List later in the post.

A couple of points stood out:

*   Redis is a brilliant idea because it takes underutilized resources on servers and turns them into valuable service.
*   Twitter specialized Redis with two new data types that fit their use cases perfectly. So they got the performance they needed, but it locked them into an older code based and made it hard to merge in new features. I have to wonder, why use Redis for this sort of thing? Just create a timeline service using your own datastructures. Does Redis really add anything to the party?
*   Summarize large chunks of log data on the node, using your local CPU power, before saturating the network.
*   If you want something that’s high performance separate the fast path, which is the data path, away from the slow path, which is the command and control path. 
*   Twitter is moving towards a container environment with Mesos as the job scheduler. This is still a new approach so it's interesting to hear about how it works. One issue is the Mesos wastage problem that stems from requirement to specify hard resource usage limits in a complicated runtime world.
*   A central cluster manager is really important to keep a cluster in a state that’s easy to understand.
*   The JVM is slow and C is fast. Their cache proxy layer is moving back to C/C++.

With that in mind, let's learn more about how Redis is used at Twitter:

## <span>Why Redis?</span>

*   <span>Redis drives Timeline, Twitter’s most important service. Timeline is an index of tweets indexed by an id. Chaining tweets together in a list produces the Home Timeline. The User Timeline, which consists of tweets the user has tweeted, is just another list.</span>

*   <span>Why consider Redis instead of Memcache? The Network Bandwidth Problem and The Long Common Prefix Problem.</span>

*   <span>The Network Bandwidth Problem.</span>

    *   <span>Memcache didn’t work as well as Redis for the timeline. The problem was dealing with fanout.</span>

    *   Twitter read and writes **happen incrementally** and they are fairly small, but the timelines themselves are fairly large.

    *   <span>When a tweet is generated it needs to be written to all relevant timelines. The tweet is a small piece of data that is attached to some data structure. On read it’s desirable to load a small batch of tweets. On a scroll down another batch is loaded.</span>

    *   <span>The hometime line can be largish, what is reasonable for a viewer to read in one set. Maybe 3000 entries, for example. Which means for performance reasons accessing the databases should be avoided.</span>

    *   <span>A read-modify-write cycle  for incremental writes, and small reads, on large objects (the timeline), is too expensive and creates a network bottleneck.</span>

    *   <span>On a gigalink at 100K+ reads and writes per second, if the average object size is more than 1K, the network becomes the bottleneck.</span>

*   <span>The Long Common Prefix Problem (really two problems)</span>

    *   <span>A flexible schema approach is used for data formats. An object has certain attributes that may or may not exist. A separate key can be created for each individual attribute. This requires sending out a separate request for each individual attribute and not all attributes may be in the cache.</span>

    *   <span>Metrics that are observed over time have the same name with each sample having a different time stamp. If storing each metric individually the long common prefix is being stored many many times. </span>

    *   <span>To be more space efficient in both scenarios, for metrics and a flexible schema, it is desirable to have a hierarchical key space.</span>

*   **<span>A dedicated caching cluster</span> <span>under utilizes CPUs</span>**<span>. For simple cases, in-memory key-value stores are CPU light. 1% of CPU time  on a box can handle more than 1K requests per second for small key values. Though for different data structures the result can be different.</span>

*   <span>**Redis is a brilliant idea**</span><span>. It sees what the server can do, but is not doing. For simple key-value stores, there’s a lot of CPU headroom on the server side for a service like Redis.</span>

*   <span>Redis was first used within Twitter in 2010 for the Timeline service. It is also used in the Ads service.</span>

*   <span>The on disk features of Redis are not used. Partly this is because inside Twitter the Cache and Storage services are in different teams so they use whatever mechanisms they think best. Partly this may be because the Storage team thinks another service fits their goals better than Redis.</span>

*   <span>Twitter forked Redis 2.4 and added some features to it, so they are stuck at 2.4 (2.8.14 is the latest stable version). Changes were: two data structure features within Redis; in-house cluster management features; in-house logging and data insight.</span>

*   <span>Hotkeys are a problem so they are a building a tiered caching solution with client side caching that will automatically cache hotkeys.</span>

## <span>Hybrid List</span>

*   Added Hybrid List to Redis for **more predictable memory performance**.

*   <span>Timeline is a list of Tweet IDs, so it’s a list of integers. Each ID is small.</span>

*   <span>Redis supports two list types: ziplist and linklist. Ziplist is space efficient. Linked list is flexible, but as a doubly linked list has the overhead of two pointers per key, which given the size of the ID is very high overhead.</span>

*   <span>To use memory efficiently ziplists are used exclusively.</span>

*   <span>A Redis ziplist threshold is set to the max size of a Timeline. Never store a bigger Timeline than can be stored in a ziplist. This means </span>a product decision, how many tweets can be in a Timeline, are linked to a low level component (Redis). Generally not desirable.

*   <span>Adding to and deleting from a ziplist is inefficient, especially with a very large list. Deleting from a ziplist uses memmove to move data around, to make sure the list is still contiguous. Adding to a ziplist requires a memory realloc call to make enough space for the new entry.</span>

*   Potential **high latency for write operations due to Timeline size**. Timelines vary a lot in size. Most users don’t tweet very much, so their User Timeline is small. Home Timelines, especially those involving celebreties can be huge. When updating a large timeline and the cache runs out of heap, which is often the case when using a cache, a very large number of very **small timelines will be evicted** before there’s enough contiguous RAM to handle one big ziplist. As all this cache management takes time, a write operation can have a high latency.

*   Since writes are fanned out to a lot of timelines there’s a higher chance to be caught in a **write latency trap** as memory is used for expanding the timelines.

*   It’s **hard to create a SLA** for write operations given the high variability of write latencies.

*   Hybrid List is a linked list of ziplists. A threshold is set of how big each ziplist can be in bytes. In bytes because to **memory efficient** it helps to **allocate and deallocate blocks of the same size**. When a list goes over it is spilled into the next ziplist. A ziplist is not recycled until the list is empty, which means it is possible, through deletion, to have each ziplist have only one entry. In practice, tweets aren’t deleted all that often.

*   <span>Before Hybrid List a workaround was to expire larger timelines more quickly, which freed up memory for other timelines, but was expensive when a user went to view their timeline.</span>

## <span>BTree</span>

*   <span>Added BTree to Redis to support range queries on hierarchical keys to return a list of results.</span>

*   <span>In Redis the way to deal with secondary keys or fields is a hash map. To have sorted data in order to perform a range query a sorted set is used. Sorted set orders by a score which is a double, so an arbitrary secondary key or an arbitrary name can’t be used for the sorting. Since hash map uses a linear search it’s not great if there are a lot of secondary keys or fields.</span>

*   BTree is the attempt fix the shortcomings of hash map and sorted set. It’s better to just have **one data structure that does what you want**. It’s easier to understand and reason about.

*   Borrowed the BSD implementation of BTree and added it to Redis to create a BTree. Supports key lookup as well as range query. Has good lookup performance. The code is relatively simple. The downside is **BTree is not memory efficient**. It has a lot of meta data overhead due to the pointers.

## <span>Cluster Management</span>

*   <span>A cluster is using more than one instance of Redis for a single purpose. If a data set is larger than a single Redis instance can handle or throughput is higher than what a single instance can handle, the key space will need to be partitioned so the data can be stored in more than one shard, across a set of instances. Routing is taking a key and figuring out which shard the data for the key is on.</span>

*   Thinks cluster management is the **number one reason Redis adoption hasn’t exploded**. When a cluster is available there’s no reason not to **migrate all cache use cased to Redis**.

*   <span>Tricky to get Redis cluster right. People use Redis because as a data structure server the idea is to perform frequent updates. But a lot of Redis operations are not idempotent. If there’s a network glitch a retry is required and the data can be corrupted.</span>

*   Redis cluster **favors having a** **centralized manager** **dictating the global view**. With memcache a lot clusters use a client side approach based on consistent hashing. If there’s inconsistent data, so be it. To provide really good services, a cluster needs features like detecting which shard is down and then replaying operations to get back in sync. After a long enough period spent down cache state should be cleaned up. Corrupted data in Redis is hard to detect. When there’s a list and it’s missing a chunk, it’s hard to tell.

*   <span>Twitter has multiple attempts at building a Redis cluster.</span> [<span>Twemproxy</span>](https://github.com/twitter/twemproxy) <span>which is not used by Twitter internally, it was built for</span> [<span>Twemcache</span>](https://github.com/twitter/twemcache) <span>and Redis support was added. Two more solutions were based on proxy style routing. One was associated with the Timeline service and not meant to be general. The second was a generalization of the Timeline solution that provided cluster management, replication, and shard repairing.</span>

*   **Three options in a cluster**: servers talk to each other to reach agreement of what a cluster looks like; use a proxy; or do client side cluster management where the clients form a quorum.

*   Didn’t go with a server approach because the philosophy is to **keep servers simple****, dumb and fast**.

*   <span>Didn’t go with the client because</span> <span>**changes are hard to propagate**</span><span>. Approximately 100 projects in Twitter use a cache cluster. Changing anything in the client would have to be pushed to 100 clients it could take years for changes to propagate. Quick iteration means it’s almost impossible to put code in the client.</span>

*   Went with a proxy style routing approach and partitioning for two reasons. A cache service is a high performance service. If you want something that’s **high performance separate the fast path, which is the data path, away from the slow path****, which is the command and control path**. If cluster management is merged into the server it complicates the code for Redis, which is a stateful service, any time you want to **fix a bug or provide an upgrade** to the cluster management code, the stateful Redis service must be restarted too, which will potentially throw away a bunch of data. A rolling restart of a cluster is painful.

*   There was a concern using the proxy approach that another network hop is inserted between the client and the server. **Profiling showed the extra hop is a myth**. At least in their ecosystem. Latency to through the Redis server was less than .5 milliseconds. At Twitter most of the backend services are Java based and use Finagle to talk to each other. When going through the Finagle path the latency was close to 10 milliseconds. So the extra hop isn’t the problem. **Inside the JVM is the problem**. Outside the JVM you can do pretty much whatever you want, unless of course you go through another JVM.

*   <span>Failure of a proxy doesn’t matter much. On the data path introducing a proxy layer isn’t so bad. The client doesn’t care which proxy they talk to. If a proxy fails after a timeout the client goes to another proxy. No sharding is happening at the proxy level, they are all stateless. To scale throughput simply add more proxies. The tradeoff is additional cost. The proxy layer is allocated resources just to do the forwarding. Cluster management, sharding, and doing the view of the cluster happens outside the proxies. The proxies don’t have to agree with each other.</span>

*   <span>Twitter has instances that have</span> <span>**100K open connections**</span> <span>and it works fine. There’s just overhead to pay. There’s no reason to close connections. Just keep them open, it improves latency.</span>

*   Cache clusters are used as a [<span>look-aside cache</span>](http://www.quora.com/Is-Memcached-look-aside-cache). The caches themselves are not responsible for data replenishment. The client is responsible for fetching a missing key from storage then caching it. If a node goes down the shard is moved to another node. The failed machine is flushed when it comes back so no data is left around. All this is done by the **cluster leader**. A central viewpoint is really important to keep a cluster in a state that’s easy to understand.

*   Did an experiment with a proxy written in C++. The **C++ proxy saw a** **significant performance increase** (no number given). The proxy tier is being moved back to C and C++.

## <span>Data Insight</span>

*   When there’s a call saying the cache system is misbehaving **most of the time the cache is fine**. Usually the clients are configured wrong. Or they are abusing the cache system by requesting way too many keys. Or requesting the same key over and over again and saturating the server or the link.

*   When you tell someone they are abusing your system **they want proof**. Which key? Which shard is bad? What kind of traffic leads to this behaviour? Proof requires metrics and analysis that can be shown to customers.

*   An SOA architecture doesn’t give you problem isolation or make debugging easier automatically. You have to have **good visibility** **into every component** that makes up the system.

*   <span>Decided to build Insight into caching. The cache is written in C and is fast, so it can provide data that other components can’t. Other compents can't handle the load of providing data for every request.</span>

*   **Logging every single command is possible**. The cache can log everything at 100K qps. Only meta data is logged, values are not logged (Good joke about the NSA).

*   **Avoid locking and blocking**. Especially don’t block on disk writes.

*   At 100 qps and a 100 bytes per log message, each box will log 10MB of data per second. That’s a lot of data to move off the box. 10% of network bandwidth would be used just in case something went bad. **Economically not feasible**.

*   <span>**Precompute logs on the box to reduce costs**</span><span>. Assumption is that it is already knows what will be computed. A process reads the logs and generates a summary and periodically sends this view of the box. The view is tiny compared to the original data.</span>

*   <span>View data is aggregated by Storm, stored, and there’s a visualization system sitting on top. You can get data like here are your top 20 keys; here’s your traffic by second and there’s a peak which means the traffic pattern is spiky; here’s are the number of unique keys, which helps with capacity planning. A lot can be done when every single log is captured.</span>

*   <span>Insight is very valuable for operations. If there are packet drops often that can be linked to either a hot key or spiky traffic behaviour.</span>

## <span>Wish List for Redis</span>

*   <span>Explicit memory management</span><span>.</span>

*   <span>**Deployable (Lua) Scripts**</span><span>. Talked about near the start.</span>

*   <span>**Multi-threading**</span><span>. Would make cluster management easier. Twitter has a lot of “tall boxes,” where a host has 100+ GB of memory and a lot of CPUs. To use the full capabilities of a server a lot of Redis instances need to be started on a physical machine. With multi-threading fewer instances would need to be started which is much easier to manage.</span>

## <span>Lessons Learned</span>

*   <span>**Scale demands predictability**</span><span>. The larger the cluster, the more customers, the more predictable and deterministic you want your service to be. When there’s one customer and there’s a problem you can dig into a problem and it’s intriguing. When you have 70 customers you can’t keep up.</span>

*   <span>**Tail latencies matter**</span><span>. When you do fanouts to a lot of shards, when one is slow your entire query will be slow.</span>

*   <span>**Deterministic configuration is operationally important**</span><span>. Twitter is moving towards a container environment. Mesos is used as the job scheduler. The scheduler fulfills the request for the amount of CPU, memory etc. A monitor kills any job that goes over its resource requirement. Redis causes a problem in a container environment. Redis introduces external fragmentation, meaning you use more memory to store the same amount of data. If you don’t want to be killed you have to compensate for that with oversupply. You have to think my memory fragmentation ratio won’t go over 5%, but I’ll allocate 10% more as a buffer space. Maybe even 20%. Or I think I’ll get 5000 connections per host, but just in case let me allocate memory for 10,000 connections. The result is a huge potential for waste. Super low latency services don’t play well with Mesos today, so these jobs are isolated from other jobs.</span>

*   <span>**Knowing your resource usage at runtime is really helpful**</span><span>. In a large cluster bad stuff happens. You think you are safe but things happen and behaviour is unexpected. Most services today can’t degrade gracefully. For example, when a limit of 10GB of RAM is reached then requests are rejected until there’s free RAM. This only fails a small percentage of traffic that’s proportional to the resource that they require. That's graceful. Garbage collection problems are not graceful, traffic just gets dropped on the floor, this problem affects a lot of teams in a lot of companies every day.</span>

*   <span>**Push computation to the data**</span><span>. If you look at relative network speeds, CPU speeds, and disk speeds, it makes sense to do computation before going to disk and do computation before going to the network. An example is summarizing logs on a node before they are pushed to a centralized monitoring service. LUA in Redis another way to apply computation close to the data.</span>

*   <span>**LUA is not production ready in Redis today**</span><span>. On demand scripting means service providers can’t guarantee their SLA. A loaded script can do anything. What service provider would want to take the risk of blowing their SLA because of someone elses code? A deployment model would be better. It would allow for code review and benchmarking, so resource usage and performance could be properly calculated.</span>

*   <span>**Redis as the next high performance stream processing platform**</span><span>. It has pub-sub and scripting. Why not?</span>

## <span>Related Articles</span>

*   <span>[Google: Taming The Long Latency Tail - When More Machines Equals Worse Results](http://highscalability.com/blog/2012/3/12/google-taming-the-long-latency-tail-when-more-machines-equal.html)</span>[The Architecture](http://highscalability.com/blog/2013/7/8/the-architecture-twitter-uses-to-deal-with-150m-active-users.html)
*   [Twitter Uses To Deal With 150M Active Users, 300K QPS, A 22 MB/S Firehose, And Send Tweets In Under 5 Seconds](http://highscalability.com/blog/2013/7/8/the-architecture-twitter-uses-to-deal-with-150m-active-users.html)
*   <span>[Troubles With Sharding - What Can We Learn From The Foursquare Incident?](http://highscalability.com/blog/2010/10/15/troubles-with-sharding-what-can-we-learn-from-the-foursquare.html)</span>
*   [Log Everything All The Time](http://highscalability.com/blog/2007/8/30/log-everything-all-the-time.html)
*   [Low Level Scalability Solutions - The Aggregation Collection](http://highscalability.com/blog/2013/3/6/low-level-scalability-solutions-the-aggregation-collection.html)

</div>