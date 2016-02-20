## [What's your scalability plan?](/blog/2008/2/13/whats-your-scalability-plan.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, February 13, 2008 at 2:38PM</span></div>

<div class="body">How do you plan to scale your system as you reach predictable milestones? This topic came up in another venue and it reminded me about a great comment an Anonymous wrote a while ago and I wanted to make sure that comment didn't get lost.  

The Anonymous scaling plan was relatively simple and direct:  
_My two cents on what I'm using to start a website from scratch using a single server for now. Later, I'll scale out horizontally when the need arises.  

**Phase 1**_  

_*   Single Server, Dual Quad-Core 2.66, 8gb RAM, 500gb Disk Raid 10  
    *   OS: Fedora 8\. You could go with pretty much any Linux though. I like Fedora 8 best for servers.  
    *   Proxy Cache: Varnish - it is way faster than Squid per my own benchmarks. Squid chokes bigtime.  
    *   Web Server: Lighttpd - faster than Apache 2 and easier to configure for me.  
    *   Object Cache: Memcached. Very scalable.  
    *   PHP Cache: APC. Easy to configure and seems to work fine.  
    *   Language: PHP 5 - no bloated frameworks, waste of time for me. You spend too much time trying to figure out the framework instead of getting work done.  
    *   Database - MySQL 5\. I didn't consider Postgres because I've never used it. There are just a lot more tools available for MySQL.  

    **Phase 2**  
    *   Max Ram out to 64 GB, cache everything  

    **Phase 3**  
    *   Buy load balancer + 2 more servers for front end Varnish/Memcached/Lighttpd.  
    *   Use original server as MySQL database server.  

    **Phase 4**  

    Depending on my load & usage patterns, scale out the database horizontally with an additional server. I don't expect the db to be a bottleneck for my website as only metadata info is stored there. I'll mostly be serving images stored on the file system. Possibly separate Varnish / Memcached / Lighttpd tier into separate tiers if necessary. But I'll carefully evaluate the situation at this point and scale out appropriately and use CDN for static content if necessary.  

    **Phase 5**  
    *   Max all servers to 64gb of RAM, cache, cache, cache.  

    **Phase 6**  
    *   If I get this far then I'm a multi-millionaire already so I'll replace all of the above machines with whatever the latest and greatest is at that time and keep scaling out.  

    The important point is that I know how to scale each layer when/if the need arises. I'll scale the individual machines when necessary and scale horizontally too._  

In previous post we also read where [ThemBid](http://www.highscalability.com/thembid-architecture) has a nice simple scalability plan too :  
_*   Use Munin to tell when to think about upgrading. When your growth trend will soon cross your resources trend, it's time to do something.  
    *   Move MySQL to a separate server. This frees up resources (CPU, disk, memory). What you want to run on this server depend on its capabilities. Maybe run a memcached server on it.  
    *   Move to a distributed memory cache using memcached.  
    *   Add a MySQL master/slave configuration.  
    *   If more webservers are needed us LVS on the front end as a load balancer.  
    _  

The great insight ThemBid had was to use monitoring to predict when performance is hitting a limit so you can take action before the world ends. Take a look at [Monitoring](http://highscalability.com/tags/monitoring) for some options. Some examples of how monitoring is used: [Feedburner Architecture](http://www.highscalability.com/feedburner-architecture), [Scaling Early Stage Startups](http://highscalability.com/scaling-early-stage-startups), [Thembid Architecture](http://www.highscalability.com/thembid-architecture), [Flickr Architecture](http://www.highscalability.com/flickr-architecture), [Ebay Architecture](http://highscalability.com/ebay-architecture).  

Most problems are pretty predictable, especially if you read this site. Have a plan in mind for what you want to do when you grow. You don't have to do all now, but make the path easier by starting in the right direction now. You'll also be much less stressed when all hell breaks loose.</div>