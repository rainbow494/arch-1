## [The 4 Building Blocks of Architecting Systems for Scale](/blog/2012/9/19/the-4-building-blocks-of-architecting-systems-for-scale.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, September 19, 2012 at 8:32AM</span></div>

<div class="body">

![](http://farm9.staticflickr.com/8316/8003362389_19cb902e0d_m.jpg)

If you are looking for an excellent overview of general architecture principles then take a look at Will Larson's [Introduction to Architecting Systems for Scale](http://lethain.com/introduction-to-architecting-systems-for-scale/). Based on his experiences at Yahoo! and Digg, Will covers key concepts in some depth. A quick gloss on the building blocks:

1.  ****Load Balancing: Scalability & Redundancy**.** Horizontal scalability and redundancy are usually achieved via load balancing, the spreading of requests across multiple resources.
    *   ****Smart Clients**.** Theclient has a list of hosts and load balances across that list of hosts. Upside is simple for programmers. Downside is it's hard to update and change.
    *   ****Hardware Load Balancers**.** Targeted at larger companies, this is dedicated load balancing hardware. Upside is performance. Downside is cost and complexity.
    *   ****Software Load Balancers**.** The recommended approach, it's  software that handles load balancing, health checks, etc.
2.  ****Caching**.** Make better use of resources you already have. Precalculate results for later use. 
    1.  ****Application Versus Database Caching**.** Databases caching is simple because the programmer doesn't have to do it. Application caching requires explicit integration into the application code.
    2.  ****In Memory Caches**.** Performs best but you usually have more disk than RAM.
    3.  ****Content Distribution Networks**.** Movesthe burden of serving static resources from your application and moves into a specialized distributed caching service.
    4.  ****Cache Invalidation**.** Caching is great but the problem is you have to practice safe cache invalidation. 
3.  ****Off-Line Processing**.** Processing that doesn't happen in-line with a web requests. Reduces latency and/or handles batch processing. 
    1.  ****Message Queues**.** Work is queued to a cluster of agents to be processed in parallel.
    2.  ****Scheduling Periodic Tasks**.** Triggers daily, hourly, or other regular system tasks. 
    3.  ****Map-Reduce**.** When your system becomes too large for ad hoc queries then move to using a specialized data processing infrastructure.
4.  ****Platform Layer**.** Disconnect application code from web servers, load balancers, and databases using a service level API. This makes it easier to add new resources, reuse infrastructure between projects, and scale a growing organization. 

</div>