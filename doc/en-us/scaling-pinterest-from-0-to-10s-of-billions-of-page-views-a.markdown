## [Scaling Pinterest - From 0 to 10s of Billions of Page Views a Month in Two Years](/blog/2013/4/15/scaling-pinterest-from-0-to-10s-of-billions-of-page-views-a.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, April 15, 2013 at 9:25AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7180/6886606039_bc5c70dbf9_m.jpg)

<span>Pinterest has been riding an exponential growth curve, doubling every month and half. They’ve gone from 0 to 10s of billions of page views a month in two years, from 2 founders and one engineer to over 40 engineers, from one little MySQL server to 180 Web Engines, 240 API Engines, 88 MySQL DBs (cc2.8xlarge) + 1 slave each, 110 Redis Instances, and 200 Memcache Instances.</span>

<span>Stunning growth. So what’s Pinterest's story? To tell their story we have our bards, Pinterest’s</span> [<span>Yashwanth Nelapati</span>](http://www.linkedin.com/in/yashh) <span>and</span> [<span>Marty Weiner</span>](http://pinterest.com/martaaay/)<span>, who tell the dramatic story of Pinterest’s architecture evolution in a talk titled</span> [<span>Scaling Pinterest</span>](http://www.infoq.com/presentations/Pinterest)<span>. This is the talk they would have liked to hear a year and half ago when they were scaling fast and there were a lot of options to choose from. And they made a lot of incorrect choices.</span>

<span>This is a great talk. It’s full of amazing details. It’s also very practical, down to earth, and it contains strategies adoptable by nearly anyone. Highly recommended.</span>

<span>Two of my favorite lessons from the talk:</span>

1.  <span>Architecture is doing the right thing when growth can be handled by adding more of the same stuff. You want to be able to scale by throwing money at a problem which means throwing more boxes at a problem as you need them. If your architecture can do that, then you’re golden.</span>
2.  <span>When you push something to the limit all technologies fail in their own special way. This lead them to evaluate tool choices with a preference for tools that are: mature; really good and simple; well known and liked; well supported; consistently good performers; failure free as possible; free. Using these criteria they selected: MySQL, Solr, Memcache, and Redis. Cassandra and Mongo were dropped.</span>

<span>These two lessons are interrelated. Tools following the principles in (2) can scale by adding more boxes. And as load increases mature products should have fewer problems. When you do hit problems you’ll at least have a community to help fix them.  It’s when your tools are too tricky and too finicky that you hit walls so high you can’t climb over.</span>

<span>It’s in what I think is the best part of the entire talk, the discussion of why sharding is better than clustering, that you see the themes of growing by adding resources, few failure modes, mature, simple, and good support, come into full fruition. Notice all the tools they chose grow by adding shards, not through clustering. The discussion of why they prefer sharding and how they shard is truly interesting and will probably cover ground you’ve never considered before.</span>

<span>Now, let’s see how Pinterest scales:</span>

## <span>Basics</span>

*   <span>Pins are an image with a collection of other bits of information, a description of what makes it important to the user, and link back to where they found it.</span>

*   <span>Pinterest is a social network. You can follow people and boards.</span>

*   <span>Database: They have users who have boards and boards have pins; follow and repin relationships; authentication information.</span>

## <span>Launched in March 2010 - The Age of Finding Yourself</span>

<span>At this point you don’t even know what product you are going to build. You have ideas, so you are iterating and changing things quickly. So you end up with a lot of strange little MySQL queries you would never do in real life.</span>

<span>The numbers at this early date:</span>

*   <span>2 founders</span>

*   <span>1 engineer</span>

*   <span>Rackspace</span>

*   <span>1 small web engine</span>

*   <span>1 small MySQL DB</span>

## <span>January 2011</span>

<span>Still in stealth mode and the product is evolving from user feedback. </span>The numbers:

*   <span>Amazon EC2 + S3 + CloudFront</span>

*   <span>1 NGinX, 4 Web Engines (for redundancy, not really for load)</span>

*   <span>1 MySQL DB + 1 Read Slave (in case master goes down)</span>

*   <span>1 Task Queue + 2 Task Processors</span>

*   <span>1 MongoDB (for counters)</span>

*   <span>2 Engineers</span>

## <span>Through Sept 2011 - The Age of Experimentation</span>

Went on a crazy run where they were doubling every month and half. Insane growth.

*   <span>When you are growing that fast everything breaks every night and every week.</span>

*   At this point you read a lot of white papers that say just ad a box and you're done. So they start adding a lot of technology. They all break.

*   As a result you end up with a very complicated picture:

    *   <span>Amazon EC2 + S3 + CloudFront</span>

    *   <span>2NGinX, 16 Web Engines + 2 API Engines</span>

    *   <span>5 Functionally sharded MySQL DB + 9 read slaves</span>

    *   <span>4 Cassandra Nodes</span>

    *   <span>15 Membase Nodes (3 separate clusters)</span>

    *   <span>8 Memcache Nodes</span>

    *   <span>10 Redis Nodes</span>

    *   <span>3 Task Routers + 4 Task Processors</span>

    *   <span>4 Elastic Search Nodes</span>

    *   <span>3 Mongo Clusters</span>

    *   <span>3 Engineers</span>

*   <span>5 major database technologies for just their data alone.</span>

*   <span>Growing so fast that MySQL was hot and all the other technologies were being pushed to the limits.</span>

*   <span>When you push something to the limit all these technologies fail in their own special way.</span>

*   <span>Started dropping technologies and asked themselves what they really wanted to be. Did a massive rearchitecture of everything.</span>

## <span>January 2012 - The Age of Maturity</span>

*   After everything was rearchitected the system now looks like:

    *   Amazon EC2 + S3 + Akamai, ELB

    *   <span>90 Web Engines + 50 API Engines</span>

    *   <span>66 MySQL DBs (m1.xlarge) + 1 slave each</span>

    *   <span>59 Redis Instances</span>

    *   <span>51 Memcache Instances</span>

    *   <span>1 Redis Task Manager + 25 Task Processors</span>

    *   <span>Sharded Solr</span>

    *   <span>6 Engineers</span>

*   <span>Now on sharded MySQL, Redis, Memcache, and Solr. That’s it. The advantage is it’s really simple and mature technologies</span>

*   <span>Web traffic keeps going up at the same velocity and iPhone traffic starts ramping up.</span>

## <span>October 12 2012 - The Age of Return</span>

<span>About 4x where they were in January.</span>

*   The numbers now looks like:
    *   Amazon EC2 + S3 + Edge Cast,Akamai, Level 3

    *   180 Web Engines + 240 API Engines

    *   88 MySQL DBs (cc2.8xlarge) + 1 slave each

    *   110 Redis Instances

    *   200 Memcache Instances

    *   4 Redis Task Manager + 80 Task Processors

    *   Sharded Solr

    *   40 Engineers (and growing)

*   <span>Notice that the architecture is doing the right thing. Growth is by adding more of the same stuff. You want to be able to scale by throwing money at the problem. You want to just be able to throw more boxes at the problem as you need them.</span>

*   <span>Now moving to SSDs</span>

## <span>Why Amazon EC2/S3?</span>

*   <span>Pretty good reliability. Datacenters go down too. Multitenancy adds some risk, but it’s not bad.</span>

*   <span>Good reporting and support. They have really good architects and they know the problems</span>

*   <span>Nice peripherals, especially when you are growing. You can spin up in App Engine and talk to their managed cache, load balancing, map reduce, managed databases, and all the other parts that you don’t have to write yourself. You can bootstrap on Amazon’s services and then evolve them when you have the engineers.</span>

*   <span>New instances available in seconds. The power of the cloud. Especially with two engineers you don’t have to worry about capacity planning or wait two weeks to get your memcache. 10 memcaches can be added in a matter of minutes.</span>

*   <span>Cons: limited choice. Until recently you could get SSDs and you can’t get large RAM configurations.</span>

*   <span>Pros: limited choice. You don’t end up with a lot of boxes with different configurations.</span>

## <span>Why MySQL?</span>

*   <span>Really mature.</span>

*   <span>Very solid. It’s never gone down for them and it’s never lost data.</span>

*   <span>You can hire for it. A lot of engineers know MySQL.</span>

*   <span>Response time to request rate increases linearly. Some technologies don’t respond as well when the request rate spikes.</span>

*   <span>Very good software support - XtraBackup, Innotop, Maatkit</span>

*   <span>Great community. Getting questions answered is easy.</span>

*   <span>Very good support from companies like Percona.</span>

*   <span>Free - important when you don’t have any funding initially.</span>

## <span>Why Memcache?</span>

*   <span>Very mature.</span>

*   <span>Really simple. It’s a hashtable with a socket jammed in.</span>

*   <span>Consistently good performance</span>

*   <span>Well known and liked.</span>

*   <span>Consistently good performance.</span>

*   <span>Never crashes.</span>

*   <span>Free</span>

## <span>Why Redis?</span>

*   <span>Not mature, but it’s really good and pretty simple.</span>

*   <span>Provides a variety of data structures.</span>

*   <span>Has persistence and replication, with selectability on how you want them implemented. If you want MySQL style persistence you can have it. If you want no persistence you can have it. Or if you want 3 hour persistence you can have it.</span>

    *   <span>Your home feed is on Redis and is saved every 3 hours. There’s no 3 hour replication. They just backup every 3 hours.</span>

    *   <span>If the box your data is stored on dies, then they just backup a few hours. It’s not perfectly reliable, but it is simpler. You don’t need complicated persistence and replication. It’s a lot simpler and cheaper architecture.</span>

*   <span>Well known and liked.</span>

*   <span>Consistently good performance.</span>

*   <span>Few failure modes. Has a few subtle failure modes that you need to learn about. This is where maturity comes in. Just learn them and get past it.</span>

*   <span>Free</span>

## <span>Solr</span>

*   <span>A great get up and go type product. Install it and a few minutes later you are indexing.</span>

*   <span>Doesn’t scale past one box. (not so with latest release)</span>

*   <span>Tried Elastic Search, but at their scale it had trouble with lots of tiny documents and lots of queries.</span>

*   <span>Now using Websolr. But they have a search team and will roll their own.</span>

## <span>Clustering vs Sharding</span>

*   <span>During rapid growth they realized they were going to need to spread their data evenly to handle the ever increasing load.</span>

*   <span>Defined a spectrum to talk about the problem and they came up with a range of options between Clustering and Sharding.</span>

### <span>Clustering - everything is automatic:</span>

*   <span>Examples: Cassandra, MemBase, HBase</span>

*   <span>Verdict: too scary, maybe in the future, but for now it’s too complicated and has way too many failure modes.</span>

*   <span>Properties:</span>

    *   <span>Data distributed automatically</span>

    *   <span>Data can move</span>

    *   <span>Rebalance to distribute capacity</span>

    *   <span>Nodes communicate with each other. A lot of crosstalk, gossiping and negotiation.</span>

*   <span>Pros:</span>

    *   <span>Automatically scale your datastore. That’s what the white papers say at least.</span>

    *   <span>Easy to setup.</span>

    *   <span>Spatially distribute and colocate your data. You can have datacenter in different regions and the database takes care of it.</span>

    *   <span>High availability</span>

    *   <span>Load balancing</span>

    *   <span>No single point of failure</span>

*   <span>Cons (from first hand experience):</span>

    *   <span>Still fairly young.</span>

    *   <span>Fundamentally complicated. A whole bunch nodes have to symmetrical agreement, which is a hard problem to solve in production.</span>

    *   <span>Less community support. There’s a split in the community along different product lines so there’s less support in each camp.</span>

    *   <span>Fewer engineers with working knowledge. Probably most engineers have not used Cassandra.</span>

    *   <span>Difficult and scary upgrade mechanisms. Could be related to they all use an API and talk amongst themselves so you don’t want them to be confused. This leads to complicated upgrade paths.</span>

    *   <span>Cluster Management Algorithm is a SPOF. If there’s a bug it impacts every node. This took them down 4 times.</span>

    *   <span>Cluster Managers are complex code replicated over all nodes that have the following failure modes:</span>

        *   <span>Data rebalance breaks. Bring a new box and data starts replicating and then it gets stuck. What do you do? There aren’t tools to figure out what’s going on. There’s no community to help, so they were stuck. They reverted back to MySQL.</span>

        *   <span>Data corruption across all nodes. What if there’s a bug that sprays badness into the write log across all of them and compaction or some other mechanism stops? Your read latencies increase. All your data is screwed and the data is gone.</span>

        *   <span>Improper balancing that cannot be easily fixed. Very common. You have 10 nodes and you notice all the node is on one node. There’s a manual process, but it redistributes the load back to one node.</span>

        *   <span>Data authority failure. Clustering schemes are very smart. In one case they bring in a new secondary. At about 80% the secondary says it’s primary and the primary goes to secondary and you’ve lost 20% of the data. Losing 20% of the data is worse than losing all of it because you don’t know what you’ve lost.</span>

### <span>Sharding - everything is manual:</span>

*   <span>Verdict: It’s the winner. Note, I think their sharding architecture has a lot in common with</span> [<span>Flickr Architecture</span>](http://highscalability.com/blog/2007/11/13/flickr-architecture.html)<span>.</span>

*   <span>Properties:</span>

    *   <span>Get rid of all the properties of clustering that you don’t like and you get sharding.</span>

    *   <span>Data distributed manually</span>

    *   <span>Data does not move. They don’t ever move data, though some people do, which puts them higher on the spectrum.</span>

    *   <span>Split data to distribute load.</span>

    *   <span>Nodes are not aware of each other. Some master node controls everything.</span>

*   <span>Pros:</span>

    *   <span>Can split your database to add more capacity.</span>

    *   <span>Spatially distribute and collocate your data</span>

    *   <span>High availability</span>

    *   <span>Load balancing</span>

    *   <span>Algorithm for placing data is very simple. The main reason. Has a SPOF, but it’s half a page of code rather than a very complicated Cluster Manager. After the first day it will work or won’t work.</span>

    *   <span>ID generation is simplistic.</span>

*   <span>Cons:</span>

    *   <span>Can’t perform most joins.</span>

    *   <span>Lost all transaction capabilities. A write to one database may fail when a write to another succeeds.</span>

    *   <span>Many constraints must be moved to the application layer.</span>

    *   <span>Schema changes require more planning.</span>

    *   <span>Reports require running queries on all shards and then perform all the aggregation yourself.</span>

    *   <span>Joins are performed in the application layer.</span>

    *   <span>Your application must be tolerant to all these issues.</span>

### <span>When to shard?</span>

*   <span>If your project will have a few TBs of data then you should shard as soon as possible.</span>

*   <span>When the Pin table went to a billion rows the indexes ran out of memory and they were swapping to disk.</span>

*   <span>They picked the largest table and put it in its own database.</span>

*   <span>Then they ran out of space on the single database.</span>

*   <span>Then they had to shard.</span>

### <span>Transition to Sharding</span>

*   <span>Started the transition process with a feature freeze.</span>

*   <span>Then they decided how they wanted to shard. Want to perform the least amount of queries and go to least number of databases to render a single page.</span>

*   <span>Removed all MySQL joins. Since the tables could be loaded into separate partitions joins would not work.</span>

*   <span>Added a ton of cache. Basically every query has to be cached.</span>

*   <span>The steps looked like:</span>

    *   <span>1 DB + Foreign Keys + Joins</span>

    *   <span>1 DB + Denormalized + Cache</span>

    *   <span>1 DB + Read Slaves + Cache</span>

    *   <span>Several functionally sharded DBs + Read slaves + Cache</span>

    *   <span>ID sharded DBs + Backup slaves + cache</span>

*   <span>Earlier read slaves became a problem because of slave lag. A read would go to the slave and the master hadn’t replicated the record yet, so it looked like a record was missing. Getting around that require cache.</span>

*   <span>They have background scripts that duplicate what the database used to do. Check for integrity constraints, references.</span>

*   <span>User table is unsharded. They just use a big database and have a uniqueness constraint on user name and email. Inserting a User will fail if it isn’t unique. Then they do a lot of writes in their sharded database.</span>

### <span>How to Shard?</span>

*   <span>Looked at Cassandra’s ring model. Looked at Membase. And looked at Twitter’s Gizzard.</span>

*   <span>Determined: the least data you move across your nodes the more stable your architecture.</span>

*   <span>Cassandra has a data balancing and authority problems because the nodes weren’t sure of who owned which part of the data. They decided the application should decide where the data should go so there is never an issue.</span>

*   <span>Projected their growth out for the next five years and presharded with that capacity plan in mind.</span>

*   <span>Initially created a lot of virtual shards. 8 physical servers, each with 512 DBs. All the databases have all the tables.</span>

*   <span>For high availability they always run in multi-master replication mode. Each master is assigned to a different availability zone. On failure the switch to the other master and bring in a new replacement node.</span>

*   <span>When load increasing on a database:</span>

    *   <span>Look at code commits to see if a new feature, caching issue, or other problem occurred.</span>

    *   <span>If it’s just a load increase they split the database and tell the applications to find the data on a new host.</span>

    *   <span>Before splitting the database they start slaves for those masters. Then they swap application code with the new database assignments. In the few minutes during the transition writes are still write to old nodes and be replicated to the new nodes. Then the pipe is cut between the nodes.</span>

### <span>ID structure</span>

*   <span>64 bits:</span>

    *   <span>shard ID: 16 bits</span>

    *   <span>type : 10 bits - Pin, Board, User, or any other object type</span>

    *   <span>local ID - rest of the bits for the ID within the table. Uses MySQL auto increment.</span>

*   <span>Twitter uses a mapping table to map IDs to a physical host. Which requires a lookup. Since Pinterest is on AWS and MySQL queries took about 3ms, they decided this extra level of indirection would not work. They build the location into the ID.</span>

*   <span>New users are randomly distributed across shards.</span>

*   <span>All data (pins, boards, etc) for a user is collocated on the same shard. Huge advantage. Rendering a user profile, for example, does not take multiple cross shard queries. It’s fast.</span>

*   <span>Every board is collocated with the user so boards can be rendered from one database.</span>

*   <span>Enough shards IDs for 65536 shards, but they only opened 4096 at first, they’ll expand horizontally. When the user database gets full they’ll open up more shards and allow new users to go to the new shards.</span>

### <span>Lookups</span>

*   <span>If they have 50 lookups, for example, they split the IDs and run all the queries in parallel so the latency is the longest wait.</span>

*   <span>Every application has a configuration file that maps a shard range to a physical host.</span>

    *   <span>“sharddb001a”: : (1, 512)</span>

    *   <span>“sharddb001b”: : (513, 1024) - backup hot master</span>

*   <span>If you want to look up a User whose ID falls into sharddb003a:</span>

    *   <span>Decompose the ID into its parts</span>

    *   <span>Perform the lookup in the shard map</span>

    *   <span>Connect to the shard, go to the database for the type, and use the local ID to find the right user and return the serialized data.</span>

### <span>Objects and Mappings</span>

*   <span>All data is either an object (pin, board, user, comment) or a mapping (user has boards, pins has likes).</span>

*   <span>For objects a Local ID maps to a MySQL blob. The blob format started with JSON but is moving to serialized thrift.</span>

*   <span>For mappings there’s a mapping table.  You can ask for all the boards for a user. The IDs contain a timestamp so you can see the order of events.</span>

    *   <span>There’s a reverse mapping, many to many table, to answer queries of the type give me all the users who like this pin.</span>

    *   <span>Schema naming scheme is noun_verb_noun: user_likes_pins, pins_like_user.</span>

*   <span>Queries are primary key or index lookups (no joins).</span>

*   <span>Data doesn’t move across database as it does with clustering. Once a user lands on shard 20, for example, and all the user data is collocated, it will never move. The 64 bit ID has contains the shard ID so it can’t be moved. You can move the physical data to another database, but it’s still associated with the same shard.</span>

*   <span>All tables exist on all shards. No special shards, not counting the huge user table that is used to detect user name conflicts.</span>

*   <span>No schema changes required and a new index requires a new table.</span>

    *   <span>Since the value for a key is a blob, you can add fields without destroying the schema. There’s versioning on each blob so applications will detect the version number and change the record to the new format and write it back. All the data doesn’t need to change at once, it will be upgraded on reads.</span>

    *   <span>Huge win because altering a table takes a lock for hours or days.  If you want a new index you just create a new table and start populating it. When you don’t want it anymore just drop it. (no mention of how these updates are transaction safe).</span>

### <span>Rendering a User Profile Page</span>

*   <span>Get the user name from the URL. Go to the single huge database to find the user ID.</span>

*   <span>Take the user ID and split it into its component parts.</span>

*   <span>Select the shard and go to that shard.</span>

*   <span>SELECT body from users WHERE id = <local_user_id></span>

*   <span>SELECT board_id FROM user_has_boards WHERE user_id=<user_id></span>

*   <span>SELECT body FROM boards WHERE id IN (<boards_ids>)</span>

*   <span>SELECT pin_id FROM board_has_pins WHERE board_id=<board_id></span>

*   <span>SELECT body FROM pins WHERE id IN (pin_ids)</span>

*   <span>Most of the calls are served from cache (memcache or redis), so it’s not a lot of database queries in practice.</span>

### <span>Scripting</span>

*   <span>When moving to a sharded architecture you have two different infrastructures, one old, the unsharded system, and one new, the sharded system. Scripting is all the code to transfer the old stuff to the new stuff.</span>

*   <span>They moved 500 million pins and 1.6 billion follower rows, etc</span>

*   <span>Underestimated this portion of the project. They thought it would take 2 months to put data in the shards, it actually took 4-5 months. And remember, this was during a feature freeze.</span>

*   <span>Applications must always write to both old and new infrastructures.</span>

*   <span>Once confident that all your data is in the new infrastructure then point your reads to the new infrastructure and slowly increase the load and test your backends.</span>

*   <span>Built a scripting farm. Spin up more workers to complete the task faster. They would do tasks like move these tables over to the new infrastructure.</span>

*   <span>Hacked up a copy of Pyres, a Python interface to Github’s Resque queue, a queue on built on top of redis. Supports priorities and retries. It was so good they replaced Celery and RabbitMQ with Pyres.</span>

*   <span>A lot of mistakes in the process. Users found errors like missing boards. Had to run the process several times to make sure no transitory errors prevented data from being moved correctly.</span>

## <span>Development</span>

*   <span>Initially tried to give developers a slice of the system. Each having their own MySQL server, etc, but things changed so fast this didn’t work.</span>

*   <span>Went to Facebook’s model where everyone has access to everything. So you have to be really careful.</span>

## <span>Future Directions</span>

*   <span>Service based architecture.</span>

    *   <span>As they are starting to see a lot of database load they start to spawn a lot of app servers and a bunch of other servers. All these servers connect to MySQL and memcache. This means there are 30K connections on memcache which takes up a couple of gig of RAM and causes the memcache daemons to swap.</span>

    *   <span>As a fix these are moving to a service architecture. There’s a follower service, for example, that will only answer follower queries. This isolates the number of machines to 30 that access the database and cache, thus reducing the number of connections.</span>

    *   <span>Helps isolate functionality. Helps organize teams around around services and support for those services. Helps with security as developer can’t access other services.</span>

## <span>Lessons Learned</span>

*   <span>It will fail. Keep it simple.</span>

*   <span>Keep it fun. There’s a lot of new people joining the team. If you just give them a huge complex system it won’t be fun. Keeping the architecture simple has been a big win. New engineers have been contributing code from week one.</span>

*   <span>When you push something to the limit all these technologies fail in their own special way.</span>

*   <span>Architecture is doing the right thing when growth can be handled by adding more of the same stuff. You want to be able to scale by throwing money at the problem by throwing more boxes at the problem as you need them. If your architecture can do that, then you’re golden.</span>

*   <span>Cluster Management Algorithm is a SPOF. If there’s a bug it impacts every node. This took them down 4 times.</span>

*   <span>To handle rapid growth you need to spread data out evenly to handle the ever increasing load.</span>

*   <span>The least data you move across your nodes the more stable your architecture. This is why they went with sharding over clusters.</span>

*   <span>A service oriented architecture rules. It isolates functionality, helps reduce connections, organize teams, organize support, and  improves security.</span>

*   <span>Asked yourself what your really want to be. Drop technologies that match that vision, even if you have to rearchitecture everything.</span>

*   <span>Don’t freak out about losing a little data. They keep user data in memory and write it out periodically. A loss means only a few hours of data are lost, but the resulting system is much simpler and more robust, and that’s what matters.</span>

## <span>Related Articles</span>

*   [Discuss on Hacker News](https://news.ycombinator.com/item?id=5552504)
*   [Pinterest Architecture Update - 18 Million Visitors, 10x Growth,12 Employees, 410 TB Of Data](http://highscalability.com/blog/2012/5/21/pinterest-architecture-update-18-million-visitors-10x-growth.html)
*   [<span>A Short On The Pinterest Stack For Handling 3+ Million Users</span>](http://highscalability.com/blog/2012/2/16/a-short-on-the-pinterest-stack-for-handling-3-million-users.html)
*   [<span>Pinterest Cut Costs From $54 To $20 Per Hour By Automatically Shutting Down Systems</span>](http://highscalability.com/blog/2012/12/12/pinterest-cut-costs-from-54-to-20-per-hour-by-automatically.html)
*   <span>[Instagram Architecture Update: What’s New With Instagram?](http://highscalability.com/blog/2012/4/16/instagram-architecture-update-whats-new-with-instagram.html)</span>
*   [7 Scaling Strategies Facebook Used To Grow To 500 Million Users](http://highscalability.com/blog/2010/8/2/7-scaling-strategies-facebook-used-to-grow-to-500-million-us.html) and [The Four Meta Secrets Of Scaling At Facebook](http://highscalability.com/blog/2010/6/10/the-four-meta-secrets-of-scaling-at-facebook.html) - I think you'll find some similarities in their approaches.

</div>