---
author: Paul Huang
home: https://github.com/rainbow494
---

# 101 Questions to Ask When Considering a NoSQL Database
# 使用NoSQL前应该考虑的101个问题

![](http://farm5.static.flickr.com/4127/5188198566_3fe006d562_m.jpg)

You need answers, I know, but all I have here are some questions to consider when thinking about which database to use. These are taken from my [webinar](http://voltdb.com/resources/webinars-all) [What Should I Do? Choosing SQL, NoSQL or Both for Scalable Web Applications](http://www.slideshare.net/toddhoffious/what-should-ido-11). It's a companion article to [What The Heck Are You Actually Using NoSQL For?](http://highscalability.com/blog/2010/12/6/what-the-heck-are-you-actually-using-nosql-for.html)

我知道你想要答案，但我能提供的，只有面对选择时需要考虑的问题。这些问题来自于我的[在线会议](http://voltdb.com/resources/webinars-all)、[左手？右手？双手并用？SQL NoSQL如何选择](http://www.slideshare.net/toddhoffious/what-should-ido-11)、还有这篇指南 - [NoSQL踩坑记](http://highscalability.com/blog/2010/12/6/what-the-heck-are-you-actually-using-nosql-for.html)

Actually, I don't even know if there are a 101 questions, but there are a lot/way too many. You might want to use these questions as kind of a NoSQL [I Ching](http://en.wikipedia.org/wiki/I_Ching), guiding your way through the immense possibility space of options that are in front of you. Nothing is fated, all is interpreted, but it might just trigger a new insight or two along the way.

说实话，我甚至吃不准是不是只要考虑101个问题，或者还有更多问题需要思考。又或者你会把这些当成是有助推衍真相的NoSQL易经，并在过程中意识到，没有什么是永恒唯一的，一切都源自你的解读，你将能从一些新的角度来观察问题

## Where are you starting from?
## 思考起点

*   A can do anything green field application?
*   是否要做绿色应用？
*   In the middle of a project and worried about hitting bottlenecks?
*   项目中期，担心会遇到瓶颈？
*   Worried about hitting the scaling wall once you deploy?
*   担心系统部署后难以扩展？
*   Adding a separate loosely coupled service to an existing system?
*   为已有系统增加一个独立的松耦合的服务？
*   What are your resources? expertise? budget?
*   现有资源？知识储备？预算？
*   What are your pain points? What's so important that if it fails you will fail??What forces are pushing you?
*   痛点？为什么这个点如此重要？因为失败你就会破产？你的原动力？
*   What are your priorities?Prioritize them. What is really important to you, what must get done?
*   待处理问题的优先级？为它们排序，从中找出那些重要且必须的项目
*   What are your risks?Prioritize them. Is the risk of being unavailable more important than being inconsistent?
*   可能风险？为它们排序，例如:系统不可用造成的风险是否比数据不一致所造成的风险更高？


## What are you trying to accomplish?
## 目标
* What are you trying to accomplish?
* 当前的目标是？
* What's the delivery schedule?
* 交付期限？
* Do the research to be specific, like Facebook did with their [messaging system](http://highscalability.com/blog/2010/11/16/facebooks-new-real-time-messaging-system-hbase-to-store-135.html):
* 是否像Facebook对他们的消息系统一样[messaging system](http://highscalability.com/blog/2010/11/16/facebooks-new-real-time-messaging-system-hbase-to-store-135.html)
对自己的系统做过深入研究？
> Facebook chose HBase because they monitored their usage and figured out what was needed: a system that could handle two types of data patterns.
> Facebook之所以选择HBase是因为通过对惯用场景的研究,他们发现他们真正需要的是一个可以处理两种不同数据模式的数据库

## Things to Consider...Your Problem
## 考虑事项之：你的问题

*   Do you need to build a custom system?    
*   需要构建定制系统？
*   Access patterns?
    1. A short set of temporal data that tends to be volatile
    2. An ever-growing set of data that rarely gets accessed
    3. High write loads
    4. High throughput
    5. Sequential
    6. Random    
*   数据访问模式？
    1. 临时存储、容易改变的短期数据
    1. 永久存储、很少使用的长期数据
    1. 高写负载
    1. 高传输
    1. 有序
    1. 随机
*   Requires scalability?
*   需要具备伸缩性？
*   Is availability more important than consistency, or is it latency, transactions, durability, performance, or ease of use?
*   对系统来说可用性比一致性更重要？等待时间、事务、持久化、性能、易用性哪个更重要？
*   Cloud or colo? Hosted services? Resources like disk space?
*   如何部署？选择云端还是主机托管？涉及哪些资源？
*   Can you find people who know the stack?
*   能找到具备相关知识的人手？
*   Tired of the data transformation (ORM) treadmill?
*   包含大量乏味的数据转换工作(ORM)？
*   Store data that can be accessed quickly and is used often?
*   数据能快速存取并经常访问？
*   Would like a high level interface like PaaS?
*   系统类似Paas，有许多高层级接口？

## Things to Consider...Money
## 考虑事项之：资金

* Cost With money you have different options than if you don't. You can probably make the technologies you know best scale.
* 如果有资金，那么就需要在如何分配上做出考量，这样，才有可能利用所知的技术构建出最好的可伸缩系统
* Inexpensive scaling?
* 支持低成本降低扩展？
* Lower operations cost?    
* 支持低运营成本？
* No sysadmins?    
* 支持无人值守？
* Type of license?
* 许可证类型？
* Support costs?
* 技术支持价格？

## Things to Consider...Programming
## 考虑事项之：编码

* Flexible datatypes and schemas?
* 灵活的数据类型、架构？
* Support for which language bindings?    
* 支持哪些语言？
* Web support: JSON, REST, HTTP, JSON-RPC
* 通信方式、协议：JSON、REST、HTTP、JSON-RPC？
* Built-in stored procedure support?Javascript?
* 内建存储过程？原生支持JS？
* Platform support: mobile, workstation, cloud
* 部署平台：手机、工作站还是云端？
* Transaction support: key-value, distributed, ACID, BASE, eventual consistency, multi-object ACID transactions.
* 事务支持：主键、分布式、原子性、BASE、 eventual consistency、multi-object ACID transactions.
* Datatype support: graph, key-value, row, column, JSON, document, references, relationships, advanced data structures, large BLOBs.
* 数据类型支持：图表、键值对、行、列、JSON、文档、引用、关系、高级数据结构、超大对象
* Prefer the simplicity of transaction model where you can just update and be done with it? In-memory makes it fast enough and big systems can fit on just a few nodes.
* 倾向于简单且易于修改的事务模型？存放在内存中以保证快速读写并只需极少的节点？

## Things to Consider...Performance
## 考虑事项之：性能

* Performance metrics: IOPS/sec, reads, writes, streaming?
* 性能指标： IOPS/sec、读、写、传输？
* Support for your access pattern: random read/write; sequential read/write; large or small or whatever chunk size you use?
* 是否支持你的数据存取模式：随机读写、队列读写？数据块大小如何？
* Are you storing frequently updated bits of data?
* 对比特数据做频繁的更新排序？
* High Concurrency vs High Performance?
* 高并发、高性能哪个优先？
* Problems that limit the type of work load you care about?
* 负载上限的所有问题中你更关心哪方面？
* Peak QPS on highly-concurrent workloads?
* 高并发场景下的QPS峰值如何?
* Test your specific scenarios?
* 有对特定应用场景做过测试？

## Things to Consider...Features
## 考虑事项之：主要特性

* Spooky scalability at a distance: support across multiple data-centers?
* 在超大范围内保证可扩展性：例如支持跨多个数据中心？
* Ease of installation, configuration, operations, development, deployment, support, manage, upgrade, etc.    
* 易安装、配置、维护、开发、部署、产品支持、管理、升级...
* Data Integrity: In DDL, Stored Procedure, or App
* 数据完整性：通过DDL、存储过程、还是应用来保证？
* Persistence design: Memtable/SSTable; Apend-only B-tree; B-tree; On-disk linked lists; In-memory replicated; In-memory snapshots; In-memory only; Hash; Pluggable.
* 持久化设计：Memtable/SSTable、Apend-only B-tree、B-tree、On-disk linked lists、In-memory replicated、In-memory snapshots、In-memory only、Hash、Pluggable
* Schema support: none, rigid, optional, mixed
* 架构支持：无、严格、可选、混合
* Storage model: embedded, client/server, distributed, in-memory
* 存储模式：嵌入式、客户端/服务端、分布式、内存
* Support for search, secondary indexes, range queries, ad-hoc queries, MapReduce?
* 支持搜索、[二级索引](http://www.cnblogs.com/yangjin-55/archive/2012/08/05/2786518.html)、[范围查询](https://en.wikipedia.org/wiki/Range_query_(database))、[ad-hoc查询](http://blog.csdn.net/maray/article/details/6578956)、MapReduce？
* Hitless upgrades?
* 支持在线升级？

## Things to Consider...More Features
## 考虑事项之：次要特性

* Tunability of consistency models?
* 支持可调优的[一致性模型](https://en.wikipedia.org/wiki/Consistency_model)？
* Tools availability and product maturity?
* 工具实用性？产品完备性？
* Expand rapidly?Develop rapidly?Change rapidly?
* 扩展速度？开发速度？更改速度？
* Durability? On power failure?
* 健壮性？容灾性？
* Bulk import? Export?
* 支持批量导入、导出？
* Hitless upgrades?
* 支持在线升级？
* Materialized views for rollups of attributes?
* 支持[物化视图](http://www.cnblogs.com/lizhe88/archive/2011/01/23/1942423.html)？
* Built-in web server support?
* 内建web服务？
* Authentication, authorization, validation?
* 验证、授权、校验
* Continuous write-behind for system sync?
* 同步过程中是否支持连续[后台写入](http://community.gemstone.com/display/gemfire60/Database+write-behind+and+read-through)？
* What is the story for availability, data-loss prevention, backup and restore?
* 数据保护、数据备份、数据恢等相关问题的应用场景？
* Automatic load balancing, partitioning, and repartitioning?
* 支持自动负载均衡、分区、再分割？
* Live addition and removal of machines?
* 可在线添加和移除主机？

## Things to Consider...The Vendor
## 考虑事项之：供应商

* Viability of the company?
* 供应商的发展前景？
* Future direction?
* 供应商的发展方向？
* Community and support list quality?
* 社区品质及支持列表？
* Support responsiveness?
* 产品支持响应速度？
* How do they handle disasters?
* 供应商如何应对灾害？
* Quality and quantity of partnerships developed?
* 已有合作伙伴的数量及品质？
* Customer support:enterprise-level SLA, paid support, none
* 客户支持：企业级SLA，支持费用，无

## 原文链接
- [101 Questions To Ask When Considering A NoSQL Database](http://highscalability.com/blog/2011/6/15/101-questions-to-ask-when-considering-a-nosql-database.html)
