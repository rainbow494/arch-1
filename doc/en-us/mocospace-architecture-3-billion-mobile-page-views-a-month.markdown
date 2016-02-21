## [MocoSpace Architecture - 3 Billion Mobile Page Views a Month](/blog/2010/5/3/mocospace-architecture-3-billion-mobile-page-views-a-month.html)

    

    

![](http://img.mocospace.com.edgesuite.net/html/images/logo_moco-alpha.png)

This is a guest post by Jamie Hall, Co-founder & CTO of [MocoSpace](http://www.mocospace.com/), describing the architecture for their mobile social network. This is a timely architecture to learn from as it combines several hot trends: it is very large, mobile, and social. What they think is especially cool about their system is: how it optimizes for device/browser fragmentation on the mobile Web; their multi-tiered, read/write, local/distributed caching system; selecting PostgreSQL over MySQL as a relational DB that can scale.

MocoSpace is a mobile social network, with 12 million members and 3 billion page views a month, which makes it one of the most highly trafficked mobile Websites in the US. Members access the site mainly from their mobile phone Web browser, ranging from high end smartphones to lower end devices, as well as the Web. Activities on the site include customizing profiles, chat, instant messaging, music, sharing photos & videos, games, eCards and blogs. The monetization strategy is focused on advertising, on both the mobile and Websites, as well as a virtual currency system and a handful of premium feature upgrades.

## Stats

1.  3 billion page views a month
2.  Top 4 most trafficked mobile website after MySpace, Facebook and Google (http://www.groundtruth.com/mobile-is-mobile)
3.  75% mobile Web, 25% Web
4.  12 million members
5.  6 million unique visitors a month
6.  100k concurrent users
7.  12 million photo uploads a month
8.  2 million emails received a day, 90% spam, 2.5 million sent a day
9.  Team of 8 developers, 2 QA, 1 sysadmin

## Platform

1.  CentOS + Red Hat
2.  Resin application server — Java Servlets, JavaServer Pages, Comet
3.  PostgreSQL
4.  Memcached
5.  ActiveMQ's job + message queue, in Red Hat cluster for high availability
6.  Squid - static content caching, tried Varnish but had stability issues
7.  JQuery + Ajax
8.  S3 for user photo & video storage (8 TB) and EC2 for photo processing
9.  F5 BigIP load balancers - sticky sessions, gzip compression on all pages
10.  Akamai CDN - 2 TB a day, 250 million requests a day
11.  Monitoring - Nagios for alerts, Zabbix for trending
12.  EMC SAN - high IO performance for databases by RAIDing (RAID 10) lots of disks, replacing with high performance Fusion-io solid-state flash ioDrives, much more cost effective
13.  PowerMTA for mail delivery, Barracuda spam firewalls
14.  Subversion source control, Hudson for continuous integration
15.  FFMPEG for Mobile to Web and Web to mobile video transcoding
16.  Selenium for browser test case automation
17.  Web tier
    1.  5x Dell 1950, 2x dual core, 16G RAM
    2.  5x Dell 6950/R905, 4x dual core, 32G RAM
18.  Database tier
    1.  2x Sun Fire X4600 M2 Server, 8x quad core, 256G RAM
    2.  2x Dell 6950, 4x dual core, 64G RAM

## Architecture

1.  All pages are dynamic, with user data and customizations as well as many browser and device specific optimizations. Browser and device fragmentation issues are much greater on mobile than on the Web. Many optimizations, adaptations required based on browser capabilities, limited support for CSS/JavaScript, screen size, etc. Mobile Web traffic is often served via network proxies (gateways), with poor support for Cookies, making session management and user identification a challenge.
2.  A big challenge is handling the device/browser fragmentation on the mobile Web - optimizing for a huge range of device capabilities (everything from iPhones with touchscreens to 5 year old Motorola Razrs), screen sizes, lack of / inconsistent Web standards compliance, etc. We abstract out our presentation layer so we can serve pages to all mobile devices from the same code base, and maintain a large device capabilities database (containing things like screen size, supported file types, maximum allowed page sizes, etc) which is used to drive generation of our pages. The database contains capability details for hundreds of devices and mobile browser types.
3.  Database is sharded based on a user key, with a master lookup table mapping users to shards. We rolled our own query and aggregation layer, allowing us to query and join data across shards, though this is not used frequently. With sharding we sacrifice some consistency, but that's Ok as long as you're not running a bank. We perform data consistency checks offline, in batches, with the goal being eventual consistency. Large tables are partitioned into smaller sub tables for more efficient access, reducing time tables are locked for updates as well as operational maintenance activities. Log shipping used for warm standbys.
4.  A multi-tiered caching system is used, with data cached locally within the application servers as well as distributed via Memcached. When making an update we don't just invalidate the cache and then re-populate after reading again from the database, rather we update Memcached with the new data and save another trip to the database. When updating the cache an invalidation directive is sent via the messaging queue to the local caches on each of the application servers.
5.  A distributed message queue is used for distributed server communication, everything from sending messages in realtime between users to system messages such as local cache invalidation directives.
6.  Dedicated server for building and traversing social graph entirely in memory, used to generate friend recommendations, etc.
7.  Load balancer used for rolling deploys of new versions of the site without affecting performance/traffic.
8.  Release every 2 weeks. Longer release cycles = exponential complexity, more difficult to troubleshoot and rollback. Development team responsible for deploying to and managing production systems ¿ ¿you built it, you manage it¿.
9.  Kickstart used to automate server builds from bare metal. Puppet is used to configure a server to a specific personality i.e. Webserver, database server, cache server etc, as well as to push updated policies to running nodes.

## Lessons Learned

1.  **Make your boxes sweat**. Don't be afraid of high system load as long as response times are acceptable. We pack as many as five application server instances on a single box, each running on a different port. Scale up to the high end of commodity hardware before scaling out. Can pick up used or refurbished powerful 4U boxes for cheap.
2.  **Understand where your bottlenecks are in each tier**. Are you CPU or IO (disk, network) bound? Database is almost always IO (disk) bound. Once the database doesn't fit in RAM you hit a wall.
3.  **Profile the database religiously**. Obsess when it comes to optimizing the database. Scaling Web tier is cheap and easy, database tier is much harder and expensive. Know the top queries on your system, both by execution time and frequency. Track and benchmark top queries with each release, need to catch and address performance issues with the database early. We use the pgFouine log analyzer and new PostgreSQL pg_stat_statements utility for generating profiling snapshots in real-time.
4.  **Design to disable**. Be able to configure and turn off anything you release instantly, without requiring a code change or deployment. Load and stress testing are important but nothing like testing with live, production traffic via incremental, phased rollouts.
5.  **Communicate synchronously only when absolutely necessary**. When one component or service fails it shouldn't bring down other parts of the system. Do everything you can in the background or as a separate thread or process, don't make the user wait. Update the database in batches wherever possible. Any system making requests outside the network need aggressive monitoring, timeout settings, and failure handling / retries. For example, we found S3 latency and failure rates can be significant, so we queue failed calls and retry later.
6.  **Think about monitoring during design, not after**. Every component should produce performance, health, throughput, etc data. Set up alerts when these exceed thresholds. Consolidated graphs showing metrics across all instances, rather than just per instance, are particularly helpful for identifying issues and trends at a glance and should be reviewed daily — if normal operating behavior isn't well understood it's impossible to identify and respond to what isn't. We tried many monitoring systems - Cacti, Ganglia, Hyperic, Zabbix, Nagios, as well as some custom built solutions. Whichever you use the most important thing is to be comfortable with it, otherwise you won't use it enough. It should be easy, using templates, etc to quickly monitor new boxes and services as you throw them up.
7.  **Distributed sessions can be a lot of overhead**. Go stateless when you can, but when you can't consider sticky sessions. If the server fails the user loses their state and may need to re-login, but that's rare and acceptable depending on what you need to do.
8.  **Monitor and beware of full/major garbage collection events in Java**, which can lock up the whole JVM for up to 30 seconds. We use Concurrent Mark Sweep (CMS) garbage collector, which introduces some additional system overhead, but have been able to eliminate full garbage collections.
9.  When a site gets large enough it becomes a magnet for spammers and hackers, both on site and from outside via email, etc. Captcha and IP monitoring are not enough. Must **invest aggressively in detection and containment systems**, internal tools to detect suspicious user behavior and alert and/or attempt to automatically contain.
10.  **Soft delete whenever possible**. Mark data for later deletion, rather than deleting immediately. Deletion can be costly, so queue up for after hours, plus if someone makes a mistake and deletes something they shouldn¿t have it¿s easy to rollback.
11.  **N+1 design**. Never less than two of anything.

I'd really like to thank Jamie for taking the time write this experience report. It's a really useful contribution for others to learn from. If you would like to share the architecture for your fablous system, please [    contact     me](../../contact/) and we'll get started.

## Related Articles

*   [Mobile Social Network MocoSpace Now 11 Million Members Strong](http://techcrunch.com/2010/03/17/mobile-social-network-mocospace-now-11-million-members-strong/ "Mobile Social Network MocoSpace Now 11 Million  Members Strong")
*   [    How MocoSpace Works    ](http://computer.howstuffworks.com/internet/social-networking/networks/mocospace.htm)

    