## [More Troubles with Caching](/blog/2010/9/30/more-troubles-with-caching.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, September 30, 2010 at 2:52PM</span></div>

<div class="body">

![](http://farm5.static.flickr.com/4133/5040270820_368ba3a193_m.jpg)

As a tasty pairing with [Facebook And Site Failures Caused By Complex, Weakly Interacting, Layered Systems](http://highscalability.com/blog/2010/9/30/facebook-and-site-failures-caused-by-complex-weakly-interact.html), is another excellent tale of caching gone wrong by Peter Zaitsev, in an exciting twin billing: [Cache Miss Storm](http://www.mysqlperformanceblog.com/2010/09/10/cache-miss-storm/) and [More on dangers of the caches](http://www.mysqlperformanceblog.com/2010/09/23/more-on-dangers-of-the-caches/#). This is fascinating case where the cause turned out to be software upgrade that ran long because it had to be rolled back. During the long recovery time many of the cache entries timed out. When the database came back, slam, all the clients queried the database to repopulate the cache and bad things happened to the database. The solution was equally interesting: 

> So the immediate solution to bring the system up was surprisingly simple. We just had to get traffic on the system in stages allowing Memcache to be warmed up. There were no code which would allow to do it on application side so we did it on MySQL side instead. “SET GLOBAL max_connections=20” to limit number of connections to MySQL and so let application to err when it tries to put too much load on MySQL as MySQL load stabilizes increasing number of connections higher until you finally can serve all traffic without problems.

Peter also suggested a few other helpful strategies:

*   Watch frequently accessed cache items as well as cache items which take long to generate in case of cache miss 
*   Optimize Queries 
*   Use a Smarter Cache
*   Include Rollback in Maintenance Window
*   Know your Cold Cache Performance and Behavior 
*   Have a way to increase traffic gradually 
*   Consider Pre-Priming Caches 

</div>