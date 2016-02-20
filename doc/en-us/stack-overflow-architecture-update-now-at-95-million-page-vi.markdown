## [Stack Overflow Architecture Update - Now at 95 Million Page Views a Month](/blog/2011/3/3/stack-overflow-architecture-update-now-at-95-million-page-vi.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, March 3, 2011 at 9:02AM</span></div>

<div class="body">

![](http://farm6.static.flickr.com/5052/5488952221_77f2126053_o.jpg)

A lot has happened since my first article on the [Stack Overflow Architecture](/blog/2009/8/5/stack-overflow-architecture.html). Contrary to the theme of that last article, which lavished attention on Stack Overflow's dedication to a scale-up strategy, Stack Overflow has both grown up and out in the last few years.

Stack Overflow has grown up by more then doubling in size to over 16 million users and multiplying its number of page views nearly 6 times to 95 million page views a month.  

Stack Overflow has grown out by expanding into the [Stack Exchange Network](http://stackexchange.com/), which includes Stack Overflow, Server Fault, and Super User for a grand total of 43 different sites. That's a lot of fruitful multiplying going on.

What hasn't changed is Stack Overflow's openness about what they are doing. And that's what prompted this update. A recent series of posts talks a lot about how they've been handling their growth: [Stack Exchange’s Architecture in Bullet Points](http://blog.serverfault.com/post/stack-exchanges-architecture-in-bullet-points/), [Stack Overflow’s New York Data Center](http://blog.serverfault.com/post/1432571770/), [Designing For Scalability of Management and Fault Tolerance](http://blog.serverfault.com/post/1097492931/), [Stack Overflow Search — Now 81% Less](http://blog.stackoverflow.com/2011/01/stack-overflow-search-now-81-less-crappy/), [Stack Overflow Network Configuration](http://blog.stackoverflow.com/2010/01/stack-overflow-network-configuration/), [Does StackOverflow use caching and if so, how?](http://meta.stackoverflow.com/questions/69164/does-stackoverflow-use-caching-and-if-so-how), [Which tools and technologies build the Stack Exchange Network?](http://meta.stackoverflow.com/questions/10369/which-tools-and-technologies-build-the-stack-exchange-network).

Some of the more obvious differences across time are:

*   **Just More**. More users, more page views, more datacenters, more sites, more developers, more operating systems, more databases, more machines. Just a lot [more of more](http://blog.stackoverflow.com/2011/01/state-of-the-stack-2010-a-message-from-your-ceo/).
*   **Linux**. Stack Overflow was known for their Windows stack, now they are using a lot more Linux machines for HAProxy, Redis, Bacula, Nagios, logs, and routers. All support functions seem to be handled by Linux, which has required the development of [parallel release processes](http://blog.serverfault.com/post/1097492931/). 
*   **Fault Tolerance**. Stack Overflow is now [being served by two different](http://blog.stackoverflow.com/2010/01/stack-overflow-network-configuration/) switches on two different internet connections, they've added redundant machines, and some functions have moved to a second datacenter.
*   **NoSQL**. Redis is now used as a [caching layer](http://meta.stackoverflow.com/questions/69164/does-stackoverflow-use-caching-and-if-so-how) for the entire network. There wasn't a separate caching tier before so this a big change, as is using a NoSQL database on Linux.

Unfortunately, I couldn't find any coverage on some of the open questions I had last time, like how they were going to deal with multi-tenancy across so many diffrent properties, but there's still plenty to learn from. Here's a roll up a few different sources:

### The Stats

*   95 Million Page Views a Month
*   800 HTTP requests a second
*   180 DNS requests a second
*   55 Megabits per second
*   16 Million Users  - Traffic to Stack Overflow grew 131% in 2010, to 16.6 million global monthly uniques. 

### Data Centers

*   1 Rack with Peak Internet in OR (Hosts our chat and Data Explorer)
*   2 Racks with Peer 1 in NY (Hosts the rest of the Stack Exchange Network)

### Hardware

*   10 Dell R610 IIS web servers (3 dedicated to Stack Overflow):
    *   1x Intel Xeon Processor E5640 @ 2.66 GHz Quad Core with 8 threads
    *   16 GB RAM
    *   Windows Server 2008 R2

*   2 Dell R710 database servers:
    *   2x Intel Xeon Processor X5680 @ 3.33 GHz
    *   64 GB RAM
    *   8 spindles
    *   SQL Server 2008 R2

*   2 Dell R610 HAProxy servers:
    *   1x Intel Xeon Processor E5640 @ 2.66 GHz
    *   4 GB RAM
    *   Ubuntu Server

*   2 Dell R610 Redis servers:
    *   2x Intel Xeon Processor E5640 @ 2.66 GHz
    *   16 GB RAM
    *   CentOS

*   1 Dell R610 Linux backup server running Bacula:
    *   1x Intel Xeon Processor E5640 @ 2.66 GHz
    *   32 GB RAM

*   1 Dell R610 Linux management server for Nagios and logs:
    *   1x Intel Xeon Processor E5640 @ 2.66 GHz
    *   32 GB RAM

*   2 Dell R610 VMWare ESXi domain controllers:
    *   1x Intel Xeon Processor E5640 @ 2.66 GHz
    *   16 GB RAM

*   2 Linux routers
*   5 Dell Power Connect switches

### Dev Tools

*   **C#**: Language
*   **Visual Studio 2010 Team Suite**<span style="font-weight: normal;">: </span>IDE
*   **Microsoft ASP.NET (version 4.0)**<span style="font-weight: normal;">: </span>Framework
*   **ASP.NET MVC 3**<span style="font-weight: normal;">: </span>Web Framework
*   **Razor**<span style="font-weight: normal;">: </span>View Engine
*   **jQuery 1.4.2**<span style="font-weight: normal;">: </span>Browser Framework:
*   **LINQ to SQL, some raw SQL**<span style="font-weight: normal;">: </span>Data Access Layer
*   **Mercurial and Kiln**<span style="font-weight: normal;">: </span>Source Control
*   **Beyond Compare 3**<span style="font-weight: normal;">:</span> Compare Tool

### Software and Technologies Used

*   Stack Overflow uses a [WISC](http://stackoverflow.com/questions/177901/what-does-wisc-stack-mean) stack via [BizSpark](http://blog.stackoverflow.com/2009/03/stack-overflow-and-bizspark/)
*   **Windows Server 2008 R2 x64**: Operating System
*   **SQL Server 2008 R2** running **Microsoft Windows Server 2008 Enterprise Edition x64**: Database
*   Ubuntu Server
*   CentOS
*   **IIS 7.0**: Web Server
*   **HAProxy**: for load balancing
*   **Redis**: used as the distributed caching layer.
*   **CruiseControl.NET**: for builds and automated deployment
*   **Lucene.NET**:  for search
*   **Bacula**: for backups
*   **Nagios**: (with [n2rrd](http://n2rrd.diglinks.com/cgi-bin/trac.fcgi) and drraw plugins) for monitoring
*   **Splunk:** for logs
*   **SQL Monitor:** from Red Gate - for SQL Server monitoring
*   **Bind**: for DNS
*   **[Rovio](http://www.wowwee.com/en/products/tech/telepresence/rovio/rovio)**:  a little robot (a real robot) allowing remote developers to visit the office “virtually.”
*   **Pingdom**:  an external monitor and alert service.

### External Bits

<span style="border-collapse: collapse; font-family: Arial, 'Liberation Sans', 'DejaVu Sans', sans-serif; font-size: 14px; line-height: 18px; color: #000000;">Code that is not included as part of the development tools:</span>

*   reCAPTCHA
*   DotNetOpenId
*   WMD - Now developed as open source. See github network graph
*   Prettify
*   Google Analytics
*   Cruise Control .NET
*   HAProxy
*   Cacti
*   MarkdownSharp
*   Flot
*   Nginx
*   Kiln
*   CDN: none, all static content is served off the [sstatic.net](http://sstatic.net/), which is a fast, cookieless domain intended for static content delivered to the Stack Exchange family of websites.

### Developers and System Administrators

*   14 Developers
*   2 System Administrators

### Content

*   **License:** Creative Commons Attribution-Share Alike 2.5 Generic
*   **Standards:** OpenSearch, Atom
*   **Host:** PEAK Internet

### More Architecture and Lessons Learned

*   HAProxy is used instead of Windows NLB because HAProxy is cheap, easy, free, works great as a 512MB VM “device” on a network via Hyper-V. It also works in front of the boxes so it’s completely transparent to them, and easier to troubleshoot as a different networking layer instead of being intermixed with all your windows configuration.
*   A CDN is not used because even “cheap” CDNs like Amazon one are very expensive relative to the bandwidth they get bundled into their existing host’s plan. The least they could pay is $1k/month based on Amazon’s CDN rates and their bandwidth usage.
*   Backup is to disk for fast retrieval and to tape for historical archiving.
*   Full Text Search in SQL Server is very badly integrated, buggy, deeply incompetent, so they went to Lucene.
*   Mostly interested in peak HTTP request figures as this is what they need to make sure they can handle.
*   All properties now run on the same Stack Exchange platform. That means Stack Overflow, Super User, Server Fault, Meta, WebApps, and Meta Web Apps are all running on the same software.
*   There are separate StackExchange sites because people have different sets of expertise that shouldn't cross over to different topic sites. [You can be the greatest chef in the world, but that doesn't qualify you for fixing a server.](http://meta.stackoverflow.com/questions/69422/why-separate-stack-exchange-accounts)
*   They aggressively cache everything.
*   All pages accessed by (and subsequently served to) annonymous users are cached via [Output Caching](http://learn.iis.net/page.aspx/154/walkthrough-iis-70-output-caching/).
*   Each site has 3 distinct caches: local, site, global.
*   **local cache**: can only be accessed from 1 server/site pair 
    *   To limit network latency they use a local "L1" cache, basically HttpRuntime.Cache, of recently set/read values on a server. This would reduce the cache lookup overhead to 0 bytes on the network.
    *   Contains things like user sessions, and pending view count updates.
    *   This resides purely in memory, no network or DB access.
*   **site cache**:  can be accessed by any instance (on any server) of a single site
    *   Most cached values go here, things like hot question id lists and user acceptance rates are good examples
    *   This resides in Redis (in a distinct DB, purely for easier debugging)
    *   Redis is so fast that the slowest part of a cache lookup is the time spent reading and writing bytes to the network.
    *   Values are compressed before sending them to Redis. They have plenty of CPU and most of their data are strings so they get a great compression ratio.
    *   The CPU usage on their Redis machines is 0%.
*   **global cache**: which is shared amongst all sites and servers
    *   Inboxes, API usage quotas, and a few other truly global things live here
    *   This resides in Redis (in DB 0, likewise for easier debugging)
*   Most items in the cache expire after a timeout period (a few minutes usually) and are never explicitly removed. When a specific cache invalidation is required they use [Redis messaging](http://code.google.com/p/redis/wiki/PublishSubscribe) to publish removal notices to the "L1" caches.
*   Joel Spolsky is not a Microsoft Loyalist, he doesn't make the technical decisions for Stack Overflow, and considers Microsoft licensing a rounding error. Consider yourself corrected [Hacker News commentor](http://news.ycombinator.com/item?id=2284900).
*   For their IO system [they selected](http://blog.serverfault.com/post/our-storage-decision/) a RAID 10 array of [Intel X25 solid state drives](http://www.intel.com/design/flash/nand/extreme/index.htm) . The RAID array eased any concerns about reliability and the SSD drives performed really well in comparision to FusionIO at a much cheaper price.
*   The [full boat cost](http://news.ycombinator.com/item?id=2285931) for their Microsoft licenses would be approximately $242K. Since Stack Overflow is using Bizspark they are not paying near the full sticker price, but that's the max they could pay. 
*   [Intel NICs are replacing Broadcom NICs](http://blog.serverfault.com/post/broadcom-die-mutha/) and their primary production servers. This solved problems they were having with  connectivity loss, packet loss, and corrupted arp tables.

## Related Articles

*   [Hacker News Thread on this Post](http://news.ycombinator.com/item?id=2284900) / [Reddit Thread](http://www.reddit.com/r/programming/comments/fwpik/stackoverflow_scales_using_a_mixture_of_linux_and/)
*   [Stack Exchange’s Architecture in Bullet Points](http://blog.serverfault.com/post/stack-exchanges-architecture-in-bullet-points/) / [HackerNews Thread](http://news.ycombinator.com/item?id=2207789)
*   [Stack Overflow’s New York Data Center](http://blog.serverfault.com/post/1432571770/) - <span style="font-family: 'Trebuchet MS', Arial, 'Bitstream Vera Sans', sans-serif; line-height: 17px; color: #000000;">hardware of the various machines?</span>
*   [Designing For Scalability of Management and Fault Tolerance](http://blog.serverfault.com/post/1097492931/)
*   [Stack Overflow Blog](http://blog.stackoverflow.com/)
*   [Stack Overflow Search — Now 81% Less Crappy](http://blog.stackoverflow.com/2011/01/stack-overflow-search-now-81-less-crappy/) - Lucene is now running on an underused cluster.
*   [State of the Stack 2010 (a message from your CEO)](http://blog.stackoverflow.com/2011/01/state-of-the-stack-2010-a-message-from-your-ceo/)
*   [Stack Overflow Network Configuration](http://blog.stackoverflow.com/2010/01/stack-overflow-network-configuration/)
*   [Does StackOverflow use caching and if so, how?](http://meta.stackoverflow.com/questions/69164/does-stackoverflow-use-caching-and-if-so-how)
*   [Meta StackOverflow](http://meta.stackoverflow.com/)
*   [How does StackOverflow handle cache invalidation?](http://meta.stackoverflow.com/questions/6435/how-does-stackoverflow-handle-cache-invalidation)
*   [Which tools and technologies build the Stack Exchange Network?](http://meta.stackoverflow.com/questions/10369/which-tools-and-technologies-build-the-stack-exchange-network)
*   [How does Stack Overflow handle spam?](http://meta.stackoverflow.com/questions/2765/how-does-stack-overflow-handle-spam)
*   [Our Storage Decision](http://blog.serverfault.com/post/our-storage-decision/)
*   [How are “Hot” Questions Selected?](http://meta.stackoverflow.com/questions/4766/how-are-hot-questions-selected) 
*   [How are “related” questions selected?](http://meta.stackoverflow.com/questions/20473/how-are-related-questions-selected) - the title, the question body, and the tags. 
*   [Stack Overflow and DVCS](http://blog.stackoverflow.com/2010/04/stack-overflow-and-dvcs/) - Stack Overflow selects Mercurial for source code control.
*   [Server Fault Chat Room](http://chat.stackexchange.com/rooms/127/the-comms-room) 
*   [C# Redis Client](https://github.com/ServiceStack/ServiceStack.Redis)
*   [Broadcom, Die Mutha](http://blog.serverfault.com/post/broadcom-die-mutha/)

</div>