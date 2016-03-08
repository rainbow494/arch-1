## [What the heck are you actually using NoSQL for?](/blog/2010/12/6/what-the-heck-are-you-actually-using-nosql-for.html)
## [使用NoSQL时会踩哪些坑?](/blog/2010/12/6/what-the-heck-are-you-actually-using-nosql-for.html)


![](http://farm5.static.flickr.com/4127/5188198566_3fe006d562_m.jpg)

It's a truism that we should choose the _right tool for the job_. Everyone says that. And who can disagree? The problem is this is not helpful advice without being able to answer more specific questions like: What jobs are the tools good at? Will they work on jobs like mine? Is it worth the risk to try something new when all my people know something else and we have a deadline to meet? How can I make all the tools work together?

众所周知，我们应该应该__为任务选择合适的工具__。每个人都这么说，谁说不是呢？但问题是就算知道这个道理，它也无法回到我们：对这个任务，是最合适的锤子是哪一把？别人用的趁手的锤子我也用得惯么？面对不断迫近的截止日，是不是应该冒风险去找把新锤子？特别是在小伙伴们也都没摸过这把锤子的时候？还有就是，我如何利用这把新锤子和我的老锯子，老斧子一起配合来完成任务？

In the NoSQL space this kind of real-world data is still a bit vague. When asked, vendors tend to give very general answers like NoSQL is good for BigData or key-value access. What does that mean for for the developer in the trenches faced with the task of solving a specific problem and there are a dozen confusing choices and no obvious winner? Not a lot. It's often hard to take that next step and imagine how their specific problems could be solved in a way that's worth taking the trouble and risk.

在NoSQL面前，真实事件的数据（类型、结构、模式）似乎不那么清晰可辨。每当被问起NoSQL是什么，方案服务商们总是倾向于给出一个抽象的答案：NoSQL对于大数据和键值类型的数据存取非常合适。但是，这回答到底是几个意思？因为对身处前线的开发者来说，他们有一堆问题要解决，商人们给的答案并不能在开发人员在遇到选择障碍时帮助他们指明道路。即便选择不多，这些回答也很难帮助开发者找出值得冒险（遇到新问题）一试的方案。

Let's change that. What problems are you using NoSQL to solve? Which product are you using? How is it helping you? Yes, this is part the research for my [webinar on December 14th](https://www1.gotomeeting.com/register/154378825), but I'm a huge believer that people learn best by example, so if we can come up with real specific examples I think that will really help people visualize how they can make the best use of all these new product choices in their own systems.

你用NoSQL来解决什么问题？你用哪款NoSQL数据库？它如何帮助你？上面几个问题是我12月14号在线会议讨论的部分内容[详见](https://www1.gotomeeting.com/register/154378825)，会议纪要或许对你有帮助，但我更相信眼见为实这个道理，如果开发一些看得见的代码实例，那会对开发人员如何为系统选择合适的产品更有帮助

Here's a list of uses cases I came up with after some trolling of the interwebs. The sources are so varied I can't attribute every one, I'll put a list at the end of the post. Please feel free to add your own. I separated the use cases out for a few specific products simply because I had a lot of uses cases for them they were clearer out on their own. This is not meant as an endorsement of any sort. Here's a master list of all the [NoSQL products](http://nosql-database.org/). If you would like to provide a specific set of use cases for a product I'd be more than happy to add that in.

下面的这些用例是我在interwebs上一直强调的东西。抱歉我不能指出每个案例的具体来源（因为来源太多），但在文章结尾我会有一个文章列表。请随意使用它们。我只是简单的把用例从产品中独立出来，因为我有许多关于这些问题的用例，而且它们一眼就能看懂。当然，这些用例出现的先后次序和产品间的好坏没有关系。下面是一份主流[NoSQL产品列表](http://nosql-database.org/)。当然，这里也欢迎大家提供关于某个产品特殊功能的用例

## **General Use Cases**
## **基本用例**

These are the general kinds of reasons people throw around for using NoSQL. Probably nothing all that surprising here.
下面这些都是常见的使用NoSQL的理由。

*   **Bigness** NoSQL is seen as a key part of a new data stack supporting: big data, big numbers of users, big numbers of computers, big supply chains, big science, and so on. When something becomes so massive that it must become massively distributed, NoSQL is there, though not all NoSQL systems are targeting big. Bigness can be across many different dimensions, not just using a lot of disk space. 

*   **大** NoSQL常被当作支持新数据存放的主要部分：大数据，大量用户，大量计算机，大供应链，大量科学运算，等等。当某个系统成长后，它很可能衍变为大型分布架构，这种情况一般就会用上NoSQL，虽然不是所有的NoSQL系统的设计目标都是大型系统。而大这个概念也可以涵盖很多方面，不仅仅是指硬盘空间。

*   **Massive write performance**. This is probably the canonical usage based on Google's influence. High volume. Facebook needs to store [135 billion messages a month](http://highscalability.com/blog/2010/11/16/facebooks-new-real-time-messaging-system-hbase-to-store-135.html). Twitter, for example, has the problem of storing [7 TB/data per day](http://www.slideshare.net/kevinweil/nosql-at-twitter-nosql-eu-2010) with the prospect of this requirement doubling multiple times per year. This is the data is too big to fit on one node problem. At 80 MB/s it takes a day to store 7TB so writes need to be distributed over a cluster, which implies key-value access, MapReduce, replication, fault tolerance, consistency issues, and all the rest. For faster writes in-memory systems can be used.

*   

*   **Fast key-value access**. This is probably the second most cited virtue of NoSQL in the general mind set.  When latency is important it's hard to beat hashing on a key and reading the value directly from memory or in as little as one disk seek. Not every NoSQL product is about fast access, some are more about reliability, for example. but what people have wanted for a long time was a better memcached and many NoSQL systems offer that.

*   

*   **Flexible schema** and **flexible datatypes**.  NoSQL products support a whole range of new data types, and this is a major area of innovation in NoSQL. We have: column-oriented, graph, advanced data structures, document-oriented, and key-value. Complex objects can be easily stored without a lot of mapping. Developers love avoiding complex schemas and ORM frameworks. Lack of structure allows for much more flexibility. We also have program and programmer friendly compatible datatypes likes JSON. 
*   **Schema migration**. Schemalessness makes it easier to deal with schema migrations without so much worrying. Schemas are in a sense dynamic, because they are imposed by the application at run-time, so different parts of an application can have a different view of the schema.
*   **Write availability**. Do your writes need to succeed no mater what? Then we can get into partitioning, CAP, eventual consistency and all that jazz.
*   **Easier maintainability**, **administration and operations**. This is very product specific, but many NoSQL vendors are trying to gain adoption by making it easy for developers to adopt them. They are spending a lot of effort on ease of use, minimal administration, and automated operations. This can lead to lower operations costs as special code doesn't have to be written to scale a system that was never intended to be used that way.
*   **No single point of failure**. Not every product is delivering on this, but we are seeing a definite convergence on relatively easy to configure and manage high availability with automatic load balancing and cluster sizing. A perfect cloud partner.
*   **Generally available parallel computing.** We are seeing MapReduce baked into products, which makes parallel computing something that will be a normal part of development in the future.
*   **Programmer ease of use**. Accessing your data should be easy. While the relational model is intuitive for end users, like accountants, it's not very intuitive for developers. Programmers grok keys, values, JSON, Javascript stored procedures, HTTP, and so on. NoSQL is for programmers. This is a developer led coup. The response to a database problem can't always be to hire a really knowledgeable DBA, get your schema right, denormalize a little, etc., programmers would prefer a system that they can make work for themselves. It shouldn't be so hard to make a product perform. Money is part of the issue. If it costs a lot to scale a product then won't you go with the cheaper product, that you control, that's easier to use, and that's easier to scale?
*   **Use the right data model for the right problem**. Different data models are used to solve different problems. Much effort has been put into, for example, wedging graph operations into a relational model, but it doesn't work. Isn't it better to solve a graph problem in a graph database? We are now seeing a general strategy of trying find the best fit between a problem and solution.
*   **[Avoid hitting the wall](http://blog.hypertable.com/?p=79)**. Many projects hit some type of wall in their project. They've exhausted all options to make their system scale or perform properly and are wondering what next? It's comforting to select a product and an approach that can jump over the wall by linearly scaling using incrementally added resources.  At one time this wasn't possible. It took custom built everything, but that's changed. We are now seeing usable out-of-the-box products that a project can readily adopt.
*   [Distributed systems support](http://s3.amazonaws.com/cimlabs/Oredev-Enterprise-NoSQL.pdf). Not everyone is worried about scale or performance over and above that which can be achieved by non-NoSQL systems. What they need is a distributed system that can span datacenters while handling failure scenarios without a hiccup. NoSQL systems, because they have focussed on scale, tend to exploit partitions, tend not use heavy strict consistency protocols, and so are well positioned to operate in distributed scenarios.
*   [Tunable CAP tradeoffs](http://dbmsmusings.blogspot.com/2010/04/problems-with-cap-and-yahoos-little.html). NoSQL systems are generally the only products with a "slider" for choosing where they want to land on the CAP spectrum. Relational databases pick strong consistency which means they can't tolerate a partition failure. In the end this is a business decision and should be decided on a case by case basis. Does your app even care about consistency? Are a few drops OK? Does your app need strong or weak consistency? Is availability more important or is consistency? Will being down be more costly than being wrong? It's nice to have products that give you a choice.

## More Specific Use Cases

*   Managing large streams of non-transactional data: Apache logs, application logs, MySQL logs, clickstreams, etc.
*   Syncing online and offline data. This is a niche [CouchDB has targeted](http://www.readwriteweb.com/enterprise/2010/07/nosql-database-couchdb.php). 
*   Fast response times under all loads.
*   Avoiding heavy joins for when the query load for complex joins become too large for a RDBMS.
*   Soft real-time systems where low latency is critical. Games are one example.
*   Applications where a wide variety of different write, read, query, and consistency patterns need to be supported. There are systems optimized for 50% reads 50% writes, 95% writes, or 95% reads. Read-only applications needing extreme speed and resiliency, simple queries, and can tolerate slightly stale data. Applications requiring moderate performance, read/write access, simple queries, completely authoritative data. Read-only application which complex query requirements.
*   Load balance to accommodate data and usage concentrations and to help keep microprocessors busy.
*   Real-time inserts, updates, and queries.
*   Hierarchical data like threaded discussions and parts explosion.
*   Dynamic table creation.
*   Two tier applications where low latency data is made available through a fast NoSQL interface, but the data itself can be calculated and updated by high latency Hadoop apps or other low priority apps.
*   Sequential data reading. The right underlying data storage model needs to be selected. A B-tree may not be the best model for sequential reads.
*           Slicing off part of service that may need better performance/scalability onto it's own system. For example, user logins may need to be high performance and this feature could use a dedicated service to meet those goals.        
*   Caching. A  high performance caching tier for web sites and other applications. Example is a cache for the Data Aggregation System used by the Large Hadron Collider.
*   Voting.
*   Real-time page view counters.
*   User registration, profile, and session data.
*   Document, catalog management  and content management systems. These are facilitated by the ability to store complex documents has a whole rather than organized as relational tables. Similar logic applies to inventory, shopping carts, and other structured data types.
*   Archiving. Storing a large continual stream of data that is still accessible on-line. Document-oriented databases with a flexible schema that can handle schema changes over time.
*   Analytics. Use MapReduce, Hive, or Pig to perform analytical queries and scale-out systems that support high write loads.
*   Working with [heterogenous types of data](http://brehaut.net/blog/2010/couch_impedance#), for example, different media types at a generic level.
*   Embedded systems. They don’t want the overhead of SQL and servers, so they uses something simpler for storage.
*   A "market" game, where you own buildings in a town. You want the building list of someone to pop up quickly, so you partition on the owner column of the building table, so that the select is single-partitioned. But when someone buys the building of someone else you update the owner column along with price.
*  [JPL](http://qconsf.com/sf2010/presentation/Out+of+This+World+Cloud+Computing) is using SimpleDB to store rover plan attributes. References are kept to a full plan blob in S3.             
*  Federal law enforcement agencies [tracking Americans in real-time](http://www.wired.com/threatlevel/2010/12/realtime/) using credit cards, loyalty cards and travel reservations.            
*  [Fraud detection](http://www.slideshare.net/SparsityTechnologies/dex-introduction#) by comparing transactions to known patterns in real-time.            
*  [Helping diagnose](http://www.slideshare.net/SparsityTechnologies/dex-introduction#) the typology of tumors by integrating the history of every patient.            
*  In-memory database for high update situations, like a [web site](http://news.ycombinator.com/item?id=16430) that displays everyone's "last active" time (for chat maybe). If users are performing some activity once every 30 sec, then you will be pretty much be at your limit with about 5000 simultaneous users.
*  Handling lower-frequency multi-partition queries using materialized views while continuing to process high-frequency streaming data.
*  Priority queues.
*  Running calculations on cached data, using a program friendly interface, without have to go through an ORM.
*  [Unique a large dataset](http://wiki.apache.org/cassandra/UseCases) using simple key-value columns.
*  To keep querying fast, values can be rolled-up into [different time slices](https://www.cloudkick.com/blog/2010/mar/02/4_months_with_cassandra/).
*  [Computing the intersection](http://about.digg.com/blog/looking-future-cassandra) of two massive sets, where a join would be too slow.
*  A [timeline ala Twitter](http://highscalability.com/scaling-twitter-making-twitter-10000-percent-faster). 

## Redis Use Cases

Redis is unique in the repertoire as it is a data structure server, with many fascinating use cases that [people are excited to share](http://simonwillison.net/static/2010/redis-tutorial/). 

*  Calculating [whose friends are online](http://www.lukemelia.com/blog/archives/2010/01/17/redis-in-practice-whos-online/) using sets. 
*  Memcached on steroids.
*  Distributed lock manager for process coordination.
*  Full text inverted index lookups.
*  Tag clouds.
*  Leaderboards. Sorted sets for maintaining high score tables.
*  Circular log buffers.
*  Database for university course availability information. If the set contains the course ID it has an open seat. Data is scraped and processed continuously and there are ~7200 courses.
*  Server for backed sessions. A random cookie value which is then associated with a larger chunk of serialized data on the server) are a very poor fit for relational databases. They are often created for every visitor, even those who stumble in from Google and then leave, never to return again. They then hang around for weeks taking up valuable database space. They are never queried by anything other than their primary key.
*  Fast, atomically incremented counters are a great fit for offering real-time statistics.
*  Polling the database every few seconds. Cheap in a key-value store. If you're sharding your data you'll need a central lookup service for quickly determining which shard is being used for a specific user's data. A replicated Redis cluster is a great solution here - GitHub use exactly that to manage sharding their many repositories between different backend file servers.
*   Transient data. Any transient data used by your application is also a good fit for Redis. [CSRF tokens](http://en.wikipedia.org/wiki/Cross-site_request_forgery#Prevention) (to prove a POST submission came from a form you served up, and not a form on a malicious third party site, need to be stored for a short while, as does handshake data for various security protocols. 
*   Incredibly easy to set up and ridiculously fast (30,000 read or writes a second on a laptop with the default configuration)
*   Share state between processes. Run a long running batch job in one Python interpreter (say loading a few million lines of CSV in to a Redis key/value lookup table) and run another interpreter to play with the data that’s already been collected, even as the first process is streaming data in. You can quit and restart my interpreters without losing any data. 
*   Create [heat maps of the BNP’s membership list](http://www.guardian.co.uk/news/datablog/2009/oct/19/bnp-membership-list-constituency "BNP membership where you live") for the Guardian
*   Redis semantics map closely to Python native data types, you don’t have to think for more than a few seconds about how to represent data.
*   That’s a simple capped log implementation (similar to a MongoDB capped collection)—push items on to the tail of a ’log’ key and use ltrim to only retain the last X items. You could use this to keep track of what a system is doing right now without having to worry about storing ever increasing amounts of logging information.
*   An interesting example of an application built on Redis is [Hurl](http://hurl.it/), a tool for debugging HTTP requests built in 48 hours by Leah Culver and Chris Wanstrath. 
*   It’s common to use MySQL as the backend for storing and retrieving what are essentially key/value pairs. I’ve seen this over-and-over when someone needs to maintain a bit of state, session data, counters, small lists, and so on. When MySQL isn’t able to keep up with the volume, we often turn to memcached as a write-thru cache. But there’s a bit of a mis-match at work here. 
*   With sets, we can also keep track of ALL of the IDs that have been used for records in the system.
*   Quickly pick a random item from a set. 
*   API limiting. This is a great fit for Redis as a rate limiting check needs to be made for every single API hit, which involves both reading and writing short-lived data.  
*   A/B testing is another perfect task for Redis - it involves tracking user behaviour in real-time, making writes for every navigation action a user takes, storing short-lived persistent state and picking random items.
*   Implementing the inbox method with Redis is simple: each user gets a queue (a capped queue if you're worried about memory running out) to work as their inbox and a set to keep track of the other users who are following them. Ashton Kutcher has over 5,000,000 followers on Twitter - at 100,000 writes a second it would take less than a minute to fan a message out to all of those inboxes.
*   Publish/subscribe is perfect for this broadcast updates (such as election results) to hundreds of thousands of simultaneously connected users. Blocking queue primitives mean message queues without polling.
*   Have workers periodically report their load average in to a sorted set.
*   Redistribute load. When you want to issue a job, grab the three least loaded workers from the sorted set and pick one of them at random (to avoid the thundering herd problem).
*   Multiple GIS indexes. 
*   Recommendation engine based on relationships.
*   Web-of-things data flows.
*   Social graph representation. 
*   Dynamic schemas so schemas don't have to be designed up-front. Building the data model in code, on the fly by adding properties and relationships, dramatically simplifies code. 
*   Reducing the impedance mismatch because the data model in the database can more closely match the data model in the application.

## VoltDB Use Cases

VoltDB as a relational database is not traditionally thought of as in the NoSQL camp, but I feel based on their [radical design perspective](http://highscalability.com/blog/2010/6/28/voltdb-decapitates-six-sql-urban-myths-and-delivers-internet.html) they are so far away from Oracle type systems that they are much more in the NoSQL tradition.

*   Application: Financial trade monitoring
    1.  Data source: Real-time markets
    2.  Partition key: Market symbol (ticker, CUSIP, SEDOL, etc.)
    3.  High-frequency operations: Write and index all trades, store tick data (bid/ask)
    4.  Lower-frequency operations: Find trade order detail based on any of 20+ criteria, show TraderX's positions across all market symbols

*   Application: Web bot vulnerability scanning (SaaS application)
    1.  Data source: Inbound HTTP requests
    2.  Partition key: Customer URL
    3.  High-frequency operations: Hit logging, analysis and alerting
    4.  Lower-frequency operations: Vulnerability detection, customer reporting

*   Application: Online gaming leaderboard 
    1.  Data source: Online game 
    2.  Partition key: Game ID 
    3.  High-frequency operations: Rank scores based on defined intervals and player personal best
    4.  Lower-frequency transactions: Leaderboard lookups

*   Application: Package tracking (logistics)
    1.  Data source: Sensor scan
    2.  Partition key: Shipment ID
    3.  High-frequency operations: Package location updates
    4.  Lower-frequency operations: Package status report (including history), lost package tracking, shipment rerouting

*   Application: Ad content serving
    1.  Data source: Website or device, user or rule triggered
    2.  Partition key: Vendor/ad ID composite
    3.  High-frequency operations: Check vendor balance, serve ad (in target device format), update vendor balance
    4.  Lower-frequency operations: Report live ad view and click-thru stats by device (vendor-initiated)

*   Application: Telephone exchange call detail record (CDR) management
    1.  Data source: Call initiation request
    2.  Partition key: Caller ID
    3.  High-frequency operations: Real-time authorization (based on plan and balance)
    4.  Lower-frequency operations: Fraud analysis/detection

*   Application: Airline reservation/ticketing
    1.  Data source: Customers (web) and airline (web and internal systems)
    2.  Partition key: Customer (flight info is replicated)
    3.  High-frequency operations: Seat selection (lease system), add/drop seats, baggage check-in
    4.  Lower-frequency operations: Seat availability/flight, flight schedule changes, passenger re-bookings on flight cancellations

## Analytics Use Cases

Kevin Weil at Twitter is great at providing Hadoop use cases. At Twitter this includes counting big data with standard counts, min, max, std dev; correlating big data with probabilities, covariance, influence; and research on Big data. Hadoop is on the fringe of NoSQL, but it's very useful to see what kind of problems are being solved with it.

*   How many request do we serve each day?
*   What is the average latency? 95% latency?
*   Grouped by response code: what is the hourly distribution?
*   How many searches happen each day at Twitter?
*   Where do they come from?
*   How many unique queries?
*   How many unique users?
*   Geographic distribution?
*   How does usage differ for mobile users?
*   How does usage differ for 3rd party desktop client users?
*   Cohort analysis: all users who signed up on the same day—then see how they differ over time.
*   Site problems: what goes wrong at the same time?
*   Which features get users hooked?
*   Which features do successful users use often?
*   Search corrections and suggestions (not done now at Twitter, but coming in the feature).
*   What can web tell about a user from their tweets?
*   What can we tell about you from the tweets of those you follow?
*   What can we tell about you from the tweets of your followers?
*   What can we tell about you from the ratio of your followers/following?
*   What graph structures lead to successful networks? (Twitter’s graph structure is interesting since it’s not two-way)
*   What features get a tweet retweeted?
*   When a tweet is retweeted, how deep is the corresponding retweet three?
*   Long-term duplicate detection (short term for abuse and stopping spammers)
*   Machine learning. About not quite knowing the right questions to ask at first. How do we cluster users?
*   Language detection (contact mobile providers to get SMS deals for users—focusing on the most popular countries at first).
*   How can we detect bots and other non-human tweeters?

## Poor Use Cases

*   **OLTP**. Outside VoltDB, complex multi-object transactions are generally not supported. Programmers are supposed to denormalize, use documents, or use other complex strategies like compensating transactions.
*   **Data integrity**. Most of the NoSQL systems rely on applications to enforce data integrity where SQL uses a declarative approach. Relational databases are still the winner for data integrity.
*   **Data independence**.  Data outlasts applications. In NoSQL applications drive everything about the data. One argument for the relational model is as a repository of facts that can last for the entire lifetime of the enterprise, far past the expected life-time of any individual application.
*   **SQL**. If you require SQL then very few NoSQL system will provide a SQL interface, but more systems are starting to provide SQLish interfaces.
*   **Ad-hoc queries**. If you need to answer real-time questions about your data that you can’t predict in advance, relational databases are generally still the winner. 
*   **Complex relationships**. Some NoSQL systems support relationships, but a relational database is still the winner at relating.
*   **Maturity and stability**. Relational databases still have the edge here. People are familiar with how they work, what they can do, and have confidence in their reliability. There are also more programmers and toolsets available for relational databases. So when in doubt, this is the road that will be traveled.

## Related Articles

*   [Hacker News](http://news.ycombinator.com/item?id=1976429) and [Reddit](http://www.reddit.com/r/programming/comments/ehaeq/high_scalability_what_the_heck_are_you_actually/) thread on this Post. More use case suggestions there. 
*   [List of NoSQL Systems](http://nosql-database.org/)
*   [Pig](http://research.yahoo.com/node/90) - infrastructure to support ad-hoc analysis of very large data sets.
*   [Digging Deeper Into Data With Hadoop](http://gigaom.com/2009/06/07/digging-deeper-into-data-with-hadoop/) By Gary Orenstein 
*   [NoSQL East 2009 - Summary of Day 1](http://journal.uggedal.com/nosql-east-2009---summary-of-day-1) by Eivind Uggedal
*   [The “NoSQL” approach: struggling to see the benefits](http://nsaunders.wordpress.com/2010/08/05/the-nosql-approach-struggling-to-see-the-benefits/) by Neil Saunders 
*   [Design Patterns for DistributedNon-Relational Databases](http://static.last.fm/johan/nosql-20090611/intro_nosql.pdf ) by Todd Lipcon.
*   [The Future Is Big Data in the Cloud](http://gigaom.com/2009/10/25/the-future-is-big-data-in-the-cloud/) By Ping Li
*   [One size does not fit all: “document stores”, “nosql databases” , ODBMSs](http://www.odbms.org/blog/2010/02/document-stores-nosql-databases-odbmss/) by Roberto V. Zicari
*   [NoSQL is for niches](http://www.zdnet.com/blog/open-source/nosql-is-for-niches/6128) By Dana Blankenhorn.
*   [MongoDB Use Cases](http://www.mongodb.org/display/DOCS/Use+Cases)
*   [MongoDB and Ecommerce](http://kylebanker.com/blog/2010/04/30/mongodb-and-ecommerce/) by  Kyle Banker
*   [Archiving - a good MongoDB use case?](http://blog.mongodb.org/post/1200539426/archiving-a-good-mongodb-use-case)
*   [Five Reasons to Use NoSQL](http://facility9.com/2010/09/16/five-reasons-to-use-nosql) by Jeremiah Peschka
*   [The Business Case for NoSQL, NoETL and NoProblems](http://www.itbusinessedge.com/cm/blogs/lawson/the-business-case-for-nosql-noetl-and-noproblems/?cs=40284) by Loraine Lawson
*   [Is It Time For NoETL](http://intelligent-enterprise.informationweek.com/blog/archives/2010/03/is_it_time_for.html;jsessionid=3YOYNBBPASI5VQE1GHRSKH4ATMY32JVN)? by Seth Grimes
*   [Holy Large Hadron Collider, Batman!](http://blog.mongodb.org/post/660037122/holy-large-hadron-collider-batman)
*   [I Can't Wait for NoSQL to Die](http://teddziuba.com/2010/03/i-cant-wait-for-nosql-to-die.html) by Ted Dziuba.
*   [NoSQL Basics, Benefits and Best-Fit Scenarios](http://intelligent-enterprise.informationweek.com/channels/information_management/showArticle.jhtml;jsessionid=ZLGYK42ZWIZSXQE1GHOSKH4ATMY32JVN?articleID=227701021&pgno=2#) by Curt Monash 
*   [Redis Tutorial](http://simonwillison.net/static/2010/redis-tutorial/) by Simon Willison
*   [Why I Like Redis](http://simonwillison.net/2009/Oct/22/redis/ ) by Simon Willison
*   [Redis: Lightweight key/value Store That Goes the Extra Mile](http://www.linux-mag.com/id/7496) by Jeremy Zawodny
*   [Remote Dictionary Server](http://www.slideshare.net/ezmobius/redis-remote-dictionary-server)
*   [Is NoSQL for me? I’m just a small fish](http://devlicio.us/blogs/hadi_hariri/archive/2010/11/24/is-nosql-for-me-i-m-just-a-small-fish.aspx) by Hadi Hariri
*   [Why Big Enterprises are Interested in NoSQL](http://s3.amazonaws.com/cimlabs/Oredev-Enterprise-NoSQL.pdf) by Jon Moore
*   [Visual Guide to NoSQL Systems](http://blog.nahurst.com/visual-guide-to-nosql-systems) by Nathan Hurst
*   [You Can't Sacrifice Partition Tolerance](http://codahale.com/you-cant-sacrifice-partition-tolerance/) by Coda Hale
*   [NoSQL Netflix Use Case Comparison](http://perfcap.blogspot.com/) by Adrian Cockcroft
*   [Comparison guide of horizontally scalable datastores](http://cattell.net/datastores/Datastores.pdf) by Rick Cattell
*   [Weak Consistency and CAP Implications](http://www.igvita.com/2010/06/24/weak-consistency-and-cap-implications/) by Ilya Grigorik 
*   [Schema-Free MySQL vs NoSQL](http://www.igvita.com/2010/03/01/schema-free-mysql-vs-nosql/)  by Ilya Grigorik 
*   [Neo4j - 5 Cool Graph Examples](http://www.slideshare.net/peterneubauer/neo4j-5-cool-graph-examples-4473985)
*   [Has anyone used Graph-based Databases](http://stackoverflow.com/questions/1000162/have-abyone-used-graph-based-databases-http-neo4j-org)
*   [Neotechnology Uses Cases](http://neotechnology.com/customers)
*   [CouchOne Ataxo Case Study](http://www.couchone.com/case-study-ataxo)
*   [NOSQL: scaling to size and scaling to complexity](http://blogs.neotechnology.com/emil/2009/11/nosql-scaling-to-size-and-scaling-to-complexity.html) by Emil Eifrem
*   [NoSQL, NoProblem (Not Really… but it’s still awesome)](http://jeremypinkham.com/post/1587967351/nosql-noproblem-not-really-but-its-still-awesome#) by Jeremy Pinkham
*   [An Expert's Guide to Oracle Technology](http://it.toolbox.com/blogs/oracle-guide/acid-vs-base-25938) by Lewis Cunningham
*   N[oSQL vs SQL, Why Not Both?](http://www.cloudbook.net/resources/stories/nosql-vs-sql-why-not-both) By Alaric Snell-Pym 
*   [Databases: relational vs object vs graph vs document](http://www.cbsolution.net/roller/ontarget/entry/databases_relational_vs_object_vs) by On Target 
*   [Use cases are driving the divergence, and the convergence, of NoSQL solutions](http://blog.membase.com/use-cases-are-driving-divergence-and-convergence-nosql-solutions) by James  Phillips
*   [The beginning of the end of NoSQL](http://blogs.the451group.com/information_management/2010/11/12/the-beginning-of-the-end-of-nosql/) by Matthew Aslett 
*   [Going NoSQL with MongoDB](http://msdn.microsoft.com/en-us/magazine/ee310029.aspx) by Ted Neward
*   [NoSQL Ecosystem](http://www.rackspacecloud.com/blog/2009/11/09/nosql-ecosystem/) by Jonathan Ellis
*   [The New Dimension of NoSQL Scalability: Complexity](http://nosql.mypopescu.com/post/287581423/the-new-dimension-of-nosql-scalability-complexity) by Alex Popescu
*   [Quick Reference to Alternative data storages](http://themindstorms.blogspot.com/2009/05/quick-reference-to-alternative-data.html) by Alex Popescu
*   [6 Reasons Why Relational Database Will Be Superseded](http://www.havemacwillblog.com/2008/11/6-reasons-why-relational-database-will-be-superseded/)  by Robin Bloor
*   [NoSQL Misconceptions](http://www.viget.com/extend/nosql-misconceptions/) by Ben Scofield
*   [SQL Databases Don't Scale](http://adam.heroku.com/past/2009/7/6/sql_databases_dont_scale/) by Adam Wiggins 
*   [“One Size Fits All”: An Idea Whose Time Has Come and Gone](http://www.cs.brown.edu/~ugur/fits_all.pdf) by Michael Stonebraker and Uğur Çetintemel
*   [Future of RDBMS is RAM Clouds & SSD](http://www.igvita.com/2009/12/07/future-of-rdbms-is-ram-clouds-ssd/) by Ilya Grigorik
*       [To scale or not to scale: Key/Value, Document, SQL, JPA](http://www.slideshare.net/uri1803/to-scale-or-not-to-scale-keyvalue-document-sql-jpa-whats-right-for-my-app#)  by Uri Cohen    
