## [MySpace Architecture](/blog/2009/2/12/myspace-architecture.html)

    

    

**Update:**[Presentation: Behind the Scenes at MySpace.com](http://www.infoq.com/news/2009/02/MySpace-Dan-Farino). Dan Farino, Chief Systems Architect at MySpace shares details of some of MySpace's cool internal operations tools.  

MySpace.com is one of the fastest growing site on the Internet with 65 million subscribers and 260,000 new users registering each day. Often criticized for poor performance, MySpace has had to tackle scalability issues few other sites have faced. How did they do it?

Site: http://myspace.com

## Information Sources

*   [Presentation: Behind the Scenes at MySpace.com](http://www.infoq.com/news/2009/02/MySpace-Dan-Farino)*   [Inside MySpace.com](http://www.baselinemag.com/print_article2/0,1217,a=198614,00.asp)  

    ## Platform

    *   ASP.NET 2.0*   Windows*   IIS*   SQL Server  

    ## What's Inside?

    *   300 million users.*   Pushes 100 gigabits/second to the internet. 10Gb/sec is HTML content.*   4,500+ web servers windows 2003/IIS 6.0/APS.NET.*   1,200+ cache servers running 64-bit Windows 2003\. 16GB of objects cached in RAM.*   500+ database servers running 64-bit Windows and SQL Server 2005.  

    *   MySpace processes 1.5 Billion page views per day and handles 2.3 million concurrent users during the day*   Membership Milestones:  
    - 500,000 Users: A Simple Architecture Stumbles  
    - 1 Million Users:Vertical Partitioning Solves Scalability Woes  
    - 3 Million Users: Scale-Out Wins Over Scale-Up  
    - 9 Million Users: Site Migrates to ASP.NET, Adds Virtual Storage  
    - 26 Million Users: MySpace Embraces 64-Bit Technology*   500,000 accounts was too much load for two web servers and a single database.*   At 1-2 Million Accounts  
    - They used a database architecture built around the concept of vertical partitioning, with separate databases for parts of the website that served different functions such as the log-in screen, user profiles and blogs.  
    - The vertical partitioning scheme helped divide up the workload for database reads and writes alike, and when users demanded a new feature, MySpace would put a new database online to support it.  
    - MySpace switched from using storage devices directly attached to its database servers to a storage area network (SAN), in which a pool of disk storage devices are tied together by a high-speed, specialized network, and the databases connect to the SAN. The change to a SAN boosted performance, uptime and reliability.*   At 3 Million Accounts  
    - the vertical partitioning solution didn't last because they replicated some horizontal information like user accounts across all vertical slices. With so many replications one would fail and slow down the system.  
    - individual applications like blogs on sub-sections of the Web site would grow too large for a single database server  
    - Reorganized all the core data to be logically organized into one database  
    - split its user base into chunks of 1 million accounts and put all the data keyed to those accounts in a separate instance of SQL Server*   9 Million–17 Million Accounts  
    - Moved to ASP.NET which used less resources than their previous architecture. 150 servers running the new code were able to do the same work that had previously required 246.  
    - Saw storage bottlenecks again. Implementing a SAN had solved some early performance problems, but now the Web site's demands were starting to periodically overwhelm the SAN's I/O capacity—the speed with which it could read and write data to and from disk storage.  
    - Hit limits with the 1 million-accounts-per-database division approach as these limits were exceeded.  
    - Moved to a virtualized storage architecture where the entire SAN is treated as one big pool of storage capacity, without requiring that specific disks be dedicated to serving specific applications. MySpace now standardized on equipment from a relatively new SAN vendor, 3PARdata*   Added a caching tier—a layer of servers placed between the Web servers and the database servers whose sole job was to capture copies of frequently accessed data objects in memory and serve them to the Web application without the need for a database lookup.*   26 Million Accounts  
    - Moved to 64-bit SQL server to work around their memory bottleneck issues. Their standard database server configuration uses 64 GB of RAM.*   **Horizontally Federated Database**. Databases are partition by purpose. Have profile, email databases etc. Partition is based on user range. 1 Million users live in each database. So you have Profile1, Profile2 all the way up to Profile300 as they have 300 million users.*   Doesn't use ASP cache because they don't have a high enough hit rate on the front-end. The middle tier cache does have a high hit rate.*   **Failure isolation**. Segment requests into web server by database. Allow only 7 threads per database. So if the database is slow only those threads will slowdown and the traffic in the other threads will flow.  

    ## Operations

    *   **PerfCollector**. Centralized collection of performance data via UDP. More reliable than Windows and allows any client to connect and see stats.*   **Web Based Stack Dump Tool**. Can right-click on a problem server and get stack dump of the .Net managed threads. Used to have to RDC into system and attach a debugger and 1/2 later get an answer. Slow, nonscalable, and tedious. Not just a stack dump, gives a lot of context about what the thread is doing. Troubleshooting is easier because you can see 90 threads are blocked on a database so the database may be down.*   **Web Base Heap Dump Tool**. Dumps all memory allocations. Very useful for developers. Save hours of doing it by hand.*   **Profiler**. Traces a request from start to finish and produces a report. See URL, methods, status, everything that will help you identify a slow request. Looks at lock contentions, are a lot of exceptions being thrown, anything that might be interesting. Very light weight. It's running on one box in every VIP (group of 100 servers) in production. Samples 1 thread every 10 seconds. Always tracing in background.*   **Powershell**. Microsoft's new shell that runs in process and pass objects between commands versus parsing text output. MySpace develops a lot of commandlets to support operations.*   Developed their own asynchronous communication technology to get around windows networking problems and treat servers as a group. Can ship a .cs file, compile it, run it, and ship the response back.*   **Codespew**. Pushes code updates on their communication technology. Used to do 5 code pushes a day, now down to 1 a week.  

    ## Lessons Learned

    *   You can build big websites using Microsoft tech.*   A cache should have been used from the beginning.*   The cache is a better place to store transitory data that doesn't need to be recorded in a database, such as temporary files created to track a particular user's session on the Web site.*   Built in OS features to detect denial of service attacks can cause inexplicable failures.*   Distribute your data to geographically diverse data centers to handle power failures.*   Consider using virtualized storage/clustered file systems from the start. It allows you to massively parallelize IO access while being able to add disk as needed without any reorganization needed.*   Develop tools that work in a production environment. Can't simulate everything in test environment. The scale and variety of uses APIs are put to can't be simulated in QA during testing. Legitimate users and hackers will run into corner cases that weren't hit in testing, though QA will find most of the problems.*   Throw hardware at problems. Easier than changing their backend software to a new way of doing things. The example is they add a new database server for every million users. It might be more efficient to change their approach to more efficiently use the database hardware, but it's easier just to add servers. For now.    