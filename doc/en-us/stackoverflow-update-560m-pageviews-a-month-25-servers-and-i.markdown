## [StackOverflow Update: 560M Pageviews a Month, 25 Servers, and It's All About Performance](/blog/2014/7/21/stackoverflow-update-560m-pageviews-a-month-25-servers-and-i.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, July 21, 2014 at 9:00AM</span></div>

<div class="body">

![](https://farm4.staticflickr.com/3873/14704918901_2b46d4dd3e_n.jpg)

The folks at Stack Overflow remain incredibly open about what they are doing and why. So it’s time for another [update](http://highscalability.com/blog/2011/10/24/stackexchange-architecture-updates-running-smoothly-amazon-4.html). What has Stack Overflow been up to?

The network of sites that make up [StackExchange](http://stackexchange.com/), which includes StackOverflow, is now ranked 54th for traffic in the world; they have 110 sites and are growing at a rate of 3 or 4 a month; 4 million users; 40 million answers; and 560 million pageviews a month.

This is with just 25 servers. For everything. That’s high availability, load balancing, caching, databases, searching, and utility functions. All with a relative handful of employees. Now that’s quality engineering.

This update is based on [The architecture of StackOverflow](http://www.dev-metal.com/architecture-stackoverflow/) (video) by Marco Cecconi and [What it takes to run Stack Overflow](http://nickcraver.com/blog/2013/11/22/what-it-takes-to-run-stack-overflow/) (post) by Nick Craver. In addition, I’ve merged in comments from various sources. No doubt some of the details are out of date as I meant to write this article long ago, but it should still be representative.<span> </span>

Stack Overflow still uses Microsoft products. Microsoft infrastructure works and is cheap enough, so there’s no compelling reason to change. Yet SO is pragmatic. They use Linux where it makes sense. There’s no purity push to make everything Linux or keep everything Microsoft. That wouldn’t be efficient.<span style="font-size: 12px;"> </span>

Stack Overflow still uses a scale-up strategy. No clouds in site. With their SQL Servers loaded with 384 GB of RAM and 2TB of SSD, AWS would cost a fortune. The cloud would also slow them down, making it harder to optimize and troubleshoot system issues. Plus, SO doesn’t need a horizontal scaling strategy. Large peak loads, where scaling out makes sense, hasn’t  been a problem because they’ve been quite successful at sizing their system correctly.

So it appears Jeff Atwood’s quote: "Hardware is Cheap, Programmers are Expensive", still seems to be living lore at the company.

Marco Ceccon in his talk says when talking about architecture you need to answer this question first: what kind of problem is being solved?

First the easy part. What does StackExchange do? It takes topics, creates communities around them, and creates awesome question and answer sites.<span style="font-size: 12px;"> </span>

The second part relates to scale. As we’ll see next StackExchange is growing quite fast and handles a lot of traffic. How does it do that? Let’s take a look and see….

## Stats

*   StackExchange network has 110 sites growing at a rate of 3 or 4 a month.

*   4 million users

*   8 million questions

*   40 million answers

*   As a network #54 site for traffic in the world

*   100% year over year growth

*   560 million pageviews a month

*   Peak is more like 2600-3000 requests/sec on most weekdays. Programming, being a profession, means weekdays are significantly busier than weekends.

*   25 servers

*   2 TB of SQL data all stored on SSDs

*   Each web server has 2x 320GB SSDs in a RAID 1.

*   Each ElasticSearch box has 300 GB also using SSDs.

*   Stack Overflow has a 40:60 read-write ratio.  

*   DB servers average 10% CPU utilization

*   11 web servers, using IIS

*   2 load balancers, 1 active, using HAProxy

*   4 active database nodes, using MS SQL

*   3 application servers implementing the tag engine, anything searching by tag hits

*   3 machines doing search with ElasticSearch

*   2 machines for distributed cache and messaging using Redis

*   2 Networks (each a Nexus 5596 + Fabric Extenders)

*   2 Cisco 5525-X ASAs (think Firewall)

*   2 Cisco 3945 Routers

*   2 read-only SQL Servers for used mainly for the Stack Exchange API

*   VMs also perform functions like deployments, domain controllers, monitoring, ops database for sysadmin goodies, etc.<span style="font-size: 12px;"> </span>

## Platform

*   ElasticSearch

*   Redis

*   HAProxy

*   MS SQL

*   [Opserver](https://github.com/opserver/Opserver)

*   TeamCity

*   [Jil](https://github.com/kevin-montrose/Jil) - Fast .NET JSON Serializer, built on Sigil

*   [Dapper](https://code.google.com/p/dapper-dot-net/) - a micro ORM.

## UI<span style="font-size: 12px;"> </span>

*   The UI has message inbox that is sent a message when you get a new badge, receive a message, significant event, etc. Done using WebSockets and is powered by redis.

*   Search box is powered by ElasticSearch using a REST interface.

*   With so many questions on SO it was impossible to just show the newest questions, they would change too fast, a question every second. Developed an algorithm to look at your pattern of behaviour and show you which questions you would have the most interest in. It’s uses complicated queries based on tags, which is why a specialized Tag Engine was developed.

*   Server side templating is used to generate pages.

## Servers<span style="font-size: 12px;"> </span>

*   The 25 servers are not doing much, that is the CPU load is low. It’s calculated SO could run on only 5 servers.

*   The database server is at 10%, except when it bursts while performing a backups.

*   How so low? The databases servers have 384GB of RAM and the web servers are at 10%-15% CPU usage.

*   Scale-up is still working. Other scale-out sites with a similar number of pageviews tend to run on 100, 200, up to 300 servers.

*   Simple system. Built on .Net. Have only 9 projects, others systems have 100s. Reason to have so few projects is is so compilation is lightning fast, which requires planning at the beginning. Compilation takes 10 seconds on a single computer.

*   110K lines of code. A small number given what it does.

*   This minimalist approach comes with some problems. One problem is not many tests. Tests aren’t needed because there’s a great community. Meta.stackoverflow is a discussion site for the community and where bugs are reported. Meta.stackoverflow is also a beta site for new software. If users find any problems with it they report the bugs that they’ve found, sometimes with solution/patches.

*   Windows 2012 is used in New York but are upgrading to 2012 R2  (Oregon is already on it). For Linux systems it’s Centos 6.4.

*   Load is really almost all over 9 servers, because 10 and 11 are only for meta.stackexchange.com, meta.stackoverflow.com, and the development tier. Those servers also run around 10-20% CPU which means we have quite a bit of headroom available.

## SSDs<span style="font-size: 12px;"> </span>

*   Intel 330 as the default (web tier, etc.)

*   Intel 520 for mid tier writes like Elastic Search

*   Intel 710 & S3700 for the database tier. S3700 is simply the successor to the high endurance 710 series.

*   Exclusively RAID 1 or RAID 10 (10 being any arrays with 4+ drives). Failures have not been a problem, even with hundreds of intel 2.5" SSDs in production, a single one hasn’t failed yet. One or more spare parts are kept for each model, but multiple drive failure hasn't been a concern.

*   ElasticSearch performs much better on SSDs, given SO writes/re-indexes very frequently.

*   SSD [changes the use of search](http://nickcraver.com/blog/2013/11/22/what-it-takes-to-run-stack-overflow/#comment-1201059884). Lucene.net couldn’t handle SO’s concurrent workloads due to locking issues, so they moved to ElasticSearch. It turns out locks around the binary readers really aren't necessary in an all SSD environment.

*   The only scale-up problems so far is SSD space on the SQL boxes due to the growth pattern of reliability vs. space in the non-consumer space, that isdrives that have capacitors for power loss and such.

## High Availability<span style="font-size: 12px;"> </span>

*   The main datacenter is in New York and the backup datacenter is in Oregon.

*   Redis has 2 slaves, SQL has 2 replicas, tag engine has 3 nodes, elastic has 3 nodes - any other service has high availability as well (and exists in both data centers).

*   Not everything is slaved between data centers (very temporary cache data that's not needed to eat bandwidth by syncing, etc.) but the big items are, so there is still a shared cache in case of a hard down in the active data center. A start without a cache is possible, but it isn't very graceful.

*   Nginx was used for SSL, but a transition has been made to using HAProxy to terminate SSL.

*   Total HTTP traffic sent is only about 77% of the total traffic sent. This is because replication is happening to the secondary data center in Oregon as well as other VPN traffic. The majority of this traffic is the data replication to SQL replicas and redis slaves in Oregon.

## Databasing<span style="font-size: 12px;"> </span>

*   MS SQL Server.

*   Stack Exchange has one database per-site, so Stack Overflow gets one, Super User gets one, Server Fault gets one, and so on. The schema for these is the same. This approach of having different database is effectively a form of partitioning and horizontal scaling.

*   In the primary data center (New York) there is usually 1 master and 1 read-only replica in each cluster. There's also 1 read-only replica (async) in the DR data center (Oregon). When running in Oregon then the primary is there and both of the New York replicas are read-only and async.

*   There are a few wrinkles. There is one "network wide" database which has things like login credentials, and aggregated data (mostly exposed through stackexchange.com user profiles, or APIs).

*   Careers Stack Overflow, stackexchange.com, and Area 51 all have their own unique database schema.

*   All the schema changes are applied to all site databases at the same time. They need to be backwards compatible so, for example, if you need to rename a column - a worst case scenario - it's a multiple steps process: add a new column, add code which works with both columns, back fill the new column, change code so it works with the new column only, remove the old column.

*   Partitioning is not required. Indexing takes care of everything and the data just is not large enough. If something warrants a filtered indexes, why not make it way more efficient? Indexing only on DeletionDate = Null and such is a common pattern, others are specific FK types from enums.

*   Votes are in 1 table per item, for example 1 table for post votes, 1 table for comment votes. Most pages we render real-time, caching only for anonymous users. Given that, there's no cache to update, it’s just a re-query.

*   Scores are denormalized, so querying is often needed. It's all IDs and dates, the post votes table just has 56,454,478 rows currently. Most queries are just a few milliseconds due to indexing.<span style="font-size: 12px;"> </span>

*   The Tag Engine is entirely self-contained, which means not having to depend on an external service for very, very core functionality. It's a huge in-memory struct array structure that is optimized for SO use cases and precomputed results for heavily hit combinations. It's a simple windows service running on a few boxes working in a redundant team. CPU is about 2-5% almost always. Three boxes are not needed for load, just redundancy. If all those do fail at once, the local web servers will load the tag engine in memory and keep on going. 

*   On Dapper's lack of a compiler checking queries compared to traditional ORM. The compiler is checking against what you told it the database looks like. This can help with lots of things, but still has the fundamental disconnect problem you'll get at runtime. A huge problem with the tradeoff is the generated SQL is nasty, and finding the original code it came from is often non-trivial. Lack of ability to hint queries, control parameterization, etc. is also a big issue when trying to optimize queries. For example. literal replacement was added to Dapper to help with query parameterization which allows the use of things like filtered indexes. Dapper also intercepts the SQL calls to dapper and add add exactly where it came from. It saves so much time tracking things down.

## Coding<span style="font-size: 12px;"> </span>

*   The process:

    *   Most programmers work remotely. Programmers code in their own batcave.

    *   Compilation is very fast.

    *   Then the few test that they have are run.

    *   Once compiled, code is moved to a development staging server.

    *   New features are hidden via feature switches.

    *   Runs on same hardware as the rest of the sites.

    *   It’s then moved to Meta.stackoverflow for testing. 1000 users per day use the site, so its a good test.

    *   If it passes it goes live on the network and is tested by the larger community.

*   Heavy usage of static classes and methods, for simplicity and better performance.

*   Code is simple because the complicated bits are packaged in a library and open sourced and maintained. The number of .Net projects stays low because community shared parts of the code are used.

*   Developers get two or three monitors. Screens are important, they help you be productive.

## Caching

*   Cache all the things.

*   5 levels of caches.

*   1st: is the network level cache: caching in the browser, CDN, and proxies.

*   2nd: given for free by the .Net framework and is called the HttpRuntime.Cache. An in-memory, per server cache.

*   3rd: Redis. Distributed in-memory key-value store. Share cache elements across different servers that serve the same site. If StackOverflow has 9 servers then all servers will be able to find the same cached items.

*   4th: SQL Server Cache. The entire database is cached in-memory. The entire thing.

*   5th: SSD. Usually only hit when the SQL server cache is warming up.

*   For example, every help page is cached. Code to access a page is very terse:

    *   Static methods and static classes re used. Really bad from an OOP perspective, but really fast and really friendly towards terse code. All code is directly addressed.

    *   Caching is handled by a library layer of Redis and [Dapper](https://code.google.com/p/dapper-dot-net/), a micro ORM.

*   To get around garbage collection problems, only one copy of a class used in templates are created and kept in a cache. Everything is measured, including GC operation, from statistics it is known that layers of indirection increase GC pressure to the point of noticeable slowness.

*   CDN hits vary, since the  query string hash is based on file content, it’s only re-fetched on a build. It's typically 30-50 million hits a day for 300 to 600 GB of bandwidth. 

*   A CDN is not used for CPU or I/O load, but to help users find answers faster.

## Deploying<span style="font-size: 12px;"> </span>

*   Want to deploy 5 times a day. Don’t build grand gigantic things and then put then live. Important because:

    *   Can measure performance directly.

    *   Forced to build the smallest thing that can possibly work.

*   TeamCity builds then copies to each web tier via a powershell script. The steps for each server are:

    *   Tell HAProxy to take the server out of rotation via a POST

    *   Delay to let IIS finish current requests (~5 sec)

    *   Stop the website (via the same PSSession for all the following)

    *   Robocopy files

    *   Start the website

    *   Re-enable in HAProxy via another POST

*   Almost everything is deployed via puppet or DSC, so upgrading usually consist of just nuking the RAID array and installing from a PXE boot. it's very fast and you know it's done right/repeatable.

## Teaming

*   Teams:

    *   SRE (System Reliability Engineering): - 5 people

    *   Core Dev (Q&A site) : ~6-7 people

    *   Core Dev Mobile: 6 people

    *   Careers team that does development solely for the SO Careers product: 7 people

*   Devops and developer teams are really close-knit.

*   There's a lot of movement between teams.

*   Most employees work remotely.

*   Offices are mostly sales, Denver and London exclusively so.

*   All else equal, it is slightly prefered to have people in NYC, because the in-person time is a plus for the casual interaction that happens in between "getting things done". But the set up makes it possible to do real work and official team collaboration works almost entirely online.

*   They’ve learned that the in-person benefit is more than outweighed by how much you get from being able to hire the best talent that loves the product anywhere, not just the ones willing to live in the city you happen to be in.

*   The most common reason for someone going remote is starting a family. New York's great, but spacious it is not.

*   Offices are in Manhattan and a lot of talent is there. The data center needs to not be a crazy distance away since it is always being improved. There’s also a slightly faster connection to many backbones in the NYC location - though we're talking only a few milliseconds (if that) of difference there.

*   Making an awesome team: Love geeks. Early Microsoft, for example, was full of geeks and they conquered the world.

*   Hire from Stack Overflow community. They looks for a passion for coding, a passion for helping others, and a passion for communicating.

## Budgeting<span style="font-size: 12px;"> </span>

*   Budgets are pretty much project based. Money is only spent as infrastructure is added for new projects. The web servers that have such low utilization are the same ones purchased 3 years ago when the data center was built.

## Testing

*   Move fast and break things. Push it live.

*   Major changes are tested by pushing them. Development has an equally powerful SQL server and it runs on the same web tier, so performance testing isn't so bad.

*   Very few tests. Stack Overflow doesn't use many unit tests because of their active community and heavy usage of static code.

*   Infrastructure changes. There’s 2 of everything, so there’s a backup with the old configuration whenever possible, with a quick failback mechanism. For example, keepalived does failback quickly between load balancers.

*   Redundant systems fail over pretty often just to do regular maintenance. SQL backups are tested by having a dedicated server just for restoring them, constantly (that's a free license - do it). Plan to start full data center failovers every 2 months or so - the secondary data center is read-only at all other times.

*   Unit tests, integration tests and UI tests run on every push. All the tests must succeed before a production build run is even possible. So there’s some mixed messages going on about testing.

*   The things that obviously should have tests have tests. That means most of the things that touch money on the Careers product, and easily unit-testable features on the Core end (things with known inputs, e.g. flagging, our new top bar, etc), for most other things we just do a functionality test by hand and push it to our incubating site (formerly meta.stackoverflow, now meta.stackexchange).

## Monitoring / Logging

*   Now considering using http://logstash.net/ for log management. Currently a dedicated service inserts the syslog UDP traffic into a SQL database. Web pages add headers for the timings on the way out which are captured with HAProxy and are included in the syslog traffic.

*   [Opserver](https://github.com/opserver/Opserver) and Realog. are how many metrics are surfaced. Realog is a logging display system built by Kyle Brandt and Matt Jibson in Go

*   Logging is from the HAProxy load balancer via syslog instead of via IIS. This is a lot more versatile than IIS logs.

## Clouding

*   <div id="_mcePaste">Hardware is cheaper than developers and efficient code. You are only as fast as your slowest bottleneck and all the current cloud solutions have fundamental performance or capacity limits.<span style="font-size: 12px;"> </span></div>

*   Could you build SO well if building for the cloud from day one? Mostl likely. Could you consistency render all your pages performing several up to date queries and cache fetches across that cloud network you don't control and getting sub 50ms render times? That's another matter. Unless you're talking about substantially higher cost (at least 3-4x), the answer is no - it's still more economical for SO to host in their own servers.

## Performance as a Feature<span style="font-size: 12px;"> </span>

*   StackOverflow puts a [heavy emphasis on performance](http://blog.codinghorror.com/performance-is-a-feature/). The goal for the main page  is to load in less than 50ms, but can be as low as 28ms.

*   Programmers are fanatic about reducing page load times and improving the user experience.

*   Timings for every single request to the network are recorded. With these kind of metrics you can make decisions on where to improve your system.

*   The primary reason their servers run at such low utilization is efficient code. Web servers average between 5-15% CPU, 15.5 GB of RAM used and 20-40 Mb/s network traffic.  The SQL servers average around 5-10% CPU, 365 GB of RAM used, and 100-200 Mb/s of network traffic. This has three major benefits: general room to grow before and upgrade is necessary; headroom to stay online for when things go crazy (bad query, bad code, attacks, whatever it may be); and the ability to clock back on power if needed.

## Lessons Learned<span style="font-size: 12px;"> </span>

*   **Why use Redis if you use MS products?** [gabeech](http://www.reddit.com/r/programming/comments/1r83x7/what_it_takes_to_run_stack_overflow/cdkpv7w): It's not about OS evangelism. We run things on the platform they run best on. Period. C# runs best on a windows machine, we use IIS. Redis runs best on a *nix machine we use *nix.

*   **Overkill as a strategy**. Nick Craver on why their network is over provisioned: Is 20 Gb massive overkill? You bet your ass it is, the active SQL servers average around 100-200 Mb out of that 20 Gb pipe.  However, things like backups, rebuilds, etc. can completely saturate it due to how much memory and SSD storage is present, so it does serve a purpose.

*   **SSDs Rock**. The database nodes all use SSD and the average write time is 0 milliseconds.

*   [Know your read/write workload](http://sqlblog.com/blogs/louis_davidson/archive/2009/06/20/read-write-ratio-versus-read-write-ratio.aspx).

*   **Keeping things very efficient means new machines are not needed often**. Only when a new project comes along that needs different hardware for some reason is new hardware added. Typically memory is added, but other than that efficient code and low utilization means it doesn't need replacing. So typically talking about adding a) SSDs for more space, or b) new hardware for new projects.

*   **Don’t be afraid to specialize**. SO uses complicated queries based on tags, which is why a specialized Tag Engine was developed.

*   **Do only what needs to be done**. Tests weren’t necessary because an active community did the acceptance testing for them. Add projects only when required. Add a line of code only when necessary. You Aint Gone Need It really works.

*   **Reinvention is OK**. Typical advice is don’t reinvent the wheel, you’ll just make it worse, by making it square, for example. At SO they don't worry about making a "Square Wheel". If developers can write something more lightweight than an already developed alternative, then go for it.

*   **Go down to the bare metal**. Go into the IL (assembly language of .Net). Some coding is in IL, not C#. Look at SQL query plans. Take memory dumps of the web servers to see what is actually going on. Discovered, for example, a split call generated 2GB of garbage.

*   **No bureaucracy**. There’s always some tools your team needs. For example, an editor, the most recent version of Visual Studio, etc. Just make it happen without a lot of process getting in the way.

*   **Garbage collection driven programming**. SO goes to great lengths to reduce garbage collection costs, skipping practices like TDD, avoiding layers of abstraction, and using static methods. While extreme, the result is highly performing code. When you're doing hundreds of millions of objects in a short window, you can actually measure pauses in the app domain while GC runs. These have a pretty decent impact on request performance.

*   **The cost of inefficient code can be higher than you think**.  Efficient code stretches hardware further, reduces power usage, makes code easier for programmers to understand.

## Related Articles

*   [On Hacker News](https://news.ycombinator.com/item?id=8064534)

*   [What it takes to run Stack Overflow](http://nickcraver.com/blog/2013/11/22/what-it-takes-to-run-stack-overflow/) by Nick Craver

    *   [On Reddit](http://www.reddit.com/r/programming/comments/1r83x7/what_it_takes_to_run_stack_overflow/)

*   Video of [The architecture of StackOverflow](http://www.dev-metal.com/architecture-stackoverflow/)  by Marco Cecconi

    *   [On HackerNews](https://news.ycombinator.com/item?id=7052835)

    *   [Talk Slides](https://speakerdeck.com/sklivvz/the-architecture-of-stackoverflow-developer-conference-2013)

*   [Stackoverflow.com: the road to SSL](http://nickcraver.com/blog/2013/04/23/stackoverflow-com-the-road-to-ssl/)

*   [Which tools and technologies are used to build the Stack Exchange Network?](http://meta.stackexchange.com/questions/10369/which-tools-and-technologies-are-used-to-build-the-stack-exchange-network)

*   [Assault by GC](http://blog.marcgravell.com/2011/10/assault-by-gc.html)

*   [Microsoft SQL Server 2012 AlwaysOn AGs at StackOverflow](http://www.brentozar.com/archive/2012/09/microsoft-sql-server-alwayson-ags-at-stackoverflow/)

*   [Why We (Still) Believe in Working Remotely](http://blog.stackoverflow.com/2013/02/why-we-still-believe-in-working-remotely/)

*   [Running Stack Overflow SQL 2014 CTP 2](http://nickcraver.com/blog/2013/11/18/running-stack-overflow-sql-2014-ctp-2/)

*   Over the years HighScalability has had several articles on StackOverflow: [Stack Overflow Architecture](http://highscalability.com/blog/2009/8/5/stack-overflow-architecture.html), [Scaling Ambition At StackOverflow](http://highscalability.com/blog/2010/2/15/scaling-ambition-at-stackoverflow.html), [Stack Overflow Architecture Update](http://highscalability.com/blog/2011/3/3/stack-overflow-architecture-update-now-at-95-million-page-vi.html), [StackExchange Architecture Updates](http://highscalability.com/blog/2011/10/24/stackexchange-architecture-updates-running-smoothly-amazon-4.html).

</div>