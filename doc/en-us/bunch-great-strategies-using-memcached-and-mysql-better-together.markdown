## [A Bunch of Great Strategies for Using Memcached and MySQL Better Together](/blog/a-bunch-of-great-strategies-for-using-memcached-and-mysql-be.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, August 4, 2008 at 5:06PM</span></div>

<div class="body">

The primero recommendation for speeding up a website is almost always to add cache and more cache. And after that add a little more cache just in case. Memcached is almost always given as the recommended cache to use. What we don't often hear is how to effectively use a cache in our own products. MySQL hosted two excellent webinars (referenced below) on the subject of how to deploy and use memcached. The star of the show, other than MySQL of course, is Farhan Mashraqi of Fotolog. You may recall we did an earlier article on Fotolog in [Secrets to Fotolog's Scaling Success](http://highscalability.com/secrets-fotologs-scaling-success), which was one of my personal favorites.  

Fotolog, as they themselves point out, is probably the largest site nobody has ever heard of, pulling in more page views than even Flickr. Fotolog has 51 instances of memcached on 21 servers with 175G in use and 254G available. As a large successful photo-blogging site they have very demanding performance and scaling requirements. To meet those requirements they've developed a sophisticated approach to using memcached that others can learn from and emulate. We'll cover some of the highlightable strategies from the webinar down below the fold.

## What is Memcached?

The first part of the first webinar gives a good overview of memcached. Just in case the rock you've been hiding under recently disintegrated, you may not have heard about memcached (as if). Memached is: _A high-performance, distributed memory object caching system, generic in nature, but intended for use in speeding up dynamic web applications by alleviating database load_. Memcached essentially creates an in-memory shard on top of a pool of servers from which an application can easily get and set up to 1MB of unstructured data. Memcached is two hash tables, one from the client to the server and another one inside the server. The magic is that none of the memcached servers need know about each other. To scale up you just add more servers and the key hashing algorithm makes it all work out right. Memcached is not redundant, has no failover, and has no authentication. It's simple server for storing and getting data, the complex bits must be implemented by applications.  

The rest of the first webinar is Farhan explaining in wonderful detail how they use memcached at Fotolog. The beginning of the second seminar covers much of the same ground as the first. In the last half of the second seminar Farhan gives a number of excellent code examples showing how memcached is installed, used, and managed. If you've never used memcached before there's a lot of good stuff presented.  

## Memcached and MySQL Go Better Together

There's a little embrace and extend in the webinar as MySQL cluster is presented several times as doing much the same job as memcached, but more reliably. However, the recommended approach for using memcached and MySQL is:

*   **Write scale the database by sharding**. Partition data across multiple servers so more data can be written in parallel. This avoids a single server becoming the bottleneck.*   **Front MySQL with a memcached farm to scale reads**. Applications access memcached first for data and if the data is not in memcached then the application tries the database. This removes a great deal of the load on a database so it can continue to perform it's transactional duties for writes. In this architecture the database is still the system of record for the true value of data.*   **Use MySQL replication for reliability and read query scaling**. There's an effective limit to the number of slaves that can be supported so just adding slaves won't work as scaling strategy for larger sites.  

    Using this approach you get scalable reads and writes along with high availability.  

    Given that MySQL has a cache, why is memcached needed at all?*   **The MySQL cache is associated with just one instance**. This limits the cache to the maximum address of one server. If your system is larger than the memory for one server then using the MySQL cache won't work. And if the same object is read from another instance its not cached.*   **The query cache invalidates on writes**. You build up all that cache and it goes away when someone writes to it. Your cache may not be much of a cache at all depending on usage patterns.*   **The query cache is row based**. Memcached can cache any type of data you want and it isn't limited to caching database rows. Memcached can cache complex complex objects that are directly usable without a join.  

    ## Cache everything that is slow to query, fetch, or calculate.

    This is Fotolog's rule of deciding what to cache. What is considered slow depends on your requirements. But when something becomes slow it's a candidate for caching.  

    ## Fotolog's Caching Typology

    Fotolog has come up with an interesting typology of their different caching strategies:*   **Non-Deterministic Cache** - the classic memcached model of reading through the cache and writing to the database.*   **State Cache** - maintain current application state in cache.*   **Proactive Cache** - push changes from the database directly to the cache.*   **File System Cache** - save NFS load by serving files from the cache instead of the file system.*   **Partial Page Cache** - cache displayable page elements, not just data.*   **Application Based Replication** - use a client side API to hide all the low level details of interacting with the cache.  

    ## Non-Deterministic Cache

    This is the typical way of using memcached. I think the non-determinism comes in because an application can't depend on data being in the cache. The data may have been evicted because a slab is full or because the data simply hasn't been added yet. Other Fotolog strategies are deterministic, which means the application can assume the data is always present.  

    ### Useful For

    *   Ideal for complex objects that are read several times. Especially for sharded environments where you need to collect data from multiple shards.*   Good replacement for MySQL query cache*   Caching relationships and other lists*   Slow data that’s used across many pages*   Don’t cache if its more taxing to cache than you’ll save*   Tag clouds and auto-suggest lists  

    For example, when a photo is uploaded the photo is uploaded on the page of every friend. These lists are taxing to calculate so they are cached.  

    ### Usage Steps

    *   Check memcached for your data key.*   If the data doesn't exist in the cache then check database.*   If the data exists in the database then populate memcached.  

    ### Stats

    *   45 memcached instances dedicated to nondeterministic cache*   Each instance (on average):  
    * ~440 gets per second  
    * ~40 sets per second  
    * ~11 gets/set  

    Fotolog likes to characterize their caching policy in terms of the ratio of gets to sets.  

    ### Potential Problems

    *   While memcached is usually a light CPU user, Fotolog ran their cache on their application servers and found some problems:  
    * 90% CPU usage  
    * Memory garbage collected nearly once a minute  
    * Experienced blocking on memcached on app servers*   Operations are not transactional. One work around is to set expirations so stale data doesn't stay around long.  

    ## State Cache

    Keeps the current state of an application in cache.  

    ### Useful For

    *   Expensive operations.*   Sessions. If a memcached server goes down just make people login again.*   Keep track of who's online and their current status, especially for IM applications. Use memcached or the load would cripple the database.  

    ### Usage Steps

    It's a form of non-deterministic caching so the same steps apply.  

    ### Stats

    *   9G dedicated. Depending on the number users all the users can keep in cache.  

    ## Deterministic Cache

    Keep all data for particular database tables in the cache. An application always assumes that the data they need is in the cache (deterministic), applications never go to the database for data. Applications don't have to check memcached before accessing the data. The data will exist in the cache because the database is essentially loaded into the cache and is always kept in sync.  

    ### Useful For

    *   Read scalability. All reads go through the cache and the database is completely offloaded.*   Ideal for caching things that have no expiration.*   Heavily accessed data/objects/lists*   User credentials*   User profiles*   User preferences*   Active media belonging to users, like photo lists.*   Outsourcing logins to memcached. Don't hit database on login. All logins are processed through the cache.  

    ### Usage Steps

    *   Multiple dedicated cache pools are maintained. Instead of one large pool make separate standalone pools. Multiple pools are necessary for high availability. If a cache pool goes down then applications switch over to the next pool. If a multiple pools did not exist and a pool did go down then the database would be swamped with the load.*   All cache pools are maintained by the application. A write to the database means writing to multiple memcache pools. When an update to a user profile happens, for example, the update has to replicated to multiple caches from that point onwards.*   When the site starts after a shutdown then when it comes up the deterministic cache is populated before the site is up. The site is rarely rebooted so this is rare occurrence.*   Reads could also be load balanced against the cache pools for better performance and higher scalability.  

    ### Stats

    *   ~ 90,000 gets / second across cache cluster*   ~ 300 sets / second*   get/set ratio of ~ 300  

    ### Potential Problems

    *   Must have enough memory to hold everything.*   The database has rows and the cache may have objects. The database caching logic must know enough to create objects from the database schema.*   Maintaining the multiple caches seems complex. Fotolog uses Java and Hibernate. They wrote their own client to handle rotation through pools.*   Maintaining multiple caches adds a lot of overhead to the application. In practice there's little replication overhead compared to benefit.  

    ## Proactive Caching

    Data magically shows up in the cache. As updates happen to the database the cache is populated based on the database change. Since the cache is updated as the database is updated the chances of data being in cache are high. It's non-deterministic caching with a twist.  

    ### Useful For

    *   Pre-Populating Cache: Keeping memcached updated minimizes calls to database if object not present.*   “Warm up” cache in cases of cross data-center replication.  

    ### Usage Steps

    There are typically three implementation approaches:*   Parse binary log for updates. When an update is found perform the same operation on the cache.*   Implement user defined functions. Setup triggers that call UDF to update the cache. See http://tangent.org/586/Memcached_Functions_for_MySQL.html for more details.*   Use the Blackhole gambit. Facebook is rumored to use the Blackhole storage engine to populate cache. Data written to a Blackhole table is replicated to cache. Facebook uses it more to invalidate data and for cross country replication. The advantage of this approach is the data is not replicated through MySQL which means there are no binary logs for the data and it's not CPU intensive.  

    ## File System Caching

    NFS has significant overhead when used with a large number of servers. Fotolog originally stored XML files on a SAN and exposed them using NFS. They saw contention on these files so they put files in memcached. Big  
    performance improvements were seen and it kept NFS mounts open for other requests. Smaller media can also be stored in the cache.  

    ## Partial Page Caching

    Cache directly displayable page elements. The other caching strategies cache data used to create pages, but some things are still compute intensive and require a lot of work. So instead of just caching objects, prepare and cache entire page elements for reuse. For page elements that are accessed many times per second this can be a big win. For example: calculating top users in a region, popular photo list, and featured photo list. Especially when using sharding it can take some time to calculate these lists, so caching the resulting page elements makes a lot of sense.  

    ## Application Based Replication

    Write data to the cache through your own API. The API hides implementation details like:*   An application writes to one memcached client which writes to multiple memcached instances.*   Where the cache pools are and how many their are.*   Writing to multiple pools at the same time.*   Rotating to another pool on a pool failure until another pool is brought up and updated with data.  

    This approach is very fast if not network bound and very cost effective.  

    ## Miscellaneous

    There were a few suggestions on using memcached that didn't fit in any other section, so they're gathered here for posterity:*   Have a lot of nodes to handle loss. Losing a node with a few nodes will cause a spike on the database as everything reloads. Having more servers means less database load on failure.*   Use a warm standby that takes over IP of a memcached server that fails. This means you clients will not have to update their cache lists.*   Memcached can operate with UDP and TCP. Persistence connections are better because there's less overhead. Cache designed to use 1000s of connections.*   Use separate memcached servers to reduce contention with applications.*   Check that your slab sizes match the size of the data you are allocating or you could be wasting a lot of memory.  

    Here are some additional strategies from [Memcached and MySQL tutorial](http://download.tangent.org/talks/Memcached%20Study.pdf):*   Don't think row-level (database) caching, think complex objects.*   Don't run memcached on your database server, give your database all the memory it can get.*   Don't obsess about TCP latency - localhost TCP/IP is optimized down to an in-memory copy.*   Think multi-get - run things in parallel whenever you can.*   Not all memcached client libraries are made equal, do some research on yours.*   Instead of invalidating your data, expire it whenever you can - memcached will do all the work*   Generate smart keys - ex. on update, increment a version number, which will become part of the key*   For bonus points, store the version number in memcached - call it generation*   The latter will be added to Memcached soon - as soon as Brian gets around to it  

    ## Final Thoughts

    Fotolog has obviously put a great deal of thought and effort into creating sophisticated scaling strategies using memcached and MySQL. What I'm struck with is the enormous amount of effort that goes into syncing rows and objects back and forth between the cache and the database. Shouldn't it be easier? What role is the database playing when the application makes such constant use of the object cache? Wouldn't more memory make the disk based storage unnecessary?  

    ## Related Articles

    *   [Designing and Implementing Scalable Applications with Memcached and MySQL](http://www.mysql.com/news-and-events/on-demand-webinars/display-od-142.html) by Farhan Mashraqi from Fotolog, Monty Taylor from Sun, and Jimmy Guerrero from Sun*   [Memcached for Mysql Advanced Use Cases](http://mashraqi.com/2008/07/memcached-for-mysql-advanced-use-cases_09.html) by Farhan Mashraqi of Fotolog*   [Memcached and MySQL tutorial](http://download.tangent.org/talks/Memcached%20Study.pdf) by Brian Aker, Alan Kasindorf - Overview with examples of how a few companies use memcached. Good presentation notes by [Colin Charles](http://www.bytebot.net/blog/archives/2008/04/14/memcached-and-mysql-tutorial).*   Strategy: [Break Up the Memcache Dog Pile](http://highscalability.com/strategy-break-memcache-dog-pile)*   [Secrets to Fotolog's Scaling Success](http://highscalability.com/secrets-fotologs-scaling-success)*   [Memcached for MySQL](http://www.mysql.com/products/enterprise/memcached.html)</div>