## [ThemBid Architecture](/blog/2007/7/26/thembid-architecture.html)

    

    

ThemBid provides a market where people needing work done broadcast their request and accept bids from people competing for the job. Unlike many of the sites profiled at HighScalability, ThemBid is not in the popular press as often as Paris Hilton. It's not a media darling or a giant of the industry. But what I like is they have a strategy, a point-of-view for building websites and were gracious enough to share very detailed instructions on how to go about building a website. They even delve into actual installation details of the various software packages they use. Anyone can benefit by taking a look at their work.

Site: http://www.thembid.com/

## Information Sources

*   [Build Scalable Web 2.0 Sites with Ubuntu, Symfony, and Lighttpd](http://blog.thembid.com/index.php/2007/04/05/build-scalable-web-20-sites-with-ubuntu-symfony-and-lighttpd/)  

    ## Platform

    *   Linux (Ubuntu)*   Symfony*   Lighttpd*   PHP*   eAccelerator*   Eclipse*   Munin*   AWStats  

    ## What's Inside?

    ### The Stats

    *   Started work in December of 2006 and had a full demo by March 2007.*   One developer/sys admin worked with a part-time graphics designer.*   Targeted a few thousand users after launch.  

    ### The Architecture

    *   **Hardware**. Dual core server with 2GB RAM*   **Storage**. 2 x 36SCSI 10K RPM on RAID1\.*   **Data Center**. They went with with Layeredtech for the managed server because of past positive experiences.*   **Development Environment**. Ubuntu and Eclipse.*   **OS**. They chose the server distribution of Ubuntu because that's what they use on the client side and Ubuntu supports "simpler installation and easier maintenance than typical IT deployments."*   **Web Server**. Lighttpd is used to handle static content and forward the dynamic PHP page requests to FastCGI.*   **Database**. MySQL. When growth is necessary the idea is to move to a master-slave arrangement and them maybe MySQL cluster.*   **Web Framework**. Went with PHP because they knew it and other successful sites like Digg and Yahoo successfully deploy PHP. They chose Symfony as there framework because of its nice documentation and active development community. And Yahoo also uses Symfony. It's a decision that has worked well for them.*   **PHP Cache**. eAccelerator is used to compile and cache PHP scripts.*   **Object and Content Cache**. The plan is to cache a lot of content. For a bid site like theirs this makes sense. Many of the pieces are used over and over again so putting them in memory will speed up the entire system and take pressure off the database and the IO system. Initially the used a SQLite cache on top of of a memory based file system. This choice was because it was supported by Symfony. When a memcached plugin is available they'll try that.*   **Client Side Cache**. Lighttp's mod_expire module is used to prevent Javascript, style sheets, and images that rarely change from being uncessarily redownloaded by the browser.*   **Monitoring**. Munin is used to monitor their resource usage. It's as simple as visiting "yoursite.com/status" to see what's going on.*   **Log Analysis**. AWStats is used to track hits and types of requests. This information can be used to target bottlenecks.  

    *   **Scalability Plan**.  
    - Use Munin to tell when to think about upgrading. When your growth trend will soon cross your resources trend, it's time to do something.  
    - Move MySQL to a separate server. This frees up resources (CPU, disk, memory). What you want to run on this server depend on its capabilities. Maybe run a memcached server on it.  
    - Move to a distributed memory cache using memcached.  
    - Add a MySQL master/slave configuration.  
    - If more webservers are needed us LVS on the front end as a load balancer.  

    *   **Future Directions**. Work on fault tolerance.  

    ## Lessons Learned

    *   **It's possible to create a nice site fairly quickly with just a few people using commonly available low cost tools**. And your system will be solid and powerful. No cut corners.  

    *   **Use feedback** from your system to know what needs optimizing and when it's time to scale.  

    *   **Good documentation and an active community draw people**. These are very attractive qualities for people making decisions about what to use. It's hard to go with a tool chain when it looks like you may get stuck in the future with no way out and no help. If you make tools make them dead easy to understand, learn, use, and deploy.  

    *   **Stick with the familiar**. It may not be optimal, it may not be the best, but it's more important that you get started and make progress. You don't want to delay releasing your site so you can learn a completely different tool chain that may make your life somewhat easier and in some projected future. The future is now.  

    *   **Use what works for other people**. The fact that Yahoo and Digg use PHP is a good recommendation. Certainly PHP is not the only way to build a site, but it does cut your risk level and help you sleep at night. It also means there's an active community that can help you when you have problems.    