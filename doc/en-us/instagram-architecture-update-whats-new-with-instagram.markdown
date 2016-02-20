## [Instagram Architecture Update: What’s new with Instagram?](/blog/2012/4/16/instagram-architecture-update-whats-new-with-instagram.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, April 16, 2012 at 9:15AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7011/6464246201_bddb8c499e_o.jpg)

The fascination over Instagram continues and fortunately we have several new streams of information to feed the insanity. So consider this article an update to [<span>The Instagram Architecture Facebook Bought For A Cool Billion Dollars</span>](http://highscalability.com/blog/2012/4/9/the-instagram-architecture-facebook-bought-for-a-cool-billio.html)<span>, based primarily on</span> [<span>Scaling Instagram</span>](http://www.scribd.com/doc/89025069/Mike-Krieger-Instagram-at-the-Airbnb-tech-talk-on-Scaling-Instagram)<span>, a slide deck for an AirBnB tech talk given by Instagram co-founder, Mike Krieger. Several other information sources, listed at the bottom of the article, were also used.</span>  

<span>Unfortunately we just have a slide deck, so the connective tissue of the talk is missing, but it’s still very interesting, in the same spirit of wisdom presentations we often see after developers come up for air after spending significant time spent in the trenches.</span>  

<span>If you expect to dive deep into the technological details and find a billion reasons why Instagram was acquired, you will be disappointed. That magic can be found in the emotional investment in the relationship between all of the users and the product, not in the bits about how they bytes are managed.</span>  
<span></span>  
<span>So what’s new with Instagram?</span>

*   <span>Some stats:</span>
    *   <span>Instagram went to 30+ million users in less than two years and then rocketed to 40 million users 10 days after the launch of its Android application.</span>
    *   <span>After the release of the Android they had 1 million new users in 12 hours.</span>
*   <span>Instagram’s mission was to communicate and share the real world.</span>
*   <span>The world has changed. 2 backend engineers can now scale a system to 30+ million users:</span>
    *   <span>Started as two guys with no backend experience</span>
    *   <span>Hosted on a single machine in LA that was less powerful than a MacBook Pro</span>
    *   <span>25K signups on the first day that melted the machine so they moved to Amazon</span>
    *   <span>2 engineers in 2010.</span>
    *   <span>3 engineers in 2011</span>
    *   <span>5 engineers 2012,  2.5 on the backend. This includes iPhone and Android development.</span>
    *   <span>Scarcity gives focus.</span>
*   <span>Most of your initial scaling problems won’t be glamorous:</span>
    *   <span>Scaling is like replacing all components on a car while driving it at 100 mph.</span>
    *   <span>A missing favicon.ico that caused a lot of 404 errors in Django</span>
    *   <span>memcached -t 4</span>
    *   <span>prefork/postfork</span>
        *   <span>Unfortunately we don’t have the details on what the problems were</span>
*   <span>The Instagram philosophy:</span>
    *   <span>simplicity</span>
    *   <span>optimized for minimal operational burden</span>
    *   <span>instrument everything</span>
*   <span>What happens when a user uploads a photo with an optional caption and location?</span>
    *   <span>Synchronous write to the media database for the user.</span>
    *   <span>If geotagged an asynchronous process POSTS the image to Solr for indexing.</span>
    *   <span>Delivery to followers is via lists stored in Redis. The Media ID is pushed on the list for everyone following the user. When it’s time render a feed they take a small number of IDs and look them up in memcached.</span>
*   <span>Scaling the Database:</span>
    *   <span>Django ORM and PostgreSQL</span>
    *   <span>Postgres selected because it had PostGIS, a spatial database extension for PostgreSQL.</span>
    *   <span>Moved the database to its own machine but photos kept on growing and there’s a limit of 68GB RAM on biggest machine in EC2</span>
    *   <span>Next strategy was vertical partitioning, moving functions to their own servers. Django DB routers made it pretty easy, photos were mapped to the photodb, for example.</span>
    *   <span>Then when the photos database reached 60GB and they couldn’t run it on one machine anymore, they moved to sharding.</span>
    *   <span>PostgreSQL can handle tens of thousands of requests with light caching with memcached.</span>
    *   <span>All snapshots are from a slave, the slave is stopped before taking a snapshot, then XFS-freeze all drives, then take the snapshot to ensure it's consistent.</span>
    *   <span>Software-RAID is used onn EBS drives to get better write throughput. Write-Ahead Logs (WALs) are put on a different RAID from the main database.</span>
*   <span>Used a pre-split logical sharding strategy:</span>
    *   <span>Sharding based on User ID.</span>
    *   <span>One of the problems with sharding is when a shard gets bigger than a machine. So they made many thousand of logical shards that map to fewer physical nodes.</span>
    *   <span>A logical shard to physical map is kept.</span>
    *   <span>When a machine fills up then a logical shard can be moved to a new machine to relieve the pressure. The advantage is nothing needs to be resharded. Saved by a level of indirection.  </span>
    *   <span>The</span> [<span>schemas</span>](http://www.postgresql.org/docs/9.0/static/ddl-schemas.html) <span>feature of PostgreSQL was very helpful, but I can’t tell exactly how from the slides.</span>
    *   Membase uses a [similar approach](http://www.couchbase.com/docs/membase-manual-1.7/membase-architecture.html).
*   <span>Follow graph:</span>
    *   <span>V1: Simple DB table: (source id, target id, status)</span>
    *   <span>Needed to answer questions like: who do I follow? Who follows me? Do I follow X? Does X follow me?</span>
    *   <span>With the DB being busy they started storing a parallel version of the follow graph in Redis. Which lead to consistency problem that required the introduction of cache invalidation and a lot of extra logic.</span>
*   <span>Love Redis for:</span>
    *   <span>caching complex objects where you want to do more than a GET: counting, subranges, membership tests</span>
    *   <span>fast inserts and a fast subsets</span>
    *   <span>data structures that are relatively bounded in size</span>
    *   <span>When a Redis instance crossed 40k req/s, and started becoming a bottleneck, bringing up another machine, SYNCing to the master, and sending read queries to it took less than 20 minutes.</span>
*   <span>Monitor everything.</span>
    *   <span>Ask questions like: How is the system right now? How does this compare to historical trends?</span>
    *   [<span>Statsd</span> ](http://github.com/etsy/statsd/)<span>- a network daemon that aggregates and rolls-up data into Graphite</span>
        *   <span>Has two types of statistics: counter and timer.</span>
        *   <span>Stats can be added dynamically</span>
        *   <span>Counters rack everything from number of signups per second to number of likes</span>
        *   <span>Timers time generation of feeds, how long it takes to follow users, and any other major action.</span>
        *   <span>Realtime allows the immediate evaluation of system and code changes</span>
        *   <span>Logging calls are inserted throughout the web application at a low sample rates so performance is not impacted.</span>
    *   [<span>Dogslow</span>](http://blog.bitbucket.org/2011/05/17/tracking-slow-requests-with-dogslow/) <span>- watches running processes and if notices any taking longer than N seconds it will snapshot the current process and write the file to disk.</span>
        *   <span>Found responses were taking over 1.5s to return a response were often stuck in memcached set() and get_many().</span>
        *   <span>Using Munin they saw Redis was handling  50k req/s</span>
*   <span>node2dm - a node.js server for delivering push notifications to Android’s C2DM service</span>
    *   <span>Written by Instagram because they couldn’t find anything else.</span>
    *   <span>Handled over 5 million push notifications</span>
    *   <span>It’s being</span> [<span>open sourced</span>](http://github.com/Instagram/node2dm)
*   <span>Surround yourself with great advisors. Instagram started with an OK stack and terrible configuration. They ended up doing great on both counts; now they have almost no downtime, they deploy several times an hour, and their comprehensive tests take only 5 minutes to run. Some of that came from skill and prior knowledge, but the majority from adaptability and the willingness to switch quickly when advisers (e.g. Adam d'Angelo) suggested better alternatives. (This summary is from</span> [<span>xianshou</span>](http://news.ycombinator.com/item?id=3832556)<span>, who attended the talk).</span>
*   <span>Engineer solutions you are not constantly returning to because they are broke:</span>
    *   <span>Implement extensive unit and functional tests</span>
    *   <span>Keep it DRY - Do Not Repeat Yourself</span>
    *   <span>Embrace loose coupling using notifications and signals.</span>
    *   <span>Do most of your work in Python and drop to C when necessary</span>
    *   <span>Keep things in the shared mind using frequent code reviews and pull requests</span>
*   <span>Realtime stats that can be added dynamically lets you diagnose and firefight without having to wait to receive new data.</span>
*   <span>If read capacity is likely to be a concern, bringing up read-slaves ahead of time and getting them in rotation is ideal; if any new read issues crop up, however, know ahead of time what your options are for bringing more read capacity into rotation.</span>
*   <span>It’s often one piece of the backend infrastructure that becomes a bottleneck. Figuring out the point at which your real, live appservers get stuck can help surface the issue.</span>
*   <span>Take tech/tools you know and try first to adapt them into a simple solution.</span>
*   <span>Have a versatile complement to your core data storage, like Redis.</span>
*   <span>Try not to have two tools doing the same job.</span>
*   <span>Know where to shed load if needed. Display shorter feeds, for example.</span>
*   <span>Don’t reinvent the wheel. If app servers are dying, for example, dont’t write yet another monitoring system. HAProxy already does this already, so use it.</span>
*   <span>Focus on making what you have better: fast, beautiful, photo sharing. Make the fast part faster. Can we all of our requests take 50% of the time?</span>
*   <span>Stay nimble, remind yourself of what is important.</span>
*   <span>Users don’t care that you wrote your own DB.</span>
*   <span>Don’t tie yourself to a solution where your in-memory database is your main DB store.</span>
*   <span>You don’t get to choose when scaling challenges come up.</span>
*   <span>Don't over-optimize or expect to know ahead of time how site will scale</span>
*   <span>Don’t think someone else will join and take care of this.</span>
*   <span>There are few, if any, unsolvable scaling challenges for a social startup</span>
*   <span>Only add software to your stack that  is optimized for operational simplicity</span>
*   <span>Have fun.</span>
*   <span>Instagram succeeded more on its understanding of the power of psychology in design than technology.</span>
*   <span>Ideas are disposable: if one doesn’t work, you quickly move on to another.</span>
*   <span>Key word is simplicity.</span>
*   <span>Timing matters. You make your own luck.</span>
*   <span>The real social part of a social network:</span>
    *   <span>Attend Stanford and the attention of the world’s biggest venture capitalists will be on you.</span>
    *   <span>In an intensely competitive start-up scene success is as much about who you know as what you know. Make sure to spend some time after the talk getting to know the people around you.</span>
    *   <span>The social ties that you make are directly correlated to success. There is some serendipity for entrepreneurs, but the people who are the rainmakers are the ones who entrepreneurs need to meet in order to make those connections that lead to success. (From Ted Zoller, a senior fellow at the Ewing Marion Kauffman Foundation).</span>

## <span>Related Articles</span>

*   [<span>Slidedeck</span>](http://www.scribd.com/doc/89025069/Mike-Krieger-Instagram-at-the-Airbnb-tech-talk-on-Scaling-Instagram) <span>- Mike Krieger, Instagram at the Airbnb tech talk, on Scaling Instagram</span>
*   [<span>How To Scale A $1 Billion Startup: A Guide From Instagram Co-Founder</span>](http://techcrunch.com/2012/04/12/how-to-scale-a-1-billion-startup-a-guide-from-instagram-co-founder-mike-krieger/)<span></span>
*   [<span>On HackerNews</span>](http://news.ycombinator.com/item?id=3831865)<span>,</span> [<span>On HackerNews</span>](http://news.ycombinator.com/item?id=3804351)
*   [<span>Behind Instagram’s Success, Networking the Old Way</span>](http://www.nytimes.com/2012/04/14/technology/instagram-founders-were-helped-by-bay-area-connections.html)
*   [<span>Instagram’s User Count Now At 40 Million, Saw 10 Million New Users In Last 10 Days</span>](http://techcrunch.com/2012/04/13/instagrams-user-count-now-at-40-million-saw-10-million-new-users-in-last-10-days/)
*   <span>[Keeping Instagram up with over a million new users in twelve hours](http://instagram-engineering.tumblr.com/post/20541814340/keeping-instagram-up-with-over-a-million-new-users-in)</span>

</div>