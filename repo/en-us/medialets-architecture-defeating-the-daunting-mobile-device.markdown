## [Medialets Architecture - Defeating the Daunting Mobile Device Data Deluge](/blog/2011/3/8/medialets-architecture-defeating-the-daunting-mobile-device.html)

    

    

![](http://farm6.static.flickr.com/5293/5509030387_42b1682df0_o.jpg)

Mobile developers have a huge scaling problem ahead: doing something useful with massive continuous streams of telemetry data from millions and millions of devices. This is a really good problem to have. It means smartphone sales are finally fulfilling their destiny: [slaughtering PCs](http://www.mobiledia.com/news/81680.html) in the sales arena. And it also means mobile devices aren't just containers for simple standalone apps anymore, they are becoming the dominant interface to giant backend systems.

While developers are now rocking mobile development on the client side, their next challenge is how to code those tricky backend bits. A company facing those same exact problems right now is [Medialets](http://www.medialets.com/), a mobile rich media ad platform. What they do is help publishers create high quality interactive ads, though for our purposes their ad stuff isn't that interesting. What I did find really interesting about their system is how they are tackling the problem of defeating the mobile device data deluge.

Each day Medialets munches on billions of new objects embedded in a stream of terabytes of raw event data flowing in from millions of mobile devices. All that data must be: generated on the mobile device; transmitted over lossy connections punctuated by long periods of disconnection; crunched; made available to reporting systems; fed back into control systems that must be able to respond within milliseconds to requests. 

This will become a common paradigm for systems featuring mobile devices. The question is, how you can make it happen?

Now that's interesting.

To help us understand more about how Medialets works, Joe Stein, Engineering Manager for Server Platforms at Medialets, was kind enough to talk to me about what they are doing. Joe also runs an excellent Hadoop podcast and blog called [All Things Hadoop](http://allthingshadoop.com/), check it out.

Joe has worked on this problem a lot and has some great ideas on how to build an effective mobile data cruncher using tools like Hadoop, MySQL, HBase, Cassandra, Ruby, Python, and Java...

## Site

*   [http://www.medialets.com](http://www.medialets.com/) - home page.
*   [http://www.medialytics.com](http://www.medialytics.com/) - insights dashboard for analyzing events from applications.

## What is Medialets?

Medialets delivers rich media advertising to mobile devices like the iPhone, iPad, and Android. Rich media means advertisements can be complex applications that embed, using a SDK, event generation and advertising functionality. The idea is that ads run inside the platform and instead of being lame adsense type ads, these can be fully interactive while providing the same brand quality they have on TV, except it's on the mobile device. Applications can have you do things like shake the device or play a little football game with Michael Strahan. All this activity generates data that must be streamed back to their server farm for processing.

In addition to advertising they also provide very elaborate [app based analytics](http://www.medialytics.com/) for their publishers.  

To see examples of their ads [go here](http://www.medialets.com/showcase). 

## The Stats

*   2-3 TB of new data every day (uncompressed). 
*   Tens of billions of analytic events created per day. An event means somebody shook, turned off, rotated, etc. the app. 
*   200+ Premium Applications run on tens of millions of mobile devices (iPhone, iPad, and Android).
*   Over 700 billion analytics events already processed.
*   During typical MapReduce jobs the can crunch on over 1 million events per second
*   Data is often available in [www.medialytics.com](http://www.medialytics.com/) within 1 hour of coming in from mobile devices
*   Approximately 100 servers total.
*   Mobile is growing. Smartphone [sales have outpaced PC sales](http://www.mobiledia.com/news/81680.html) for the first time and Medialets is seeing that in their growth. iPhone, iPad and Android [devices increased nearly](http://www.medialets.com/medialets-data-spotlight-mobile-rich-media-momentum-q4-2011/) 300% in fourth quarter 2010\. Android continues to grow market share, but iOS still dominates for premium mobile inventory.
*   A dozen people are in engineering, mostly on the client side and Medialytics. The infrastructure team is 1-2 people.

## Infrastructure

*   Ad Servers instances running on quad core x 16GB w/ 1TB
*   Event Tracking Servers  on 16 core x 12 GB w/ 10TB 
*   Event Processing, Job Execution, Log Collection & Aggregate Pre-Processing each 16 core 24GB RAM w/ 6TB of space
*   Log Collection & Aggregate Pre-Processing and Post-Processing. Hadoop Cluster nodes each 16 core x 12GB RAM w/ 2x2TB (JBOD).
*   [www.medialytics.com](http://www.medialytics.com/) varying configurations all with 16 cores from 12 to 96GB of RAM w/ 1-2TB

## Open Source Systems & Tools In Production

*   Linux
*   Haproxy
*   Tomcat
*   MySQL
*   Apache
*   Passenger
*   RoR
*   Memcached
*   Gearman
*   Hadoop
*   Pig 
*   Nagios
*   Ganglia
*   Git 
*   Jira 

## Languages

*   C/C++ - ad server
*   Java - data transformation
*   Ruby - a lot server side Ruby. Probably more Ruby than C++, but moving more to Python.
*   Python - a lot of MapReduce is being moved into Python using [Python streaming](http://allthingshadoop.com/2010/12/16/simple-hadoop-streaming-tutorial-using-joins-and-keys-with-python/).
*   Scala
*   Bash

## Architecture

*   The system was built over a couple years, mostly out of custom software, though they use Hadoop for the heavy lifting on the analytics side and MySQL as the database. Custom software was required to scale when they first started, but they are now considering more off-the-shelf products as these have evolved.
*   The system is real-time in the sense that as data trickles in it's made available as fast possible and as real as possible in reports.
*   Not cloud based. They run on physical hardware. They couldn't do what they need to do in a multitenancy environment in the cloud. Machines with 16 cores and terabytes of disk space is almost not enough.  They spend a lot of  time building their software to take advantage of the hardware, disk IO, CPU, parallel processing, utilizing as much memory as they can.
*   Everything (almost) they do is asynchronous. You don't need to be connected to a network to see an ad or for them to capture the analytics. Ads can be flighted (scheduled) weeks before a campaign kicks off and you can be on a subway and you'll still be able to see the ad. Data collected during the ad is eventually passed to the server side.
*   Publishers are responsible for building the app and integrate in the Medialets SDK. The SDK is specifically for analytics and advertisements. When they want run analytics or run ads in ad slots they use Medialets' tools. 
*   There are three basic subsytems: ad serving, event processing, and reporting. 
*   Servers break down into a few tiers:
    *   Forward facing tiers:
        *   Ad Serving Tier - designed just to run ad servers. 
        *   Tracking Tier - handles data dumps and data loads.
    *   Asynchronous processing tiers:
        *   Hadoop Cluster -  runs on its own set of servers.
        *   Java and Ruby Processes - take data that comes in and turns the data into a form usable by Hadoop to get the different aggregations going. 
*   Keep things as lean and mean as possible with only as much redundancy as needed.
    *   Hardware is configured based on what the software is doing. Not all the machines are high end or low end. Different parts of the system have different hardware needs.
    *   When responding to ad service requests there's very little disk IO or computation. There's a C++ service doing a lookup on static memory. So the ad serving machines have a lot of memory and lower end processors.
    *   Where they have data intake they have a lot of sockets blocking and writing to disk, so they need very little memory.
    *   The Hadoop MapReduce tier needs everything you can throw at it.
*   The event handling flow goes something like:
    *   Applications on mobile devices generate events. There are application events, ad events, and custom events. Custom events can be created by applications as key-value pairs and they'll aggregate on that with no custom coding required.
    *   Tracking servers receive events and write them into a log file of objects. The files represent a snap shot of time over which event data is collected, say seven minutes, for example.
    *   Java servers read these files, unpack them, and run the objects through a series of thread pools so they are transformed and merged with all the necessary meta-data. Different processes pick up the different event types and process them.
    *   The previous step creates a new file where the events are now complete after having been merged with meta-data. That new file is pushed into HDFS (the file system used by Hadoop).
    *   MapReduce is run on these HDFS files to dedupe the data. After the deduplication occurs the data set is clean.
    *   MapReduce jobs produce data that is then imported into databases that the analytics UI makes available to customers. Dozens of different types of aggregations are run. Standard metrics are provided, via Medialytics, showing what an app is doing from from both an ad and app reporting perspective. They aggregate along many different dimensions, for example, by device and by platform. Some of this data is feedback to the ad serving systems.
*   Data duplication is a key design point on mobile devices and asynchronous systems. 
    *   A phone OS doesn't guarantee an application it will get time to generate events. In the OnClose hook the publisher will run some cleanup logic, Medialets has some cleanup logic, then the event is written to a server which takes a couple milliseconds to respond, and then the local app has to update the local database. Even though this is a quick trip you are in this 15 millisecond window where the OS doesn't guarantee all the functionality will execute. The OS could kill the process or there's a crash. This will lead to duplicate events on replay depending on where the failure occurs. The iPhone's new background feature complicates the accounting. If an app is backgrounded for 30 seconds or less it's still considered the same run. There are a lot of variations.
    *   Data can still come in from a phone that's been off for 3 months. They have to be able know if they've seen that data before, a common occurrence if apps aren't used all the time.
    *   Hadoop is used to calculate metrics and aggregation, but it is also used for deduplication. When processing data they look at over a million events per second in order to dedupe the data. Everyday they get 10 billion events. They have to look at every event and decide if 1) they have seen the event before; 2) are they going to see the event again in the data set they are processing. Using MapReduce that data is broken up across a bunch of servers, which means you don't really know until you've reduced everything that any of the data was duplicated.
*   Advertising
    *   Some types of event data are streamed back in real-time. For another class of data the event has to be opened and closed before an event is created. To know how many times someone used an app in a day, for example, there must be an open and a corresponding close event. 
    *   Users in the mobile world are truly unique. In the Internet world users, unless they have logged in, are almost impossible to quantify. A computer is used by many people and you can't really identify who is using a device. A physical device like the phone is usually used by a single person, so aggregating on a device basis is essentially aggregating on a user basis. 
    *   They can gather stats like conversions along different dimensions. They can tell how long it takes someone to upgrade from version of the OS to another. Who are the different types of people? Then they can overlay that to the different types of apps that they have. 
    *   Ads are either brand or direct response advertisements, there's a mix. A movie ad, for example, when clicked on instead of going to a website could bring up a local application from the device. That allows you to buy the tickets right in the app. Having different interceptors and the capability of creating a rich user experience makes it possible to monetize interaction flows in new ways.
    *   When an app is launched an ad should be displayed immediately. Async is used for pushing data to the servers, but ads can be served synchronously. A lot of apps have a sponsor logo you see when the app opens and that must be served immediately. The first onload call is a sync call to get the ads. Their ad serving system can serve ads in under 200 milliseconds with a 3% variance. 
*   70%-80% of their storage costs are saved because they store data in [compressed sequence files](http://blog.mgm-tp.com/2010/04/hadoop-log-management-part2/).
    *   HDFS has a sequence file format. Compression can be either on a row or a block basis. Let's say you have a sequence file with 10 blocks in it and a block is defined as data that will go into your mapper (for MapReduce). If compression is on a block level and there are 10 blocks then those 10 blocks can be pushed to mappers in parallel and HDFS will handle all the decompression and streaming automatically. 
    *   Data that comes out of the reducer can be stored in the sequence file, say it's the result of the deduplication process, or it can be stored in an uncompressed format that's ready to load in another database.
    *   A lot of people leave the data uncompressed. To use Hive the data has to be uncompressed so you can interact with it. It's a matter of how much do you want to spend and where do you want the inefficiencies of your system to come into play. 
    *   It's a con to have a lot of data sitting on disk that isn't being interacting with in an uncompressed format. They selectively decide which format to use depending on if the decompression phase is worth the overhead compared to how long they keep that data on disk. 
*   Job Execution System
    *   Built in Ruby, before suitable off-the-shelf systems were available.
    *   They built a job processing system to implement workflow processing. A workflow is a set of jobs that have different tasks and steps and operate on different events. App data must be processed in many different ways and the results have to go into a few different systems, some into tables, and some into other reporting systems. All that is automated and scripted. 
*   Aggregated data is stored in MySQL for viewing by publishers and advertisers. They are reaching a limit of how much they can shard. They are looking at MongoDB and GridFs (which is part of MongoDB).
    *   GridFs is looking good to store, scale, and serve their media files, which is making them consider using MondoDB to store aggregate results sets.
    *   They are also looking at Cassandra and HBase to store their aggregate results sets. They would consider using the same infrastructure also for their tracking and event capture servers, which currently is completely custom written. 
    *   Cassandra looks attractive because it works across multiple datacenters. They would use this feature to have multiple clusters in the same dacenter and have writes occur on one cluster and reads in another, so the different traffic loads won't step on each other. They don't want to mix different types of traffic, so they don't want to do MapReduce jobs, writing from HBase, and reading from HBase all on the same machines. 
    *   HBase is an attractive option because they already write so much data to HDFS that having those files available in HBase would be exciting. They've had reliability concerns over fsync, making sure data is written to disk, but those concerns have been addressed in recent releases. HBase doesn't allow partitioning the data by different uses, which is what is attractive about Cassandra as Cassandra supports having different kinds of racks within the cluster. 
    *   Since they are already using HDFS moving all the data into Cassandra after it is processed isn't attractive, that would double their hard drive requirements.
    *   They like the idea of the [coprocessors](http://highscalability.com/blog/2010/11/1/hot-trend-move-behavior-to-data-for-a-new-interactive-applic.html) so they don't have to move the data across the network. Each job is 2-3 terabytes so to truly parallelize that without moving data is very attractive. 
    *   [TTL deletes](http://www.quora.com/Which-NoSQL-stores-have-good-support-for-LRU-or-TTL-semantics-in-storage) in Cassandra are very attractive. Cassandra can easily handle their write load, so it can be used to store incoming events. Then all the work processes can take the mobile data out of Cassandra, merge it with meta-data from other databases, to make for a fully joined objects, then that could be written to HBase, then they could do MapReduce aggregations and write the results to MongoDB.
    *   An alternative dedupe design would be to write it all to HBase and just pick the last one as the winner. Once these systems are in-place they'll rethink some of their existing processes to see how they could take advantage.
    *   A lot of prototyping trying was undertaken to figure out if they should keep with their existing software, move to another database, or stay with MySQL. They may end up with MongoDB, Cassandra, and HBase, they just want to find the right mix of capabilities for the new products they are building, and figure out how they can continue to scale without soaking up a lot of developer time.
*   AdServers are written in C++
    *   This layer provides a standard ad scheduling facility so ads can be mapped to slots, rotated, targeted, etc. Targeting can be based on platform, resolution, geography and other dimensions.
    *   There's an object cache for database data that is used to make ad delivery decisions.
    *   99% of the time the cache is sufficient, 1% of the time they have to hit the database.
    *   Reads are in a few microseconds, but increase to 200 microseconds when they have to hit the database.
    *   Also responsible for pacing ads so a campaign can be delivered across the lifetime of an app. If an app is run a million times a day, for example, and the ad campaign is for a million impressions, they don't want to use it up in one day. Let's say the advertiser wants the campaign to run for a month. The ad server looks at the analytic data and the rate requests are coming in to calculate what pace ads should be delivered.
    *   A lot of decisions are precomputed. A human will slot target, saying where an add should be displayed. 
    *   Decisions like geographical distribution are calculated on the fly. If you have a set of ad impressions to give out you'll want some to go to Canada and some to go the US, for example.
*   Java Servers
    *   Join meta-data with the data sets that come in. Meta-data is pulled from cache at 95% hit rate. When they have a new advertisement that went live, for example, they'll hit the database.
    *   Going to the database they want to be as optimistic as possible and interrupt as few as threads as possible, so they use atomic variables and CAS (compare and set) when doing very heavy read operations and very few writes. This switch increased performance 15%-20% because they are no longer blocking on writes.
    *   For the amount of reads they were doing they benchmarked it and found a Mutex took to long. Semaphores ended up blocking on the writer. Say there were 10 threads and 9 could read so no threads would block, but the 10th thread had to write which would block all the threads. This increased latency and didn't perform well compared to looping and doing the compare and set. It's possible because they are continually processing gigabytes of data that inside the JVM something was being blocked.
    *   Used [concurrent memoizer pattern](http://javathink.blogspot.com/2008/09/what-is-memoizer-and-why-should-you.html) to create thread pools that handle cache requests. The cache is a pool that will load data when required. The load uses CAS to block the actual reads that are occurring.
    *   [SEDA](http://www.eecs.harvard.edu/~mdw/proj/seda/) is used to process data through all it's different transformations. Each thread pool performs a state transformation on a chunk of data and then the data is forwarded onto another thread pool for the next transformation. For example, the first stage is to read the data off disk and serializing it onto an object array. These operations are not latency sensitive.
*   Using Ruby
    *   When using Ruby one must fork to really multiprocess functions effectively.
    *   [Rinda](http://en.wikipedia.org/wiki/Rinda_(Ruby_programming_language)) is used to create concurrencies across forked processes. Sometimes a database is used for coordination.
    *   This hides any memory leak problems or green thread problems common to interpreters.
*   Monitoring
    *   Mix of internal tools and Nagios.
    *   Do a lot of trending of their own logs with Ganglia across all their different tiers.
    *   They take a very proactive monitoring approach and spend a lot of R&D effort so they can know they have problems before they occur.
    *   Trending feeds into their monitoring. If one of their ad servers stops responding within 10 milliseconds, for more than 1 or 2 requests, within a second, they need to know about that.
    *   If request latencies go up from 200 milliseconds to 800 milliseconds on average they want to know. 
    *   They log a lot so debugging of problems occurs through the logs.

## Lessons Learned

*   ****Turn Data into Products**.** Development knows the data they have. That knowledge can be used to help the product team create new products to sell to customers. There's always a big gap between R&D and the business. Help the business be in tune with what R&D is doing. Have them understand the power of Hadoop, the data they crunch and how fast they can crunch it. A good example is a new Conversion Attribution product. If a user sees a static ad, clicks on it and downloads the app, it's likely that static ad wasn't the sole reason for the conversion. If, for example, the user experienced a rich media ad the day before (or two weeks earlier) then that ad would be apportioned some credit for the conversion, based on publisher configurable criteria. This kind of capability is only possible with a robust data processing infrastructure. Without R&D making it known that this sort of feature is even possible, it's not likely these new kinds  of high value-add products could be developed and sold to customers. 
*   **Explore New Tools**. It's a complex world of new tools. All these new technologies make it challenging to know where to put functionality. A feature can be done in so many different ways and would be done differently depending on the tool (HBase, Cassandra, MongoDB). Should the dedupe still be done in MapReduce or should it be done using coprocessors? Is it worth doubling disk by supporting two different databases? Is partitioning data by usage pattern really or win or can all the traffic patterns work on the same system? Prototype your options and think about how your architecture could change with each new tool, working alone or working together.
*   **Monitor and Capacity Plan Proactively**. Turn monitoring data around into planning for infrastructure. If you don't do that you'll just keep firefighting issues. Build your proactive alerts and trending data so even before it becomes a warning you can see it coming. Sometimes you just need another server. More data simply means more servers. It's a cost of doing business.  Knowing what server to put where in order to keep all the different dataflows going appropriately, that's the trick. It's important to compare the trends of CPU and load and to really look at the infrastructure as a whole and create a plan based on that.
*   **Look at Data from a Product Operations Perspective**. Look at new applications and see how they are similar to what has been implemented before as a way to figure out how you need to scale. Just because a new app comes on board doesn't mean need new ad servers, new data nodes, and new tracking servers need to be added. It depends. A lot of different ads make a small amount of ad requests but send a ridiculous amount of data. Trend logs to look at spikes in the data and see how latency is related to the spikes. This tells you where and how different parts of the system need to scale .

## Related Articles

*   Joe Stein's [AllThingsHadoop Twitter Feed](http://www.twitter.com/allthingshadoop)
*   [Tackling Big Data Problems at Scale](http://www.medialets.com/tackling-big-data-problems-at-scale/) by Joe Stein
*   Published Q4 2010 mobile figures [http://www.medialets.com/medialets-data-spotlight-mobile-rich-media-momentum-q4-2011/](http://www.medialets.com/medialets-data-spotlight-mobile-rich-media-momentum-q4-2011/)
*   [Hadoop, BigData and Cassandra with Jonathan Ellis](http://allthingshadoop.com/2010/05/17/hadoop-bigdata-cassandra-a-talk-with-jonathan-ellis/)  - HBase is to OLAP as Cassandra is to OLTP
*   [Twitter’s Plan To Analyze 100 Billion Tweets](http://highscalability.com/blog/2010/2/19/twitters-plan-to-analyze-100-billion-tweets.html)

    