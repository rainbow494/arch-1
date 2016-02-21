## [Scaling Early Stage Startups](/blog/2007/10/28/scaling-early-stage-startups.html)

    

    

Mark Maunder of [No VC Required](http://novcrequired.com)--who advocates not taking VC money lest you be turned into a frog instead of the prince (or princess) you were dreaming of--has an excellent [slide deck](http://novcrequired.com/scalingEarly.ppt) on how to scale an early stage startup. His blog also has some good SEO tips and a very spooky widget showing the geographical location of his readers. Perfect for Halloween! What is Mark's other worldly scaling strategies for startups?

Site: http://novcrequired.com/

## Information Sources

*   [Slides from Seattle Tech Startup Talk](http://novcrequired.com/scalingEarly.ppt).*   [Scaling Early Stage Startups](http://novcrequired.com/2007/scaling-early-stage-startups/) blog post by Mark Maunder.  

    ## The Platform

    *   Linxux*   An ISAM type data store.*   Perl*   [Httperf](http://www.hpl.hp.com/research/linux/httperf) is used for benchmarking.*   [Websitepulse.com](http://websitepulse.com/) is used for perf monitoring.  

    ## The Architecture

    *   Performance matters because being slow could cost you 20% of your revenue. The UIE guys disagree saying this ain't necessarily so. They explain their reasoning in [Usability Tools Podcast: The Truth About Page Download Time](http://www.uie.com/brainsparks/2007/09/24/usability-tools-podcast-the-truth-about-page-download-time/). The idea is: "There was still another surprising finding from our study: a strong correlation between perceived download time and whether users successfully completed their tasks on a site. There was, however, no correlation between actual download time and task success, causing us to discard our original hypothesis. It seems that, when people accomplish what they set out to do on a site, they perceive that site to be fast." So it might be a better use of time to improve the front-end rather than the back-end.  

    *   MySQL was dumped because of performance problems: MySQL didn't handle a high number of writes and deletes on large tables, writes blow away the query cache, large numbers of small tables (over 10,000) are not well supported, uses a lot of memory to cache indexes, maxed out at 200 concurrent read/write queuries per second with over 1 million records.  

    *   For data storage they evolved to a fixed length ISAM like record scheme that allows seeking directly to the data. Still uses file level locking and its benchmarked at 20,000+ concurrent reads/writes/deletes. Considering moving to BerkelyDB which is a very highly performing and is used by many large websites, especially when you primarily need key-value type lookups. I think it might be interesting to store json if a lot of this data ends up being displayed on the web page.  

    *   Moved to httpd.prefork for Perl. That with no keepalive on the application servers uses less RAM and works well.  

    ## Lessons Learned

    *   Configure your DB and web server correctly. MySQL and Apache's memory usage can easily spiral out of control which leads gridingly slow performance as swapping increases. Here are a few resources for helping with [configuration issues](http://www.possibility.com/epowiki/Wiki.jsp?page=VpsConfiguration).  

    *   Serve only the users you care about. Block content theives that crawl your site using a lot of valuable resources for nothing. Monitor the number of content pages they fetch per minute. If a threshold is exceeded and then do a reverse lookup on their IP address and configure your firewall to block them.  

    *   Cache as much DB data and static content as possible. Perl's Cache::FileCache was used to cache DB data and rendered HTML on disk.  

    *   Use two different host names in URLs to enable browser clients to load images in parallele.  

    *   Make content as static as possible Create a separate Image and CSS server to serve the static content. Use keepalives on static content as static content uses little memory per thread/process.  

    *   Leave plenty of spare memory. Spare memory allows Linux to use more memory fore file system caching which increased performance about 20 percent.  

    *   Turn Keepalive off on your dynamic content. Increasing http requests can exhaust the thread and memory resources needed to serve them.  

    *   You may not need a complex RDBMS for accessing data. Consider a lighter weight database BerkelyDB.    