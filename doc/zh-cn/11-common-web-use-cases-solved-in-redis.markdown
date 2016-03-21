## [11个适合用Redis解决的常见问题](http://highscalability.com/blog/2011/7/6/11-common-web-use-cases-solved-in-redis.html)

![](http://farm7.static.flickr.com/6034/5909127950_be0f5beeeb_o.png)

[如何快速上手Redis](http://antirez.com/post/take-advantage-of-redis-adding-it-to-your-stack.html)是一篇相当实用的短文，文中Salvatore 'antirez' Sanfilippo 展示了如何利用Redis去解决一些常见问题，Redis提供的一些原生方法像LPUSH，LTRIM，LREM等可以用来解决传统数据仓库中较难处理的编程问题。那么在你的方案中，是如何解决下面这些问题的呢？

1. **在主页中展示最新的条目** 这个功能通常的要求是要求常驻内存缓存（live in-memory cache）并且响应要快。[LPUSH](http://redis.io/commands/lpush) 一般被用来在队列头部插入content ID、[LTRIM](http://redis.io/commands/ltrim) 被用来将条目数量控制在5000条。如果用户需要的内容不在缓存里，那么让他们发起数据库查询就可以了

2. **删除和过滤** 如果一篇文章已经被删除，那么可以用[LREM](http://redis.io/commands/lrem)将其从内存中移出

3.  **排行榜及相关问题** 排行榜其实就是一个按分数排序的数据集。 [ZADD](http://redis.io/commands/zadd) 命令用来建立集合，[ZREVRANGE](http://redis.io/commands/zrevrange) 命令用来找出得分排名前100的用户，[ZRANK](http://redis.io/commands/zrank)命令用来获取用户评级，简单直接

4. **按用户投票及时间排序**  例如要实现类似Reddit的排行榜。可以利用LPUSH + LTRIM向列表中添加文章。后台程序对列表轮询并重新计算列表排名，ZADD用来按新的列表排名刷新列表。通过这种方式甚至能让负载相当重的网站快速更新排行榜。这个方法十分易于实现，对轮询代码要求也不高

5.  **实现条目超时功能** 例如你需要实现一个以unix时间作为主键的顺序列表， 那么你可以通过对当前时间(current_time)+任务时长（time_to_live）建立索引，同时让另一个后台线程使用ZRANGE命令生成队列... 同时打分并删除超时项目。

6.  **计数器** 保存各种状态是一个十分常见的需求，比方说你想禁止某个ip的访问。[INCRBY](http://redis.io/commands/incrby)命令能很方便的以原子形式生成计数器（atomically keep counters）、 [GETSET](http://redis.io/commands/getset) 用来以原子形式清除计数器（atomically clear the counter）、 _expire_ 属性用来决定对应键值的删除时间

7.  **给定时间内独立的N个项目的计数** 例如独立访客(unique visitors)问题就能通过对每个pageview使用[SADD](http://redis.io/commands/sadd)命令实现计数，SADD不会对数据集中以存在的用户重复计数。

8.  **对系统状况（如垃圾邮件）等情况进行实时分析** 利用Redis能很容易的实现垃圾过滤系统和其它实时跟踪系统。

9.  **订阅/发布** 在系统中保存一张对当前数据的更新感兴趣的用户的映射表是一个常见任务。Redis功能可以通过命令 [SUBSCRIBE](http://redis.io/commands/subscribe)， [UNSUBSCRIBE](http://redis.io/commands/unsubscribe)，及 [PUBLISH](http://redis.io/commands/publish)来实现这个任务，用法详见[pub/sub](http://redis.io/topics/pubsub)

10.  **队列** 队列在开发时经常被用到，除了push和pop这类命令外，Redis还支持[blocking queue](http://redis.io/commands/blpop) 命令，该命令可以在队列为空时让当前程序等待，直到其他程序向该队列添加工作任务后再运行。你能利用它实现一些有趣的功能，例如RSS订阅更新消息发布

11.  **缓存** Redis能用来[作为高性能缓存（manner as memcache）](https://groups.google.com/forum/#!topic/redis-db/Iqrm5E87o9Y).

本文初衷是希望让大家在实现上述功能时有更多选择，你当然可以利用代码实现相同功能，但是Redis让过程变得更简单，所以为什么不试试呢？

请阅读[原文](http://antirez.com/post/take-advantage-of-redis-adding-it-to-your-stack.html)了解更多 

## 参考列表
*   [Resque](https://github.com/blog/542-introducing-resque) - Redis-backed library for creating background jobs, placing those jobs on multiple queues, and processing them later.
*   [Hacker News thread](http://news.ycombinator.com/item?id=2705475). 
