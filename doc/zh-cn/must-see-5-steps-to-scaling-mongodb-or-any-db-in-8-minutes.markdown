## [必读: 只需5步，8分钟内扩展Mongo数据库 (或任意数据库)](http://highscalability.com/blog/2011/9/13/must-see-5-steps-to-scaling-mongodb-or-any-db-in-8-minutes.html)

![](http://farm7.static.flickr.com/6204/6096403479_68626bb1ac_m.jpg) [Jared Rosoff](http://jaredrosoff.com/)在[Scale Out Camp](http://www.scaleoutcamp.org)上发表了一篇有趣而实用的短文，[MongoDB 8分钟速成](http://www.youtube.com/watch?v=lnY8D3TL_tM) 文中的概念不仅适用于MongoDB，它们几乎适用于任何数据库。全文包括: 优化你的队列、了解你的工作集的大小、调整你的文件系统、 选择正确的磁盘、分片（Shard）。下面是关于这5个优化策略的解释:

1.  **优化你的查询** 在科学计算任务、复杂的分析任务中，b树查询比全表扫描（table scan）要快，所以分析你的查询语句，利用解释器查看你的查询语句的执行过程，如果结果显示过程中使用了游标或者有全表扫描，那么查询速度一定很慢。通过检查查询所需文档数量来完善查询语句（Look at the number of documents it looks at to satisfy a query），查看每一步的所需时间，然后通过添加索引来解决问题。

2.  **了解工作集的大小** 在数据库前再增加一层高速缓存（memcache）不一定是个好主意，因为你已经有很多内存了，那为什么不直接利用它们呢？把缓存（cache）嵌入数据库正是MongoDB的工作方式。要做到这点，首先请了解一下定义：工作集（Working set）= 活动文档（Active Documents）+ 已用索引（Used Indexes），同时要知道在内存中查找资料比在硬盘中快得多，因此，如果你有100万个用户，但当前只有10万个活跃用户，那么10万就是你的工作集的大小，所以假如你的内存足够放下这10万条记录，那么你当然可以在内存里而不是硬盘上操作他们，不过记得，索引同样需要使用内存

3.  **调校你的文件系统** 性能问题常和文件系统有关。假如你还在使用古老的EXT3文件系统，那么请改用EXT4，XFS，或其它性能更好的文件系统。 关闭访问时间记录（access time tracking），对于数据库文件，没有必要在每次访问文件时更新访问时间，因为这其实是另一种写入操作

4.  **选择正确的磁盘** 磁盘检索时间是这么回事：比如，大部分时间你的程序都在做随机IO，那磁盘检索时间就由机械臂在磁盘上的摇摆次数来决定。平均来说磁盘驱动一秒能做200次检索，快一点的驱动通过将数据移动到更快的磁盘上来提高速度，但那就是磁盘检索速度的上限了，其实总的查询次数其实是一样的。Single disk: 1块硬盘=200次查询/1秒， RAID 0 (stripe across multiple disks): 3块硬盘=600次查询/1秒，RAID 10 (mirror and stripe): 6块硬盘=1200次查询/1秒，所以请选择合适的硬盘和RAID模式。如果不差钱的话，那可以使用速度更快的固态硬盘 - （固态硬盘）.1 ms/查询 vs 5 ms/查询（硬盘）

5.  **分片** 如果你应用很慢，比如用了坏的索引或用了较慢的磁盘驱动，那某个节点会变得非常慢，请在利用分片扩展系统前修复相关问题。分片可让你在拥有备份节点（replica sets）的情况下把负载分摊到更多机器上，从而提高可用性。分区后的数据按范围分片(Data is partitioned to shards by ranges)，举例来说，如果将服务器数量扩展100倍，然后有要求进行1000次写操作，那么每台服务器只要处理10次请求就够了（Can scale out to 100s of servers. Each can process 10s of 1000s of writes）这样性能提升会变得更容易。 在拥有优良的查询，驱动，工作集的情况下，分片给数据库带来的好处能成倍体现。 

## 参考列表

*   [Hilarious Video: Relational Database Vs NoSQL Fanbois](http://highscalability.com/blog/2010/9/5/hilarious-video-relational-database-vs-nosql-fanbois.html)
