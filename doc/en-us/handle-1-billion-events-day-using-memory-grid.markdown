## [Handle 1 Billion Events Per Day Using a Memory Grid](/blog/2009/2/16/handle-1-billion-events-per-day-using-a-memory-grid.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, February 16, 2009 at 1:58AM</span></div>

<div class="body">

Moshe Kaplan of RockeTier [shows the life cycle of an affiliate marketing system](http://www.rocketier.com/banner_network_case_study.html) that starts off as a cub handling one million events per day and ends up a lion handling 200 million to even one billion events per day. The resulting system uses ten commodity servers at a cost of $35,000\.  

Mr. Kaplan's paper is especially interesting because it documents a system architecture evolution we may see a lot more of in the future: **database centric --> cache centric --> memory grid**.  

As scaling and performance requirements for complicated operations increase, leaving the entire system in memory starts to make a great deal of sense. Why use cache at all? Why shouldn't your system be all in memory from the start?

## General Approach to Evolving the System to Scale

*   Analyze the system architecture and the main business processes. Detect the main hardware bottlenecks and the related business process causing them. Focus efforts on points of greatest return.*   Rate the bottlenecks by importance and provide immediate and practical recommendation to improve performance.*   Implement the recommendations to provide immediate relief to problems. Risk is reduced by avoiding a full rewrite and spending a fortune on more resources.*   Plan a road map for meeting next generation solutions.*   Scale up and scale out when redesign is necessary.  

    ## One Million Event Per Day System

    *   The events are common advertising system operations like: ad impressions, clicks, and sales.*   Typical two tier system. Impressions and banner sales are written directly to the database.*   The immediate goal was to process 2.5 million events per day so something needed to be done.  

    ## 2.5 Million Event Per Day System

    *   PerfMon was used to check web server and DB performance counters. CPU usage was at 100% at peak usage.*   Immediate fixes included: tuning SQL queries, implementing stored procedures, using a PHP compiler, removing include files and fixing other programming errors.*   The changes successfully double the performance of the system within 3 months. The next goal was to handle 20 million events per day.  

    ## 20 Million Event Per Day System

    *   To make this scaling leap a rethinking of how the system worked was in order.*   The main load of the system was validating inputs in order to prevent forgery.*   A cache was maintained in the application servers to cut unnecessary database access. The result was 50% reduction in CPU utilization.*   An in-memory database was used to accumulate transactions over time (impression counting, clicks, sales recording).*   A periodic process was used to write transactions from the in-memory database to the database server.*   This architecture could handle 20 million events using existing hardware.*   Business projections required a system that could handle 200 million events.  

    ## 200 Million Event Per Day System

    *   The next architectural evolution was to a scale out grid product. It's not mentioned in the paper but I think [GigaSpaces](http://natishalom.typepad.com/nati_shaloms_blog/2008/05/twitter-as-an-e.html) was used.*   A Layer 7 load balancer is used to route requests to sharded application servers. Each app server supports a different set of banners.*   Data is still stored in the database as the data is used for statistics, reports, billing, fraud detection and so on.*   Latency was slashed because logic was separated out of the HTTP request/response loop into a separate process and database persistence is done offline.  

    At this point architecture supports near-linear scaling and it's projected that it can easily scale to a billion events per day.  

    ## Related Articles

    *   [GridGain: One Compute Grid, Many Data Grids](http://highscalability.com/gridgain-one-compute-grid-many-data-grids)</div>