## [Design Decisions for Scaling Your High Traffic Feeds](/blog/2013/10/28/design-decisions-for-scaling-your-high-traffic-feeds.html)

    

    

![](http://farm3.static.flickr.com/2661/4188985841_8a7dd5671e.jpg)

_Guest post by Thierry Schellenbach, Founder/CTO of Fashiolista.com, follow @tschellenbach on Twitter and Github_

[Fashiolista](http://www.fashiolista.com/) started out as a hobby project which we built on the side. We had absolutely no idea it would grow into one of the largest online fashion communities. The entire first version took about two weeks to develop and our feed implementation was dead simple. We’ve come a long way since then and I’d like to share our experience with scaling feed systems.

Feeds are a core component of many large startups such as Pinterest, Instagram, Wanelo and Fashiolista. At Fashiolista the feed system powers the [flat feed](http://www.fashiolista.com/feed/?feed_type=F), [aggregated feed](http://www.fashiolista.com/feed/?feed_type=A) and the [notification](http://www.fashiolista.com/my_style/notification/) system. This article will explain the troubles we ran into when scaling our feeds and the design decisions involved with building your own solution. Understanding the basics of how these feed systems work is essential as more and more applications rely on them.

    

Furthermore we’ve open sourced [Feedly](https://github.com/tschellenbach/Feedly), the Python module powering our feeds. Where applicable I’ll reference how to use it to quickly build your own feed solution.

## Introduction to Feeds

The problem of scaling feed systems has been widely discussed, but let me start by clarifying the basics. The solutions are aimed at making a page like your Facebook news feed, Twitter stream or [Fashiolista](http://www.fashiolista.com/) feed work under high traffic conditions. What all these systems have in common is that they show activities by the people you follow. In our case we based the activity object on [this standard](http://activitystrea.ms/specs/atom/1.0/) for activity streams. Examples of activities are “Thierry added an item to a list on Fashiolista” or “Tommaso tweeted a message”.

    There are two strategies for building feed systems:    

    

1.  **Pull**, where the feed is gathered during reads

2.  **Push** where all the feeds are pre computed during the writes.

    

    

Most real live applications will use a combination of these two approaches. The process of pushing an activity to all your followers is called a fanout.

## History & Background

The Feed system at Fashiolista went through three major redesigns. The first version worked on a PostgreSQL database, the second used Redis and the third and current version runs on Cassandra. To give you an understanding of when and why these solutions fall apart I’ll briefly cover a bit of history.

### Part one - The database

Our first setup simply queried a PostgreSQL database and looked something like this

select * from love where user_id in (...)

The most surprising thing was how robust this system was. We passed 1M loves and it kept on working, soon after we hit 5M loves and it still kept on working. Our bet was that it would break after 10M loves, but it just kept on running smoothly. It took some database tweaking but this simple system held up well into ~100M loves and 1M users. Around that time the performance of this solution started to fluctuate. In general it kept on working, but for some users the latency spiked to multiple seconds. After reading many articles on feed design we built the very first version of Feedly with Redis.

### Part two - Redis & Feedly

Our second approach stored a feed for every user in Redis. When you loved an item this activity was fanned out to all your followers. We used a few smart tricks to keep memory usage low, which I’ll cover in the next section. Redis was really easy to setup and maintain. We sharded across several Redis machines using Nydus and used [Sentinel](http://redis.io/topics/sentinel) for automatic failovers. (Currently we recommend using [Twemproxy](https://github.com/twitter/twemproxy) instead of [Nydus](https://github.com/disqus/nydus))

    Redis was a good solution, but several reasons made us look for an alternative. Firstly we wanted to support multiple content types, which made falling back to the database harder and increased our storage requirements. In addition the database fallback we were still relying on became slower as our community grew. Both these problems could be addressed by storing more data in Redis, but doing so was prohibitively expensive.    

        

### Part three - Cassandra & Feedly

    We briefly looked at [HBase](http://hbase.apache.org/), [DynamoDB](http://aws.amazon.com/dynamodb/) and [Cassandra 2.0](http://cassandra.apache.org/download/). Eventually we opted for Cassandra since it has few moving parts, is used by Instagram and is supported by [Datastax](http://www.datastax.com/). Fashiolista currently does a full push flow for the flat feed and a combination between push and pull for the aggregated feed. We store a maximum of 3600 activities in your feed, which currently takes up 2.12TB of storage. The fluctuations caused by high profile users are mitigated using priority queues, overcapacity and auto scaling.    

## Feed Design

I think our history is quite representative of the process other companies go through. When the time comes for building your own feed system (hopefully using Feedly) there are a few important design decisions to consider.

### 1\. Denormalize vs normalized

There are two approaches you can choose here. The feed with the activities by people you follow either contains the ids of the activities (normalized) or the full activity (denormalized).

Storing only the id vastly reduces your memory usage. However it also means another trip to your data store every time you load the feed. One factor to consider is how often you copy the data when denormalizing. It makes a huge difference if you are building a notification system or a news feed system. For a notification you usually notify 1 or 2 users for every action which occurs. However with a follow based feed systems the action might get copied to thousands of followers.

Furthermore the best choice really depends on your storage backend. With Redis you need to be careful about memory usage. Cassandra on the other hand has plenty of storage space, but is quite hard to use if you normalize your data.

For notification feeds and feeds built on Cassandra we recommend denormalizing your data. For feeds built on Redis you want to minimize your memory usage and keep your data normalized. [Feedly](https://github.com/tschellenbach/Feedly) allows you to pick which approach you prefer.

### 2\. Selective fanout based on producer

In their paper [Yahoo’s Adam Silberstein et.al.](http://research.yahoo.com/files/sigmod278-silberstein.pdf ) argue for a selective approach for pushing to users feeds. A similar approach is currently used by [Twitter](http://highscalability.com/blog/2013/7/8/the-architecture-twitter-uses-to-deal-with-150m-active-users.html ). The basic idea is that doing fan-outs for high profile users can cause a high and sudden load on your systems. This means you need a lot of spare capacity on standby to keep things real time (or be ok with waiting for autoscaling to kick in). In their paper they suggest reducing the load caused by these high profile users by selectively disabling fanouts. Twitter has apparently seen great performance improvements by disabling fanout for high profile users and instead loading their tweets during reads (pull).

### 3\. Selective fanout based on consumer

Another possibility of selective fanouts is to only fan-out to your active users. (Say users who logged in during the last week). At Fashiolista we used a modified version of this idea, by storing the last 3600 activities for active users, but only 180 activities for inactive ones. After those 180 items we would fallback to the database. This setup slows down the experience for inactive users returning to your site, but can really reduce your memory usage and costs.

Silberstein et al. make things more interesting by looking at the consumer and producer pair. The basic intuition is that a push approach makes most sense when:

Fortunately such a complex solution hasn’t been needed yet for Fashiolista. I’m curious at which scale you need such solutions. Be sure to let us know in the comments.

### 4\. Priorities

An alternative strategy is using different priorities for the fan-out tasks. You simply mark fan-outs to active users as high priority and fan-outs to inactive users as low priority. At Fashiolista we keep a higher buffer of capacity for the high priority cluster allowing us to cope with spikes. For the low priority cluster we rely on autoscaling and spot instances. In practice this means that less active user’s feeds may occasionally lag a few minutes behind. Using priorities reduces the impact high profile users have on system load. It doesn’t solve the problem, but greatly reduces the magnitude of the spikes.

### 5\. Redis vs Cassandra

Both Fashiolista and [Instagram](http://planetcassandra.org/blog/post/instagram-making-the-switch-to-cassandra-from-redis-75-instasavings) started out with Redis but eventually switched to Cassandra. I would recommend starting with Redis as it’s just so much easier to setup and maintain.

Redis however has a few limitations. All of your data needs to be stored in RAM which eventually becomes expensive. In addition there is no support for sharding built into Redis. This means that you have to roll your own system for sharding across nodes. ([Twemproxy](https://github.com/twitter/twemproxy) is a great option for this). Sharding across nodes is quite easy, but moving data when you add or remove nodes is a pain. You can work around these limitations by using Redis as a cache and falling back to your database. As soon as it becomes hard to fallback to the database I would consider moving from Redis to Cassandra.

The Cassandra Python ecosystem is still rapidly changing. Both [CQLEngine](https://github.com/cqlengine/cqlengine) and [Python-Driver](https://github.com/datastax/python-driver) are excellent projects, but they needed a bit of [forking](https://github.com/tbarbugli/cqlengine) to work together. If you choose Cassandra you need to be ready to invest time to learn about Cassandra and contribute to client libraries.

## Conclusion

There are many factors to take into account when building your own feed solution. Which storage backend do you choose, how do you handle spikes in load caused by high profile users and to what extend do you denormalize your data? I hope this blogpost has provided you with some inspiration.

[Feedly](https://github.com/tschellenbach/Feedly) doesn’t make any of these choices for you. It’s a framework for building feed systems and leaves it up to you to determine what works best for your use case. For an introduction to Feedly have a look at the [readme](https://github.com/tschellenbach/Feedly) or this tutorial for building a [Pinterest](http://www.mellowmorning.com/2013/10/18/scalable-pinterest-tutorial-feedly-redis/) [esque](http://www.mellowmorning.com/2013/10/18/scalable-pinterest-tutorial-feedly-redis/) [application](http://www.mellowmorning.com/2013/10/18/scalable-pinterest-tutorial-feedly-redis/). If you give it a try be sure to let us know if you encounter issues.

Note that you only need to solve this problem once you get millions of activities in your database. At Fashiolista the simple database solution got us to our first 100M loves and 1M users.

To learn more about feed design I highly recommend reading some of the articles which we based Feedly on:

    

*   [Yahoo Research Paper](http://research.yahoo.com/files/sigmod278-silberstein.pdf)
*   [Twitter 2013](http://highscalability.com/blog/2013/7/8/the-architecture-twitter-uses-to-deal-with-150m-active-users.html) Redis based, with fallback
*   [Cassandra at Instagram](http://planetcassandra.org/blog/post/instagram-making-the-switch-to-cassandra-from-redis-75-instasavings)
*   [Etsy feed scaling](http://www.slideshare.net/danmckinley/etsy-activity-feeds-architecture/)
*   [Facebook history](http://www.infoq.com/presentations/Facebook-Software-Stack)
*   [Django project, with good naming conventions.](http://justquick.github.com/django-activity-stream/) (But database only)
*   [http://activitystrea.ms/specs/atom/1.0/](http://activitystrea.ms/specs/atom/1.0/) (actor, verb, object, target)
*   [Quora post on best practises](http://www.quora.com/What-are-best-practices-for-building-something-like-a-News-Feed?q=news+feeds)
*   [Quora scaling a social network feed](http://www.quora.com/What-are-the-scaling-issues-to-keep-in-mind-while-developing-a-social-network-feed)
*   [Redis ruby example](http://web.archive.org/web/20130525202810/http://blog.waxman.me/how-to-build-a-fast-news-feed-in-redis)
*   [FriendFeed approach](http://backchannel.org/blog/friendfeed-schemaless-mysql)
*   [Thoonk setup](http://blog.thoonk.com/)
*   [Twitter's Approach](http://www.slideshare.net/nkallen/q-con-3770885)

            

    