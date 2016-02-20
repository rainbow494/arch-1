## [Scaling DISQUS to 75 Million Comments and 17,000 RPS](/blog/2010/10/26/scaling-disqus-to-75-million-comments-and-17000-rps.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, October 26, 2010 at 8:38AM</span></div>

<div class="body">

This [presentation](http://www.slideshare.net/zeeg/djangocon-2010-scaling-disqus) and [video](http://python.mirocommunity.org/video/1886/djangocon-2010-scaling-the-wor) by Jason Yan and David Cramer discusses how they scaled DISQUS, a comments as a service service for easily adding comments to your site and connecting communities. The presentation is very good, so here are just a few highlights: 

*   **Traffic**: 17,000 requests/second peak; 450,000 websites; 15 million profiles; 75 million comments; 250 million visitors; 40 million monthly users / developer.
*   **Forces**: unpredictable traffic patterns because of celebrity gossip and events like disasters; discussion never expire which means they can't fit in memory; must always be up.
*   **Machines**: 100 servers; 30% web servers (Appache + mod_wsgi); 10% databases (PostgreSQL); 25% cache servers (memcached); 20% load balancing / high availability (HAProxy + heartbeat); 15% Utility servers (Python scripts).
*   **Architecture**: Requests are load balanced across an Apache cluster. Apache talks to memcached, HAProxy/pgbouncer to handle connection pooling to the database, and a central queue service. 
*   **Strategies**: make sure indexes fit in memory; log slow queries; use connection pooling; the data model consists of user, forum, thread, post; partitions horizontally (Disqus, Your blog, etc) and vertically (forums, posts, users, sentry) at application level; joins performed in Python; Hudson is used for continuous integration; Redmine is used for bug tracking; extensive test suite; feature switches are used to turn off features; isolate slow functions from transactions; use autocommit for read slaves; a queue is used for low priority tasks; Django QuerySet caching is turned off to save memory.

</div>