## [Six Lessons Learned the Hard Way About Scaling a Million User System ](/blog/2014/4/16/six-lessons-learned-the-hard-way-about-scaling-a-million-use.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, April 16, 2014 at 8:56AM</span></div>

<div class="body">

![](https://farm8.staticflickr.com/7100/13895290294_ec9598f752_m.jpg)

Ever come to a point where you feel you've learned enough to share your experiences in the hopes of helping others traveling the same road? That's what [Martin Kleppmann](https://twitter.com/martinkl) has done in an lovingly written [Six things I wish we had known about scaling](http://martin.kleppmann.com/2014/03/26/six-things-about-scaling.html), an article well worth your time.

It's not advice about scaling a Twitter, but of building a million user system, which is the sweet spot for a lot of projects. His conclusion rings true:

> Building scalable systems is not all sexy roflscale fun. It’s a lot of plumbing and yak shaving. A lot of hacking together tools that really ought to exist already, but all the open source solutions out there are too bad (and yours ends up bad too, but at least it solves your particular problem).

Here's a gloss on the six lessons (plus a bonus lesson):

1.  **Realistic load testing is hard**. Testing a large distributed system is not like a scientific experiment that can be conducted under ideal conditions. This is hard for the scientific minded to accept. Knowing your actual access patterns is hard. Testing with synthetic data sets larger than you actually have is hard. Comparing the correctness of the old to a new system is hard. So be prepared to roll-back if new code doesn't work in practice.
2.  **Data evolution is difficult**. You have data all over the place. In your databases, logs, and in blobs. Updating data formats as they change is an enormous time sink. Larger companies often have an advantage in automating and orchestrating these processes. 
3.  **Database connections are a real limitation**. Database connections add up surprisingly fast as a system grows in both service and node counts. Each connection eats resources both on your machines and with your developers as they have to figure out how to deal with them. Use a connection pooler or write a data access layer wrapping database access behind an API.
4.  **Read replicas are an operational pain**. Read replicas to offload database access from the master is a common scaling strategy. It also takes a lot of work to setup and maintain these systems. Failure handling is a constant source of problems. 
5.  **Think about memory efficiency**. Latency spikes are often caused by memory problems. Using RAM more efficiently can be difficult because it's hard to figure out how RAM is actually being used. Many performance problems are solved by buying more RAM. Fit indexes into RAM if possible. Index on the hash of string instead of the string itself.
6.  **Change capture is under-appreciated**. As data changes in a system it must flow through many services like a database, search index, graph, index, read replicas, cache invalidation, etc. You could have applications write to multiple locations every time they make updates, but that never works in practice. You could have apps read database logs, but this isn't possible on every system. A good solution is to use a [Change Capture System](https://github.com/linkedin/databus) that receives and stores all the writes to the database. Applications can receive these updates in real-time and/or they can stream through a history of changes. The change capture system becomes the single source of truth for data for all applications. A big advantage of this approach is that data producers and consumers are decoupled which gives "you great freedom to experiment without fear of bringing down the main site." 
7.  **Cache and cache invalidation**. This is a bonus lesson from a comment by [mysteriousllama](https://news.ycombinator.com/item?id=7477146) on the article: Without proper caching and a good invalidation strategy your databases will get pounded. Use redis and memcache to cache everything possible. Don't even connect to the database unless you have to. Ensure that you can invalidate any cache entry easily and keep things atomic so you do not run in to race conditions. Use locking to ensure that when the cache expires the database does not get a dog-pile with multiple copies of the same query. You'd think the query-cache in your database of choice may be just as efficient but trust me, it is not even close. You can also cache higher-level objects than just simple queries.

    <div id="_mcePaste">Depending on your reliability requirements you may even consider treating your cache as writeback and doing batched database writes in the background. These are generally more efficient than individual writes due to a variety of factors. I've worked on several top-200 ranking sites and this has always been one of the main go-to strategies for scaling. Databases suck - Avoid querying them.</div>

</div>