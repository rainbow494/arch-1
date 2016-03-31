---
author: Paul Huang
home: https://github.com/rainbow494
layout: post
title: 使用NoSQL前应该考虑的101个问题
date: 2016-3-28 00:00:00
---

# 使用NoSQL前应该考虑的101个问题

![](http://farm5.static.flickr.com/4127/5188198566_3fe006d562_m.jpg)

我知道你想要答案，但我能提供的，只有面对选择时需要考虑的问题。这些问题来自于我的[在线会议](http://voltdb.com/resources/webinars-all)、[左手？右手？双手并用？SQL NoSQL如何选择](http://www.slideshare.net/toddhoffious/what-should-ido-11)、还有这篇指南 - [NoSQL踩坑记](http://highscalability.com/blog/2010/12/6/what-the-heck-are-you-actually-using-nosql-for.html)

说实话，我甚至吃不准是不是只要考虑101个问题，或者还有更多问题需要思考。又或者你会把这些当成是有助推衍真相的NoSQL易经，并在过程中意识到，没有什么是永恒唯一的，一切都源自你的解读，你将能从一些新的角度来观察问题

## Where are you starting from?
## 思考起点

*   是否要做绿色应用？
*   项目中期，担心会遇到瓶颈？
*   担心系统部署后难以扩展？
*   为已有系统增加一个独立的松耦合的服务？
*   现有资源？知识储备？预算？
*   痛点？为什么这个点如此重要？因为失败你就会破产？你的原动力？
*   待处理问题的优先级？为它们排序，从中找出那些重要且必须的项目
*   可能风险？为它们排序，例如:系统不可用造成的风险是否比数据不一致所造成的风险更高？


## 目标
* 当前的目标是？
* 交付期限？
* 是否像Facebook对他们的消息系统一样[messaging system](http://highscalability.com/blog/2010/11/16/facebooks-new-real-time-messaging-system-hbase-to-store-135.htmlss)对自己的系统做过深入研究？
> Facebook chose HBase because they monitored their usage and figured out what was needed: a system that could handle two types of data patterns.
> Facebook之所以选择HBase是因为通过对惯用场景的研究,他们发现他们真正需要的是一个可以处理两种不同数据模式的数据库

## 考虑事项之：你的问题

*   需要构建定制系统？
*   数据访问模式？
    1. 临时存储、容易改变的短期数据
    1. 永久存储、很少使用的长期数据
    1. 高写负载
    1. 高传输
    1. 有序
    1. 随机

*   需要具备伸缩性？
*   对系统来说可用性比一致性更重要？等待时间、事务、持久化、性能、易用性哪个更重要？
*   如何部署？选择云端还是主机托管？涉及哪些资源？
*   能找到具备相关知识的人手？
*   包含大量乏味的数据转换工作(ORM)？
*   数据能快速存取并经常访问？
*   系统类似Paas，有许多高层级接口？

## 考虑事项之：资金

* 如果有资金，那么就需要在如何分配上做出考量，这样，才有可能利用所知的技术构建出最好的可伸缩系统
* 支持低成本降低扩展？
* 支持低运营成本？
* 支持无人值守？
* 许可证类型？
* 技术支持价格？

## 考虑事项之：编码

* 灵活的数据类型、架构？
* 支持哪些语言？
* 通信方式、协议：JSON、REST、HTTP、JSON-RPC？
* 内建存储过程？原生支持JS？
* 部署平台：手机、工作站还是云端？
* 事务支持：主键、分布式、原子性、BASE、 eventual consistency、multi-object ACID transactions.
* 数据类型支持：图表、键值对、行、列、JSON、文档、引用、关系、高级数据结构、超大对象
* 倾向于简单且易于修改的事务模型？存放在内存中以保证快速读写并只需极少的节点？

## 考虑事项之：性能

* 性能指标： IOPS/sec、读、写、传输？
* 是否支持你的数据存取模式：随机读写、队列读写？数据块大小如何？
* 对比特数据做频繁的更新排序？
* 高并发、高性能哪个优先？
* 负载上限的所有问题中你更关心哪方面？
* 高并发场景下的QPS峰值如何?
* 有对特定应用场景做过测试？

## 考虑事项之：主要特性

* 在超大范围内保证可扩展性：例如支持跨多个数据中心？
* 易安装、配置、维护、开发、部署、产品支持、管理、升级...
* 数据完整性：通过DDL、存储过程、还是应用来保证？
* 持久化设计：Memtable/SSTable、Apend-only B-tree、B-tree、On-disk linked lists、In-memory replicated、In-memory snapshots、In-memory only、Hash、Pluggable
* 架构支持：无、严格、可选、混合
* 存储模式：嵌入式、客户端/服务端、分布式、内存
* 支持搜索、[二级索引](http://www.cnblogs.com/yangjin-55/archive/2012/08/05/2786518.html)、[范围查询](https://en.wikipedia.org/wiki/Range_query_(database))、[ad-hoc查询](http://blog.csdn.net/maray/article/details/6578956)、MapReduce？
* 支持在线升级？

## 考虑事项之：次要特性

* 支持可调优的[一致性模型](https://en.wikipedia.org/wiki/Consistency_model)？
* 工具实用性？产品完备性？
* 扩展速度？开发速度？更改速度？
* 健壮性？容灾性？
* 支持批量导入、导出？
* 支持在线升级？
* 支持[物化视图](http://www.cnblogs.com/lizhe88/archive/2011/01/23/1942423.html)？
* 内建web服务？
* 验证、授权、校验
* 同步过程中是否支持连续[后台写入](http://community.gemstone.com/display/gemfire60/Database+write-behind+and+read-through)？
* 数据保护、数据备份、数据恢等相关问题的应用场景？
* 支持自动负载均衡、分区、再分割？
* 可在线添加和移除主机？

## 考虑事项之：供应商

* 供应商的发展前景？
* 供应商的发展方向？
* 社区品质及支持列表？
* 产品支持响应速度？
* 供应商如何应对灾害？
* 已有合作伙伴的数量及品质？
* 客户支持：企业级SLA，支持费用，无

## 原文链接
- [101 Questions To Ask When Considering A NoSQL Database](http://highscalability.com/blog/2011/6/15/101-questions-to-ask-when-considering-a-nosql-database.html)
