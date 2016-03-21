## [通过Redis解决的11个常见问题](http://highscalability.com/blog/2011/7/6/11-common-web-use-cases-solved-in-redis.html)

![](http://farm7.static.flickr.com/6034/5909127950_be0f5beeeb_o.png)

In [How to take advantage of Redis just adding it to your stack](http://antirez.com/post/take-advantage-of-redis-adding-it-to-your-stack.html) Salvatore 'antirez' Sanfilippo shows how to solve some common problems in Redis by taking advantage of its unique data structure handling capabilities. Common Redis primitives like LPUSH, and LTRIM, and LREM are used to accomplish tasks programmers need to get done, but that can be hard or slow in more traditional stores. A very useful and practical article. How would you accomplish these tasks in your framework?

[如何快速上手Redis](http://antirez.com/post/take-advantage-of-redis-adding-it-to-your-stack.html)是一篇相当实用的短文，文中Salvatore 'antirez' Sanfilippo 展示了如何利用Redis去解决一些常见问题，Redis提供的一些原生方法像LPUSH，LTRIM，LREM等可以用来解决传统数据仓库中较难处理的编程问题。那么在你的架构中，你是如何解决下面这些问题的呢？

1.  **Show latest items listings in your home page**. This is a live in-memory cache and is very fast. [LPUSH](http://redis.io/commands/lpush) is used to insert a content ID at the head of the list stored at a key. [LTRIM](http://redis.io/commands/ltrim) is used to limit the number of items in the list to 5000\. If the user needs to page beyond this cache only then are they sent to the database.

1. **在主页中展示最新的条目** 这里的限制是要求实时的in-momory的缓存并且速度必须要快。[LPUSH](http://redis.io/commands/lpush) 一般被用来在队列头部插入content ID. [LTRIM](http://redis.io/commands/ltrim) 被用来将条目数量控制在5000条。如果用户需要的内容不在缓存里，那么只要让他们发起数据库查询就可以了

2.  **Deletion and filtering**. If a cached article is deleted it can be removed from the cache using [LREM](http://redis.io/commands/lrem).

2. **删除和过滤** 如果一篇文章已经被删除，那么可以用[LREM](http://redis.io/commands/lrem)把他从内存中移出来

3.  **Leaderboards and related problems**. A leader board is a set sorted by score. The [ZADD](http://redis.io/commands/zadd) commands implements this directly and the [ZREVRANGE](http://redis.io/commands/zrevrange) command can be used to get the top 100 users by score and [ZRANK](http://redis.io/commands/zrank) can be used to get a users rank. Very direct and easy.

3.  **排行榜及相关问题** 排行榜是一个按分数排序的数据集。 [ZADD](http://redis.io/commands/zadd) 命令专门用来解决排序问题，[ZREVRANGE](http://redis.io/commands/zrevrange) 命令能用来找出得分排名前100的用户，[ZRANK](http://redis.io/commands/zrank)命令能用来获取用户评级，简单直接

4. **Order by user votes and time**. This is a leaderboard like Reddit where the score is formula the changes over time. LPUSH + LTRIM are used to add an article to a list. A background task polls the list and recomputes the order of the list and ZADD is used to populate the list in the new order. This list can be retrieved very fast by even a heavily loaded site. This should be easier, the need for the polling code isn't elegant.

4. **按用户投票及时间排序**  这是一个类似Reddit的排行榜。利用LPUSH + LTRIM向列表中添加文章。后台程序对列表投票并重新计算列表排名，ZADD用来按新的列表排名刷新列表。通过这种方式甚至能让负载相当重的网站快速更新排行榜。这个方法十分易于实现，虽然所需的投票代码并不够优雅

5.  **Implement expires on items**. To keep a sorted list by time then use unix time as the key. The difficult task of expiring items is implemented by indexing current_time+time_to_live. Another background worker is used to make queries using ZRANGE ... with SCORES and delete timed out entries.

5.  **项目超时** 用unix时间作为主键保存一个顺序列表。 不同任务的超时时间利用当前时间(current_time)+任务时长（time_to_live）来控制。另外一个后台线程使用ZRANGE生成队列... 同时打分并删除超时项目。

6.  **Counting stuff**. Keeping stats of all kinds is common, say you want to know when to block an IP addresss. The [INCRBY](http://redis.io/commands/incrby) command makes it easy to atomically keep counters; [GETSET](http://redis.io/commands/getset) to atomically clear the counter; the _expire_ attribute can be used to tell when an key should be deleted.

6.  **计数器** 保存各种状态是一个十分常见的需求，比方说你想禁止某个ip的访问。[INCRBY](http://redis.io/commands/incrby)命令能很方便的生成一个原子计数器（atomically keep counters）、 [GETSET](http://redis.io/commands/getset) 用来清除原子计数器、 _expire_ 属性用来确定超时时间

7.  **Unique N items in a given amount of time**. This is the unique visitors problem and can be solved using [SADD](http://redis.io/commands/sadd) for each pageview. SADD won't add a member to a set if it already exists.

7.  **给定时间内独立的N项目的计数** 这是一个独立访客(unique visitors)问题，能通过对每个pageview使用[SADD](http://redis.io/commands/sadd)命令实现. SADD不会对数据集中以存在的用户重复计数.

8.  **Real time analysis of what is happening, for stats, anti spam, or whatever**. Using Redis primitives it's much simpler to implement a spam filtering system or other real-time tracking system.

9.  **Pub/Sub**. Keeping a map of who is interested in updates to what data is a common task in systems. Redis has a [pub/sub](http://redis.io/topics/pubsub) feature to make this easy using commands like [SUBSCRIBE](http://redis.io/commands/subscribe), [UNSUBSCRIBE](http://redis.io/commands/unsubscribe), and [PUBLISH](http://redis.io/commands/publish). 
10.  **Queues**. Queues are everywhere in programming. In addition to the push and pop type commands, Redis has [blocking queue](http://redis.io/commands/blpop) commands so a program can wait on work being added to the queue by another program. You can also do interesting things implement a rotating queue of RSS feeds to update.
11.  **Caching**. Redis can be used in the same [manner as memcache](https://groups.google.com/forum/#!topic/redis-db/Iqrm5E87o9Y).

The take home is to not endlessly engage in model wars, but see what can be accomplished by composing powerful and simple primitives together. Certainly you can write specialized code to do all these operations, but Redis makes it much easier to implement and reason about.

Please read the [original article](http://antirez.com/post/take-advantage-of-redis-adding-it-to-your-stack.html) for more details. 

## Related Articles

*   [Resque](https://github.com/blog/542-introducing-resque) - Redis-backed library for creating background jobs, placing those jobs on multiple queues, and processing them later.
*   [Hacker News thread](http://news.ycombinator.com/item?id=2705475). 

    