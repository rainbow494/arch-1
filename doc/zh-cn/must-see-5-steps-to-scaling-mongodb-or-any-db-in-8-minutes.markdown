## [必读: 只需5步，8分钟内扩展Mongo数据库 (或任易数据库)](http://highscalability.com/blog/2011/9/13/must-see-5-steps-to-scaling-mongodb-or-any-db-in-8-minutes.html)

![](http://farm7.static.flickr.com/6204/6096403479_68626bb1ac_m.jpg) [Jared Rosoff](http://jaredrosoff.com/) concisely, effectively, entertainingly, and convincingly gives an [8 minute MongoDB tutorial](http://www.youtube.com/watch?v=lnY8D3TL_tM) on scaling MongoDB at [Scale Out Camp](http://www.scaleoutcamp.org). The ideas aren't just limited to MongoDB, they work for most any database: Optimize your queries; Know your working set size; Tune your file system; Choose the right disks; Shard. Here's an explanation of all 5 strategies:



![](http://farm7.static.flickr.com/6204/6096403479_68626bb1ac_m.jpg) 在[Scale Out Camp](http://www.scaleoutcamp.org)上[Jared Rosoff](http://jaredrosoff.com/) 发表了一篇有趣而实用的短文，[MongoDB 8分钟速成](http://www.youtube.com/watch?v=lnY8D3TL_tM) 文中的概念不止适用于MongoDB，它们几乎适用于任何数据库: 优化你的队列、了解你的工作集的大小、调整你的文件系统、 选择正确的磁盘、分片（Shard）。下面是关于这5个策略的解释:


1.  **Optimize your queries**. Computer science works. Complexity analysis works. A btree search is faster than a table scan. So analyze your queries. Use explain to see what your query is doing. If it is saying it's using a cursor then it's doing a table scan. That's slow. Look at the number of documents it looks at to satisfy a query. Look at how long it takes. Fix: add indexes. It doesn't matter if you are running on 1 or 100 servers.

1.  **优化你的查询** 科学计算任务、复杂的分析任务、b树查询比表扫描（table scan）要快。所以分析你的查询语句，利用解释器查看你的查询的执行过程，如果结果显示过程中使用了游标或者有全表扫描，那么查询速度一定很慢。检查完成一个查询语句需要的文档数（Look at the number of documents it looks at to satisfy a query），查看每一步的所需时间，然后通过添加索引来解决问题。

2.  **Know your working set size**. Sticking memcache in front of your database is silly. You have lots of RAM, use it. Embed your cache in the database, which is how MongoDB works. Working set = Active Documents + Used Indexes. Hitting something in RAM is fast, disk is slow. If you have a billion users and only 100K are active at a time then 100K is your working set. You want to have enough RAM for those 100K so operations are in RAM on not on disk. Remember indexes take memory too. It doesn't matter if you are running on 1 or 100 servers.

2.  **了解工作集的大小** 在数据库前增加一层高速缓存（memcache）是十分愚蠢的。你已经有了内存，那就用了它们。把缓存（cache）与数据库整合在一起，这正是MongoDB的工作方式。 工作集（Working set）= 活动文档（Active Documents）+ 已用索引（Used Indexes）。在内存中查找资料比在硬盘中快得多。如果你有100万个用户但当前只有10万个活跃用户那么10万就是你的工作集的大小。假如你有内存放下这十万条记录那么你当然可以在内存里而不是硬盘上操作他们，不过记得，索引同样消耗内存

3.  **Tune your file system**. Performance problems often traced to the filesystem. EXT3 is ancient. Use EXT4, XFS, or some other well performing file system. Turn off access time tracking, for a database there's no need to update a file every time a file is accessed, this is another write. Preallocating 2GB of files on EXT3 must actually write those bytes, it's slow.

4.  **Choose the right disks**. Seek time is what matters. Most of what you are doing is random IO. Seek time is governed by a mechanical arm that has to swing over the disk. The average disk drive can do 200 seeks a second. Faster drives will move data off the disk faster, that is they have higher bandwidth, but their seek times will be the same. Single disk: you can do 200 queries a second. RAID 0 (stripe across multiple disks): 3 disks means 600 queries a second. RAID 10 (mirror and stripe): 6 disks means 1200 seeks a second. Choose the right disks. RAID matters. SSDs are awesome: .1 ms for a seek vs 5 ms for a disk seek. Great for random access.

5.  **Shard**. If your app is slow, uses bad indexing, has slow disk drives, then a single node will be slow. Fix all this stuff before scaling out using sharded. Sharding lets you spread your workload over more machines along with high availability with replica sets. Data is partitioned to shards by ranges, for example. Can scale out to 100s of servers. Each can process 10s of 1000s of writes. Can add more capacity easily. Sharding with a good database multiples the benefits of having good queries, good drives, and good working sets. 

## 参考列表

*   [Hilarious Video: Relational Database Vs NoSQL Fanbois](http://highscalability.com/blog/2010/9/5/hilarious-video-relational-database-vs-nosql-fanbois.html)
