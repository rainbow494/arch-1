## [MemSQL Architecture - The Fast (MVCC, InMem, LockFree, CodeGen) and Familiar (SQL)](/blog/2012/8/14/memsql-architecture-the-fast-mvcc-inmem-lockfree-codegen-and.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, August 14, 2012 at 9:13AM</span></div>

<div class="body">

![](http://farm9.staticflickr.com/8426/7717496094_fdcde87391_m.jpg) _This is an interview with [<span>MemSQL</span>](http://memsql.com/) <span>cofounder’s</span> [<span>Eric Frenkiel</span>](http://www.linkedin.com/in/ericfrenkiel) <span>and</span> [<span>Nikita Shamgunov</span>](http://www.linkedin.com/in/nikitashamgunov)<span>, in which they try to answer critics by going into more depth about their technology.</span>_  

<span>MemSQL ruffled a few feathers with their claim of being the fastest database in the world. According to their benchmarks MemSQL can execute 200K TPS on an EC2</span> <span>Quadruple</span> <span>Extra Large</span> <span>and on a 64 core machine they can push 1.2 million transactions a second.</span>  

<span>Benchmarks are always a dark mirror, so make of them what you will, but the target market for MemSQL is clear: projects looking for something both fast and familiar. Fast as in a novel design using a combination of technologies like</span> [<span>MVCC</span>](http://en.wikipedia.org/wiki/Multiversion_concurrency_control)<span>, code generation,</span> [<span>lock-free data structures</span>](about:blank)<span>,</span> [<span>skip lists</span>](http://en.wikipedia.org/wiki/Skip_list)<span>, and</span> [<span>in-memory execution</span>](http://highscalability.com/blog/2009/3/16/are-cloud-based-memory-architectures-the-next-big-thing.html)<span>. Familiar as in SQL and nothing but SQL. The only interface to MemSQL is SQL.</span>  

<span>It’s right to point out MemSQL gets a boost by being a first release. Only a limited subset of SQL is supported, neither replication or sharding are implemented yet, and writes queue in memory before flushing to disk. The next release will include a baseline distributed system, native replication, n-way joins, and subqueries. Maintaining performance as more features are added is a truer test.</span>  

<span>And MemSQL is RAM based, so of course it’s fast, right? Even among in-memory databases MemSQL hopes to convince you they’ve made some compelling design choices. The reasoning for their design goes something like:</span>

*   <span>Modern hardware requires a modern database. The idea is to strip everything down and rethink it all out again.</span>
*   <span>Facebook scaled for two reasons: Memcached and Code Generation. Memcached provides in-memory key-value access and</span> [<span>HipHop translates</span>](http://en.wikipedia.org/wiki/HipHop_for_PHP) <span>PHP to C++. Applying those ideas to a database you get an in-memory database that uses SQL instead of KV.</span>
*   <span>Since performance requires operating out of RAM, the first assumption is the data set fits in RAM.</span>
*   Reading fast from RAM has been solved by Memcached, which is basically a hash table sitting behind a network interface. What is not solved is the fast write problem.
*   <span>The fast write problem is solved by eliminating contention. The way to eliminate contention is by using lock-free data structures. Lock-free data structures scale well which is why MemSQL has faster writes than Memcached.</span>
*   <span>Hash tables are an obvious choice for key-value data structures, but what would you use for range queries? Skip lists are the only efficient data structures for range queries and are more scalable than</span> [<span>b-trees</span>](http://www.drdobbs.com/parallel/choose-concurrency-friendly-data-structu/208801371)<span>. Lock-free skip lists are also difficult to build.</span>
*   <span>When competing with in-memory technologies you need to execute fewer instructions for each SQL query. This goal is met via code generation. In a traditional database system there is a fixed overhead per query to set up all the contexts, prepare the tree for interpretations, and then run the query through the plan. All that is sidestepped by generating code, which becomes more of win as the number of cores and the number of queries increases.</span>
*   <span>MVCC is a good match with an in-memory databases because it offers both efficiency and transactional correctness. It also supports fast deletes. Rows can be marked deleted and then cleaned up behind the scenes.</span>

<span>On the first hearing of this strange brew of technologies you would not be odd in experiencing a little buzzword fatigue. But it all ends up working together. The mix of lock-free data structures, code generation, skip lists, and MVCC makes sense when you consider the driving forces of data living in memory and the requirement for blindingly fast execution of SQL queries.</span>  

<span>In a single machine environment MemSQL makes an excellent case for their architecture. In a distributed environment they are limited in the same way every distributed databases is limited. MVCC doesn’t offer any magic as it doesn’t translate easily across shards. The choices  MemSQL has made reflect their primary use case of fast transactions plus fast real-time SQL based analytics. MemSQL uses a sharded shared nothing approach where queries are run independently on each shard and merged together on aggregation nodes. Transactions across shards won’t be supported until two phase commit is implemented, but then they will perform like any other database.  What they really want to do well is run fast real-time aggregations across a cluster so that’s what their design reflects.</span>  

<span>A lot of other questions come to mind with such a novel design. Will MemSQL perform common operations like “return the top 5 X” as programmers have come to expect? Will MemSQL still perform when hit with a wide variety of different SQL queries? They say yes given code generation and their data structure choices. Is SQL expressive enough to solve real world problems across many domains? When you start adding stored procs or user defined functions will the carefully orchestrated dance of data structures still work?</span>  

<span>To answer these questions and more, let’s take a deeper look into the technology behind MemSQL.</span>

## <span>Stats</span>

*   <span>Largest Installation: 500 node cluster deployed on EC2 (4000 cores)</span>
*   <span>Biggest single machine deployment: 320-core SGI supercomputer; 4 TB RAM</span>
*   <span>20 billion records in a single table on a single machine</span>
*   <span>1.25m inserts/sec on a single 64-core machine</span>
*   <span>Run 25 16-core servers 24/7 for testing</span>
*   <span>15 employees, heavy on engineering</span>

## <span>Information Sources</span>

*   <span>Interview over Skype.</span>
*   <span>Email Q&A.</span>
*   <span>Everything listed under section</span> <span>Related Articles</span><span>.</span>

## <span>Use Cases</span>

*   <span>Data is proliferating and there are always green field markets that need fixing. Giving a relational interface is a good way to get real-time solved in an understandable way.</span>
*   <span>Targeted at high throughput workloads with small transactions in a heavily concurrent environment.</span>
*   <span>Users with lots of CPU and RAM who need performance. MemSQL is a complex high performance piece of software. You’ll use MemSQL if you are making money.</span>
*   <span>Answer what is happening right now questions. Most successful use case is simultaneous insert, which is only possible with a row based system, and simultaneous select, which supports real-time analytics, like min/max/distinct/ave etc.</span>
*   <span>Not in the BI market. Works well with products like Vertica. MemSQL works well with real time analytics. Data inserts transactionally yet supports a real-time analytical layer.</span>
*   <span>Relying on startups that value time over money. Build or buy? Time is the most valuable thing. Don’t need to create vertical infrastructure. Memcached is not needed which simplifies layers in the stack.</span>
*   <span>Write-heavy, read-heavy workloads</span>
    *   Machine data
    *   Traffic spikes
    *   Streaming data

## <span>Origin Story</span>

*   <span>Started in Jan 2011\. Worked in stealth mode for 14 months.</span>
*   <span>Both Eric and Nikita worked at Facebook and decided there was a better way to give SQL at scale and speed.</span>
*   <span>They quit and applied to Y Combinator with zero lines of code. Y Combinator normally expects to see a demo but bought the argument that they could build a very fast database based on their experience. Nikita  worked on the Microsoft SQL Server core engine and has other geek credentials. Eric worked on Platform at Facebook and Nikita worked on Infrastructure.</span>
*   <span>Any Facebook partner that touched the social graph immediately saw problems with scale. Games would add 2-3 million users in a matter of weeks. Media companies would have to start tracking Like buttons and comments on social deployments.</span>
*   <span>Aha moment was realizing not just Facebook had these problems. Downstream traffic from social networks and new data sources like Capital markets that may have to consume a million messages a second.</span>

## <span>Why Faster?</span>

*   <span>Lock free + code generation + MVCC is faster on multiple cores than an approach using partitioning by core and serializing data structure access per core.</span>
*   <span>Code generation minimizes code execution paths within queries and removes interpretation overhead. SQL is being hardwired into the server.</span>
*   <span>C++ is used instead of Java.</span>
*   <span>Skip lists are used instead of b-trees because b-trees don’t scale.</span>
*   <span>CPU efficiency means higher throughput. Since MemSQL uses fewer instructions per query they can achieve higher throughput. More queries can be pushed through the system because they’ve minimized parsing, caching, and plan cache matching.</span>

## The Y Combinator Cabal

*   <span>The Y Combinator network is very powerful. Through Y Combinator they were able to get a first customer way before release, which helped them prioritize features and avoid the be everything to everybody trap.</span>
*   <span>Their first customer grew to a million users in 6 weeks, put all their data in RAM, and supported just the SQL that they needed.</span>

## Lock-Free Data Structures

*   <span>Lock-free data structures scale exceptionally well as more resources (CPU, RAM) are added to a system. Lock-free data structures minimize wasted CPU during points of high contention.</span>
*   <span>Every component of the MemSQL engine is built on lock-free data structures: linked lists, queues, stacks, skip lists, and hash tables.</span>
*   <span>Lock-free queues and stacks are used throughout the system for managing state in transactions and memory managers.</span>
*   <span>The lock-free hash table is used to map query shapes to compiled plans in the plan cache.</span>
*   <span>Lock-free skip lists and hash tables are available as index data structures.</span>

## Skip Lists

*   <span>A skip list is a popular data structure that performs extremely well in-memory. It shares many fundamental properties with a randomized tree. For example, it offers O(logN) time to seek to a specific value.</span>
*   <span>The basic idea is that the bottom layer is a sorted linked list. Each higher layer is an “express” lane for the lists below. An element in layer i appears in layer i+1 with some fixed probability p (usually something like ½ or ¼).</span>
*   <span>Among the data structures that offer the efficient seek and insert properties of a tree, skip lists are notable for being implemented lock-free and perform extremely well in highly concurrent environments.</span>
*   <span>Skip lists have two main tradeoffs:</span>
    *   <span>Compared to b-trees, skip lists are slightly slower for long sequential scans.</span>
    *   <span>Lock free skip lists are unidirectional. So in MemSQL, you have to specify for each skip list index whether it should be ascending or descending. If you need both directions, you need two indexes.</span>

![](http://farm9.staticflickr.com/8303/7775050416_3dd4a291aa_n.jpg)

## MVCC

MemSQL implements multi version concurrency control to implement transactional semantics

*   <span>Every time a transaction modifies a row, MemSQL creates a new version that sits on top of the existing one. This version is only visible to the transaction that made the modification. Read queries the access the same row “see” the old version of this row.</span>
*   <span>Versions in MemSQL are implemented as a lock-free linked list.</span>
*   <span>MemSQL only takes a lock in the case of a write-write conflict on the same row. MemSQL takes a lock because it is easier to program against. The alternative would be to fail the second transaction, which would require the programmer to resolve the failure.</span>
*   <span>MemSQL implements a lock-wait timeout for deadlocks. The default value is 60 seconds. If the timeout occurs, the transaction is aborted.</span>
*   <span>MemSQL queues modified rows for the garbage collector. This lets the garbage collector clean up old versions very efficiently by avoiding a full-table scan.</span>
*   <span>Wherever possible, MemSQL can optimize single-row update queries down to simple atomic operations.</span>

## MemSQL Durability

*   <span>How it Works</span>
    *   <span>MemSQL pushes transactions to disk as fast as the disk will allow.</span>
    *   <span>Transactions first commit to an in memory buffer, and then asynchronously start writing to disk. If the size of the transaction buffer is zero, then the transaction is not acknowledged until it is committed to disk (full synchronous durability).</span>
    *   <span>A log flusher thread flushes the transactions to disk every few milliseconds. MemSQL leverages group commit to dramatically improve disk throughput.</span>
    *   <span>Once MemSQL log files reach a certain size (2 GB by default), they are compressed into snapshots. Snapshots are more compact and faster for recovery. This number is configurable as snapshot-trigger-size.</span>
    *   <span>When MemSQL is restarted, it recovers its state by reading the snapshot file (a mullti-threaded process) and then replaying the remaining log file to restore its state. Snapshot recovery is significantly faster than log recovery.</span>
    *   <span>Durability can be fully disabled for workloads that do not require it. Unless the transaction buffer fills up, this has no impact on query throughput or performance. It does not improve read performance.</span>
    *   <span>MemSQL uses checksums in its snapshot and log files to validate data consistency.</span>

*   <span>Why it's Fast</span>
    *   <span>Group commit makes MemSQL faster in highly concurrent use cases (many writers pushing a lot of data into the log). This scenario is common to customers seeking a high throughput database.</span>
    *   <span>The on-disk backup of MemSQL is extremely compact, which reduces I/O pressure.</span>
    *   <span>Without a buffer pool, writes to disk are limited to sequentially writing logs and snapshots, so MemSQL is able to efficiently take advantage of sequential I/O.</span>
    *   <span>MemSQL writes both its snapshot and log files to disk sequentially. Both hard disk and solid state drives offer significantly better performance for sequential I/O than they do for random I/O.</span>
    *   <span>MemSQL completely avoids page-swapping and can guarantee consistent high-throughput SLAs on read/write queries. No read query in MemSQL will ever wait for disk. This property is extremely important for real-time analytics scanning over terabytes of data.</span>

## Code Generation

*   <span>How it Works</span>
    *   <span>MemSQL compiles SQL queries to native code with SQL to C++ code generation. C++ is compiled with GCC and loaded into the database as a shared object.</span>
    *   <span>Compilation happens at run time. MemSQL is a just-in-time compiler for SQL. Compiled query plans are reused across server restarts so they only have to be compiled once in the lifetime of an application.</span>
    *   <span>MemSQL uses a two-phase parser. The first parser is a one-pass lightweight layer called the auto-parameterizer, which strips numbers and strings out of plans.</span>
    *   <span>For example "SELECT * FROM t WHERE id > 5 and name=’John’;" is converted to "SELECT * FROM t WHERE id > @ and name = ^;". These plans are stored in a hash table that maps parameterized queries to compiled query plans.</span>
        *   <span>If the hash table contains the plan, then the parameters are passed into the compiled code which executes the query.</span>
        *   <span>Otherwise, the query is processed by a traditional tree-based SQL parser and compiled into C++ code. The next time a query with the same shape is run, it will match the compiled plan in the hash table.</span>
    *   <span>DDL queries (CREATE/ALTER) and DML queries (SELECT/INSERT/UPDATE/DELETE) all go through code generation.</span>
    *   <span>Compiled C++ code is stored in the /plancache directory. Feel free to dive in and take a look.</span>
    *   <span>Does not support “if statements” or any procedural type logic. It’s SQL and only SQL (for now). Though they contend using the SQL base for creating generated code yields optimum performance because it removes as much interpretation as possible. Machine code generated from SQL is hardwired into the code path.</span>
    *   <span>Dynamic SQL is supported. The code generation is handled in the background by calling gcc, which is not unusual for for system that combine DSLs with dynamic linking.</span>

![](http://farm8.staticflickr.com/7132/7775075192_6e2ef1bb30_n.jpg)

*   <span>Why it's Fast</span>
    *   <span>The resulting speedup is comparable to upgrading from an interpreted language (PHP) to a compiled language (C++). This is the premise behind HipHop's PHP -> C++ compilation used at Facebook.</span>
    *   <span>The hot code path for query plans that have been compiled by the system is optimized very carefully. The hash table used to manage the plan cache is lock-free. Read queries smaller than 4kb use either pre-allocated or stack-allocated memory. Write queries allocate table memory via header-inlined slab allocators.</span>
    *   <span>malloc() is never called. Memory is allocated on process creation and managed internally from the on. This allows for complete control of garbage collection.</span>
    *   <span>Index data structures are defined in a per-table header file, which is included in every query on that table. This enables all storage engine operations to be inlined directly into the code and avoids the overhead of expensive per-row virtual function calls.</span>
    *   <span>Memory based systems can be slow depending on their design. Taking a global write lock or using memory mapped files is slow. A granular lock is taken only on a write-write conflict.</span>
    *   <span>Query compilation latency. A fresh installation of MemSQL comes with an empty plancache. Every new query shape not in plancache will be compiled with GCC. GCC compilation is still a little bit slow compared to query compilation in MySQL. Compilation takes 0.5 to 10 seconds per query per thread (depending on hardware).</span>

## Replication

*   <span>MemSQL replication is row-based, and supports master/multi-slave configuration.</span>
*   <span>Supports K-Safety. As many servers as required can be used for High Availability.</span>
*   <span>MemSQL supports online provisioning. Provisioning works by shipping and recovering from a snapshot, and then continuing to replay from the log. This process fits naturally into MemSQL’s durability scheme and is what enables online provisioning.</span>
*   <span>A slave will never encounter conflicts because order of execution is serialized in the transaction log.</span>
*   <span>MemSQL does not support master-master replication. A master-master design has a negative trade off:loss of data consistency.</span>
*   <span>MemSQL also supports synchronous replication. The tradeoff is higher write-query latency. Does not slow down reads.</span>
*   <span>Load is balanced across slaves.</span>
*   <span>When using asynchronous replication slaves are a few milliseconds behind.</span>

## The Distributed Story

*   <span>Note, this feature is not released yet,  it’s set to be released in late September.</span>
*   <span>Query Routing</span>
    *   <span>Two-tier architecture with aggregators and leaves. Leaf nodes run MemSQL and store data. Aggregators have a query planner that receives queries, breaks them into smaller queries, and routes them to one or more leaf nodes. They aggregate the results intelligently back to the user.</span>
    *   <span>Leaf nodes have a shared-nothing design. Data sharding across leaf nodes is managed by the aggregators.</span>
    *   <span>MemSQL uses standard SQL partitioning syntax to implement range and hash (key) partitioning. Range partitioning involves defining every range of data to split against, while hash partitioning takes only a shard key to hash against. MemSQL uses consistent hashing to minimize data movement in the event of a failure.</span>
    *   <span>In the event of a network partition, an aggregator communicates with the metadata node to either (a) resync changed metadata or (b) assume responsibility for remapping data ranges and update it. During this time, queries are blocked up to a tolerance timeout.</span>
    *   <span>MemSQL supports single-shard OLTP queries (routing) and complex cross-shard OLAP queries that iterate over the entire dataset.</span>
    *   <span>Because the aggregators push most of the work to the leaf nodes, MemSQL scales almost linearly with each added leaf node.</span>
    *   <span>The aggregator also uses code generation to compile the logic. For simple queries the overhead is less than .5 milliseconds.</span>
    *   <span>Leaf nodes are dumb, they do not know about other leaf nodes. MVCC only works on each leaf node, not across leaf nodes.</span>
    *   <span>There are no cross shard transactions. Updates happen independently on each shard. Two phase commit is a future feature.</span>
    *   <span>Optimize for throughput and scalability.</span>
        *   <span>By using a shared nothing architecture and by not using a two phase commit means a very high number of nodes can be supported with this approach.</span>
        *   <span>Use case for this is integration with Vertica, which requires a long load step, but can store data larger than memory. MemSQL lets you see what is happening with a system right now.</span>
        *   <span>Vertica is a column store which can do fast aggregations but can’t do fast updates, which is what MemSQL is optimized for.</span>
        *   <span>Example use case is a financial system where sharding by stocks makes sense and stats are calculated by stock.</span>
        *   <span>JDBC/ODBC can be used to sync to 3rd party systems.</span>
*   <span>Clustering</span>
    *   <span>Analytical queries can be run that touch every single node in order to produce aggregations.</span>
    *   <span>Clustering is managed by aggregator nodes communicating with an external metadata service. The metadata service can be backed by MemSQL, MySQL, or ZooKeeper.</span>
    *   <span>Each aggregator syncs and updates against the metadata service. The unified-metadata design was picked because it minimizes chatter in the system. If aggregators cannot contact the metadata node, they cannot perform DDL operations.</span>
    *   <span>Short answer about CAP theorem: MemSQL can be configured for AP (availability, partition tolerance) or CP (consistency, partition tolerance). With asynchronous replication and load-balanced reads, MemSQL can even be configured for eventual consistency.</span>
    *   <span>Long answer about the CAP theorem: As Eric Brewer (CAP inventor) points out, CAP theorem and the “pick 2 out of 3” mentality are too simplistic for analyzing a complex system (http://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed). Instead, the discussion should be about how the system “maximizes combinations of consistency and availability that make sense for the specific application.”</span>
    *   <span>Users can configure replication to match the requirements of their application:</span>
        *   <span>Replication enables high availability in the system. In the event of a network partition, MemSQL will immediately elect a new replication master to keep the system available.</span>
        *   <span>Asynchronous replication minimizes query latency for write queries. In the event of a failure, failover to replication slave and suffer partial data loss for the replication delta. MemSQL replication is powerful enough to detect and in some cases resolve divergences when the partitioned leaf node is restarted and restored to the system.</span>
        *   <span>Synchronous replication maintains strong consistency. It enables the system to be configured for k-safety (the system remains consistent and available in the presence of k failures). This option results in higher write query latencies.</span>

## Deployment and Management

*   <span>Easy new query deployment.</span>
    *   <span>Deploying new queries doesn’t require wrapping code in Java, compiling the Java, stopping the server and then deploying.</span>
    *   <span>MySQL clients send SQL statements to the server. Nothing extra is needed.</span>
    *   <span>Implication is simplicity is from SQL, adding any third party code won’t work.</span>
*   <span>How  get up and running with MemSQL:</span>
    *   <span>Download of developer edition is available directly on the website. http://www.memsql.com/#download</span>
    *   <span>Streamlined deployment on EC2\. Pre-configured AMI that accepts your license key and launches MemSQL for you on Ubuntu 12.04\. 20% of production MemSQL deployments are run this way.</span>
    *   <span>Can also run on a variety of Linux 64-bit systems by downloading a tar.gz file from download.memsql.com.</span>
*   <span>Client Libraries</span>
    *   <span>The MySQL protocol is used instead of making their client protocol. This makes it easy to integrate into existing environments.</span>
    *   <span>Very smart use of the extensive MySQL ecosystem by leveraging high performance MySQL clients instead of building their own.</span>
    *   <span>Just change your port and point to MemSQL instead of MySQL. Allows the focus to be on server development instead of client development.</span>
    *   <span>MemSQL works with any MySQL client library: ODBC, JDBC, PHP/Python/Perl/Ruby/etc, mysql c library.</span>
    *   <span>Popular manageability tools (Sequel Pro, PHPMyAdmin, MySQL Workbench), app frameworks (Ruby on Rails, Django, Symfony), and visualization tools (panopticon) work with MemSQL.</span>
*   <span>Server Management</span>
    *   <span>MemSQL controls out of memory with two knobs: maximum_memory and maximum_table_memory. maximum_memory limits the total memory use by the server and maximum_table_memory limits the amount used for table storage. If memory usage exceeds maximum_table_memory then write queries are blocked but read queries stay up. On the developer build maximum_table_memory is hardcoded to 10 GB.</span>
    *   <span>MemSQL exposes custom statistics with the SHOW STATUS, SHOW STATUS EXTENDED, and SHOW PLANCACHE commands. You can get numbers on total query compilations, query execution performance, and durability performance.</span>

## SQL Support

*   <span>Very</span> [<span>limited SQL support</span>](http://developers.memsql.com/docs/1b/unsupported.html)<span>. Just joins between two tables. No outer or full outer joins.</span>
*   <span>Does not support: views, prepared queries, stored procedures, user defined functions, triggers, foreign keys, charsets other than utf8</span>
*   <span>The only</span> [<span>supported isolation level</span>](http://developers.memsql.com/docs/1b/isolationlevel.html)<span>: READ COMMITTED</span>
*   <span>MemSQL only supports single query transactions. Every query begins and commits in its own transaction.</span>
*   <span>SQL is used:</span>
    *   <span>To reduce training costs, people (traders, business types) know SQL.</span>
    *   <span>Because they wanted to build something that was easy to use.</span>

## Pricing

*   <span>Pricing isn’t being disclosed just yet. The thought is use an hourly model so more developers can deploy on the cloud today.</span>

## Lessons Learned

*   <span>Y Combinator isn’t  just about funding, it’s a support network that helps you make much needed connections, like getting early customer wins.</span>
*   <span>Use C++ for systems-level infrastructure. It allows you to build more efficient and robust software</span>
*   <span>You should use established interfaces to drive adoption. Make it extremely easy for people to try your software</span>
*   <span>Hire people who are “ahead of the curve” in their careers and promote from within</span>
*   <span>Invest in a good code review system; we use Phabricator (Facebook’s code review system, now at phabricator.org)</span>
*   <span>Make it easy to add tests to the system and invest in hardware and software to make testing easy. Software will only become reliable from extensive testing. If you’re testing 24/7, invest in your own hardware.</span>

## Related Articles

*   <span>Paper:</span> [<span>High-Performance Concurrency Control Mechanisms for Main-Memory Databases</span>](http://arxiv.org/pdf/1201.0228v1.pdf)
*   <span>Paper:</span> [<span>A Provably Correct Scalable Concurrent Skip List</span>](http://www.cs.tau.ac.il/~shanir/nir-pubs-web/Papers/OPODIS2006-BA.pdf)
*   <span>Domas Mituzas:</span> [<span>MySQL is bazillion times faster than MemSQL</span>](http://dom.as/2012/06/26/memsql-rage/)
*   [<span>Reading Between the Benchmarks: How MemSQL Designs for Speed and Durability</span>](http://developers.memsql.com/blog/reading-between-the-benchmarks/)
*   <span>Hacker News:</span> [<span>Ex-Facebookers launch MemSQL (YC W11) to make your database fly</span>](http://news.ycombinator.com/item?id=4126007)
*   <span>Hacker News:</span> [<span>MySQL is bazillion times faster than MemSQL</span>](http://news.ycombinator.com/item?id=4162488)
*   <span>Slashdot:</span> [<span>MemSQL Makers Say They've Created the Fastest Database On the Planet</span>](http://developers.slashdot.org/story/12/06/24/2321219/memsql-makers-say-theyve-created-the-fastest-database-on-the-planet)
*   <span>Quora: SQL:</span> [<span>What benefits does MemSQL offer over running a MySQL database on ramdisk?</span>](http://www.quora.com/SQL/What-benefits-does-MemSQL-offer-over-running-a-MySQL-database-on-ramdisk/answer/Harrison-Fisk)
*   <span>Robert Scoble:</span> [<span>The @memsql guys answer Twitter questions (much faster than MySQL). at Rackspace SF</span>](http://soundcloud.com/scobleizer/the-memsql-guys-answer-twitter)
*   [<span>MemSQL Blog</span>](http://developers.memsql.com/blog/)
*   [<span>MemSQL Faq</span>](http://developers.memsql.com/docs/1c/faq.html)
*   [<span>MemSQL Twitter</span>](https://twitter.com/memsql)
*   <span>Kurt Monash:</span> [<span>Introduction to MemSQL</span>](http://www.dbms2.com/2012/06/18/introduction-to-memsql/)
*   <span>Hacker News Comment:</span> [<span>MemSQL vs VoltDB</span>](http://news.ycombinator.com/item?id=4126266)
*   <span>Bloomberg:</span> [<span>Enterprise Technology: Revenge of the Nerdiest Nerds</span>](http://www.businessweek.com/articles/2012-06-28/enterprise-technology-revenge-of-the-nerdiest-nerds)
*   [<span>VoltDB Decapitates Six SQL Urban Myths And Delivers Internet Scale OLTP In The Process</span>](http://highscalability.com/blog/2010/6/28/voltdb-decapitates-six-sql-urban-myths-and-delivers-internet.html)
*   <span>Paper:</span> [<span>Distributed Transactions for Google App Engine: Optimistic Distributed Transactions built upon Local Multi-Version Concurrency Control</span>](http://arxiv.org/abs/1106.3325)
*   <span>Paper:</span> [<span>Concurrent Programming Without Locks</span>](http://www.cl.cam.ac.uk/research/srg/netos/papers/2007-cpwl.pdf)
*   <span>Herb Sutter:</span> <span>[Choose Concurrency-Friendly Data Structures](http://www.drdobbs.com/parallel/choose-concurrency-friendly-data-structu/208801371)</span>

</div>