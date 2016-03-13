## [使用NoSQL时会踩哪些坑?](/blog/2010/12/6/what-the-heck-are-you-actually-using-nosql-for.html)

![](http://farm5.static.flickr.com/4127/5188198566_3fe006d562_m.jpg)

众所周知，我们应该应该__为任务选择合适的工具__。大家都这么说，谁说不是呢？但是就算知道这个道理，它也无法回答我们：对眼前任务，是最合适的工具是哪一把？别人用的趁手的工具我也用得惯么？面对不断迫近的截止日，是不是应该冒风险去找把新工具？特别是在小伙伴们也都没摸过这把工具的时候？还有就是，我如何利用这把新工具和我的老工具一起来完成任务？

在NoSQL的概念里，真实的数据（类型、结构、模式）似乎不那么清晰可辨。每当被问起NoSQL是什么，软件商们总是喜欢给个抽象的答案：NoSQL对大数据和键-值型数的据存取非常合适。但这个答案的含义是什么？对身处前线的开发者来说，他们有一堆问题要解决，商人们给的答案并不能在开发人员在遇到选择障碍时帮助他们指明道路，即便选择不多，这些回答也很难帮助开发者找出值得冒险一试的方案。

你用NoSQL来解决什么问题？你用哪款NoSQL数据库？它如何帮助你？上面几个问题是我12月14号在线会议讨论的部分内容[详见](https://www1.gotomeeting.com/register/154378825)，会议纪要或许对你有帮助，但我更相信眼见为实这个道理，如果开发一些看得见的代码实例，那会对开发人员如何为系统选择合适的产品更有帮助

下面的这些用例是我在interwebs上一直强调的东西。抱歉我不能指出每个案例的具体来源（因为来源太多），但在文章结尾我会有一个文章列表。请放心使用它们。我只是简单的把用例从产品中独立出来，因为我有许多关于这些问题的用例，而且它们一眼就能看懂。当然，这些用例出现的先后次序和产品间的好坏没有关系。下面是一份主流[NoSQL产品列表](http://nosql-database.org/)。当然，这里也欢迎大家提供关于某个产品特殊功能的用例

## **基本应用场景**

下面这些都是常见的使用NoSQL的理由
*   **大**
NoSQL常被用来构建存放新数据主要部分，用以支持：大数据，大量用户，大量计算机，大供应链，大量科学运算，等等。当某个系统成长后，它很可能衍变为大型分布架构，这种情况一般就会用上NoSQL，虽然不是所有的NoSQL系统的设计目标都是大型系统。而大这个概念也可以涵盖很多方面，不仅仅是指硬盘空间
*   **海量写操作** 这是在Google的影响下形成的一种规范。大容量：[Facebook每月需要存储1.35亿消息](http://highscalability.com/blog/2010/11/16/facebooks-new-real-time-messaging-system-hbase-to-store-135.html)，[Twitter每天需要存储 7TB的数据](http://www.slideshare.net/kevinweil/nosql-at-twitter-nosql-eu-2010)并且这个需求以每年两倍的速度不断增长。因为数据太过庞大，因此会遇到这样的问题，80MB/s的操作，需要在一天内写入7TB的数据，因此需要在集群内做分布式写入，需要考虑键值存取，MapReduce，响应，容错率，一致性等其他相关问题。为了更快的写入速度，还需要启用in memory系统。
*   **快速键值存取** 这可能是大部分人脑海中闪过的关于NoSQL的第二大优点，当延迟成为一个重要问题，比如很难通过哈希键从内存或某个位置很深的磁碟里读出值。不是所有NoSQL产品的设计目标都是快速存取，例如，有些会更关心可靠性。不过，越来越多的产品都开始提供更好的memcached以回应用户的呼声。
* **可变架构和可变数据类型** NoSQL产品支持各种新的数据类型，这也是NoSQL带来的主要革新之一。它们包括：column-oriented, graph, advanced data structures, , document-oriented和key-value [参考]（http://www.csdn.net/article/2013-07-24/2816330-how-to-choose-nosql-db）。复杂对象能在不做很多关联的情况下被容易的存储下来。而开发人员也喜欢避免使用复杂的架构和ORM Frameworks。弱结构使得数据更容易调整。这一点和许多开发人员喜爱的数据结构，比如JSON十分相像。
*   **架构迁移** 弱架构性使得NoSQL数据库更容易处理架构迁移的问题。NoSQL的架构在某种程度上讲是动态的，因为它们都是在运行时生成的，所以一个应用的不同部分能有不同的架构视图。
*   **可写入性** 你希望能在任何情况下都写入成功么？那我们就需要能处理分区，CAP，最终一致性等所有这些问题。
*  **易维护，管理，操作** 这点其实依赖于指定产品的特性，但是许多NoSQL提供商都试图通过增加易用性来提高市场占有率。他们在提高易用性，精简管理和自动化操作上花了很多精力。比方说，用户可以不写代码，只通过少量的操作就实现系统的扩展
*   **单点故障处理**  不是所有的产品都有这个功能，但我们可以看见的一个趋势是：管理高可用性，自动负载均衡和调整集群尺寸的相关的配置越来越容易，而云服务商的相关服务也相当完美。
*   **可通用并行计算** MapReduce正在被整合到越来越多的产品中，这使得并行计算在未来将越来越多的被应用到开发方案中
*   **编程易用性** 获取数据将变得更加简单，现有关系模型虽然很符合终端用户的认知模型（如个人账户），但在开发人员的角度来看却不那么容易明白。让程序员的思维模型里装的往往是键，值，JSON，JS存储过程，HTTP等。NoSQL是为程序员而生，一切以开发为导向，而且因为程序员常常喜欢搞一个只给自己用的系统，这种情况下预算肯定越小越好，因此也就不可能高薪雇用DBA来搞定所有数据库问题。所以NoSQL通常被设计的简单易用，易于扩展，易于维护。
*   **为正确的问题挑选正确的数据模型** 不同的数据模型用来解决不同的问题。更多的精力将被花在这件事上。举例来说: 关系模型中操作wedging graph可能不那么顺利，那为什么不用图形数据库来解决图形问题？而我们也正见到越来越多类似的事情不断发生。
*   **[避免撞墙](http://blog.hypertable.com/?p=79)** 许多项目都会触及某种类型的边界，在边界内他们会竭尽所能的扩展他们的系统或让其正常工作，那下一步呢？这时，最顺利的状况就是能找到一款产品，部署后就能轻易突破极限，然后只要添加硬件就能以线性函数的形式扩展资源。有时，这几乎是不可能的。因为这几乎要定制开发所有内容。但我们现在真的已经看到这样的的产品被成功使用的案例。
*   [分布式系统支持](http://s3.amazonaws.com/cimlabs/Oredev-Enterprise-NoSQL.pdf). 不是所有人都会去关心非NoSQL系统的扩展性和性能边界，他们要的只是一个能无缝实施故障转移的分布式系统。NoSQL系统，因为其设计焦点扩展性，倾向扩展分区，倾向使用不太严格繁杂的一致性协议，因此我们可以很容易的在分布式方案中使用
*   [Tunable CAP tradeoffs](http://dbmsmusings.blogspot.com/2010/04/problems-with-cap-and-yahoos-little.html) NoSQL系统一般都是偏向CAP光谱上某一端的产品。关系型数据库偏向强一致性，这也意味着不能容忍分区非一致性。说到底，这其实只是一个商业决定，你的app真的需要高一致性？丢几个包有问题么？可用性和一致性孰轻孰重？掉线比数据出错导致的损失更大？这都是开发一款好产品时你要面对的选择。

## 更多应用场景

*   处理大型非事务型数据流：服务器日志，应用程序日志，数据库日志，点击流等
*   同步线上线下数据。[例如该文](http://www.readwriteweb.com/enterprise/2010/07/nosql-database-couchdb.php)
*   在全负载下获得更快的响应速度
*   为RDBMS减少复杂查询
*   对于像游戏这样的实时系统降低延迟是十分重要的
*   不同的应用程序对读写和数据一致性的要求是不同的，读写比例可能是对半开，也可能是95%的读操作或者95%的写操作。只读类型的应用需要高速，resiliency，简单查询和容忍轻微数据不同步，应用需要稳定的性能，可读写，简单查询，完整性验证。
*   提供负载均衡从而提高机器使用率
*   实时系统中的插入，修改，查询
*   动态建表
*   两层应用，需要低延迟的数据可以从NoSQL接口获取，而计算或者更新这样的操作则由Hadoop或优先级较低的应用来完成
*   顺序数据读取。需要选择正确的底层数据结构，A B-树或许不是最适合的模型
*   当系统需要更好的性能和可扩展性时为系统做切片。举例来说，用户需要高性能的登录系统，而这个目标可以通过将系统从主系统中独立出来来实现
*   缓存。为网站或其它应用提供高性能缓存，例如为大型强子对撞器的数据汇集系统做缓存
*   投票
*   实时页面计数
*   用户资料，Session
*   文档，分类信息，内容管理系统。
*   存档，在线存储大型数据流，面向文档型数据库因为有灵活的结构因此更容易应对数据结构变化
*   分析数据，利用MapReduce, Hive或者Pig去执行分析查询或扩展系统时，NoSQL类型的系统支持更高的写入速度
*   [集成异构数据](http://brehaut.net/blog/2010/couch_impedance#)，举例来说，在一个通用的层次处理不同类型的媒体数据
*   嵌入式系统。当人们不想部署SQL数据库时，他们会找一个更简单方案存储数据
*   经营类游戏，当你建立城镇时，建筑列会表快速增长，因此建筑表会被垂直切分出来，而当某人购买你的建筑时，你将拥有一个关于你的建筑的独立列表
*   [JPL](http://qconsf.com/sf2010/presentation/Out+of+This+World+Cloud+Computing)利用SimpleDB储存探测器计划的属性，被引用的完整资料则被放在S3
*  联邦执法机构利用信用卡[实时追踪美国公民](http://www.wired.com/threatlevel/2010/12/realtime/)
*  [欺诈检测](http://www.slideshare.net/SparsityTechnologies/dex-introduction#) 
*  [远程诊断](http://www.slideshare.net/SparsityTechnologies/dex-introduction#)
*   In-memory数据库在经常更新的情况下会被使用，比如[这个站点](http://news.ycombinator.com/item?id=16430) 显示每个人的最后一次在线时间，如果用户每30秒就发送一次在线状态，那么当5000人同时在线时你的系统就会遇到挑战
*  当需要连续处理高频数据流时，可以利用物化视图处理低频多分区查询
*  通过可编程的接口而不是ORM，对缓存的数据运行计算
*  [Unique a large dataset](http://wiki.apache.org/cassandra/UseCases) 使用简单的键-值列.
*  提高查询效率，同时支持回滚到[任意时间段](https://www.cloudkick.com/blog/2010/mar/02/4_months_with_cassandra/)
*  [计算两个大型数据集的交集](http://about.digg.com/blog/looking-future-cassandra) 这种情况下使用JOIN速度相当缓慢
*  [推特时间线](http://highscalability.com/scaling-twitter-making-twitter-10000-percent-faster)

## 数据分析应用场景

供职于Twitter的Kevin Weil提供了Hadoop的应用场景。在Twitter，这包括了对大量数据进行技术、取最极值等运算，还包括根据相关度、方差、流行度关联数据，以及分析大数据。Hadoop虽然只跟NoSQL粘了一点边，但通过它很容易看清什么类型的问题适合用NoSQL解决

*   每天有多少请求
*   平均延迟率有多少？95%？
*   根据响应代码分组：每小时分发率？
*   Twitter每天有多少检索？
*   它们来自哪里？
*   有多少独立查询？
*   有多少独立用户？
*   地理分布情况？
*   移动用户的用户习惯是否不同？
*   来自第三方客户端的用户习惯是否不同？
*   队列分析：如果一群用户都在同一天注册，那么看看他们接下来有什么不同
*   什么样的功能对有用户吸引力？
*   什么样的功能用户最常用？
*   检索纠正和检索建议（未来将要提供的功能）
*   长周期重复侦测（防止垃圾信息）
*   机器学习
*   如何检测机器人?

## 错误的应用场景

*   **OLTP** 除了VoltDB，NoSQL通常不支复杂的多对象事务处理。程序员应该假定流程非规范化，使用documents或使用其他复杂的策略（类似补偿性事务）
*   **数据完整性**  大部分NoSQL系统依赖应用程序来保证数据完整性而不是像SQL一样直接通过数据库定义来实现，这种情况最好用关系型数据库
*   **数据独立性** 当数据独立于程序时请用关系型数据库。使用NoSQL时通常是应用驱动一切，当然这也包括数据。但对关系模型来说一个数据实体的寿命拥有他们的企业几乎一样长，这个长度通常远超过任何独立的应用
*   **SQL语句** 如果你需要支持SQL，那么请尽量用SQL数据库，不过现在原来越多的NoSQL产品开始提供SQL语法的接口
*   **Ad-hoc queries** 当你需要预先对你无法预测的数据做出实时响应，这种情况最好用关系型数据库
*   **复杂关系** 一些NoSQL系统支持关系，但这种情况最好用关系型数据库
*   **成熟稳定** 关系型数据库这方面仍余威不减。人们更熟悉它们的工作原理和使用方法，对它们的可靠性更有信心，关系型数据库的开发人员和工具也更多

## Redis应用场景

Redis的独特之处在于它很像一个数据结构服务器，更多内容详见这里[people are excited to share](http://simonwillison.net/static/2010/redis-tutorial/)

## VoltDB 应用场景

VoltDB是一个关系型数据库而非传统意义的NoSQL数据库，但我觉得根据这篇文章 [radical design perspective](http://highscalability.com/blog/2010/6/28/voltdb-decapitates-six-sql-urban-myths-and-delivers-internet.html) 他们在未来将更像NoSQL而不是Orcal这样的数据库。

## 参考文章

*   [Hacker News](http://news.ycombinator.com/item?id=1976429) and [Reddit](http://www.reddit.com/r/programming/comments/ehaeq/high_scalability_what_the_heck_are_you_actually/) thread on this Post. More use case suggestions there. 
*   [List of NoSQL Systems](http://nosql-database.org/)
*   [Pig](http://research.yahoo.com/node/90) - infrastructure to support ad-hoc analysis of very large data sets.
*   [Digging Deeper Into Data With Hadoop](http://gigaom.com/2009/06/07/digging-deeper-into-data-with-hadoop/) By Gary Orenstein 
*   [NoSQL East 2009 - Summary of Day 1](http://journal.uggedal.com/nosql-east-2009---summary-of-day-1) by Eivind Uggedal
*   [The “NoSQL” approach: struggling to see the benefits](http://nsaunders.wordpress.com/2010/08/05/the-nosql-approach-struggling-to-see-the-benefits/) by Neil Saunders 
*   [Design Patterns for DistributedNon-Relational Databases](http://static.last.fm/johan/nosql-20090611/intro_nosql.pdf ) by Todd Lipcon.
*   [The Future Is Big Data in the Cloud](http://gigaom.com/2009/10/25/the-future-is-big-data-in-the-cloud/) By Ping Li
*   [One size does not fit all: “document stores”, “nosql databases” , ODBMSs](http://www.odbms.org/blog/2010/02/document-stores-nosql-databases-odbmss/) by Roberto V. Zicari
*   [NoSQL is for niches](http://www.zdnet.com/blog/open-source/nosql-is-for-niches/6128) By Dana Blankenhorn.
*   [MongoDB Use Cases](http://www.mongodb.org/display/DOCS/Use+Cases)
*   [MongoDB and Ecommerce](http://kylebanker.com/blog/2010/04/30/mongodb-and-ecommerce/) by  Kyle Banker
*   [Archiving - a good MongoDB use case?](http://blog.mongodb.org/post/1200539426/archiving-a-good-mongodb-use-case)
*   [Five Reasons to Use NoSQL](http://facility9.com/2010/09/16/five-reasons-to-use-nosql) by Jeremiah Peschka
*   [The Business Case for NoSQL, NoETL and NoProblems](http://www.itbusinessedge.com/cm/blogs/lawson/the-business-case-for-nosql-noetl-and-noproblems/?cs=40284) by Loraine Lawson
*   [Is It Time For NoETL](http://intelligent-enterprise.informationweek.com/blog/archives/2010/03/is_it_time_for.html;jsessionid=3YOYNBBPASI5VQE1GHRSKH4ATMY32JVN)? by Seth Grimes
*   [Holy Large Hadron Collider, Batman!](http://blog.mongodb.org/post/660037122/holy-large-hadron-collider-batman)
*   [I Can't Wait for NoSQL to Die](http://teddziuba.com/2010/03/i-cant-wait-for-nosql-to-die.html) by Ted Dziuba.
*   [NoSQL Basics, Benefits and Best-Fit Scenarios](http://intelligent-enterprise.informationweek.com/channels/information_management/showArticle.jhtml;jsessionid=ZLGYK42ZWIZSXQE1GHOSKH4ATMY32JVN?articleID=227701021&pgno=2#) by Curt Monash 
*   [Redis Tutorial](http://simonwillison.net/static/2010/redis-tutorial/) by Simon Willison
*   [Why I Like Redis](http://simonwillison.net/2009/Oct/22/redis/ ) by Simon Willison
*   [Redis: Lightweight key/value Store That Goes the Extra Mile](http://www.linux-mag.com/id/7496) by Jeremy Zawodny
*   [Remote Dictionary Server](http://www.slideshare.net/ezmobius/redis-remote-dictionary-server)
*   [Is NoSQL for me? I’m just a small fish](http://devlicio.us/blogs/hadi_hariri/archive/2010/11/24/is-nosql-for-me-i-m-just-a-small-fish.aspx) by Hadi Hariri
*   [Why Big Enterprises are Interested in NoSQL](http://s3.amazonaws.com/cimlabs/Oredev-Enterprise-NoSQL.pdf) by Jon Moore
*   [Visual Guide to NoSQL Systems](http://blog.nahurst.com/visual-guide-to-nosql-systems) by Nathan Hurst
*   [You Can't Sacrifice Partition Tolerance](http://codahale.com/you-cant-sacrifice-partition-tolerance/) by Coda Hale
*   [NoSQL Netflix Use Case Comparison](http://perfcap.blogspot.com/) by Adrian Cockcroft
*   [Comparison guide of horizontally scalable datastores](http://cattell.net/datastores/Datastores.pdf) by Rick Cattell
*   [Weak Consistency and CAP Implications](http://www.igvita.com/2010/06/24/weak-consistency-and-cap-implications/) by Ilya Grigorik 
*   [Schema-Free MySQL vs NoSQL](http://www.igvita.com/2010/03/01/schema-free-mysql-vs-nosql/)  by Ilya Grigorik 
*   [Neo4j - 5 Cool Graph Examples](http://www.slideshare.net/peterneubauer/neo4j-5-cool-graph-examples-4473985)
*   [Has anyone used Graph-based Databases](http://stackoverflow.com/questions/1000162/have-abyone-used-graph-based-databases-http-neo4j-org)
*   [Neotechnology Uses Cases](http://neotechnology.com/customers)
*   [CouchOne Ataxo Case Study](http://www.couchone.com/case-study-ataxo)
*   [NOSQL: scaling to size and scaling to complexity](http://blogs.neotechnology.com/emil/2009/11/nosql-scaling-to-size-and-scaling-to-complexity.html) by Emil Eifrem
*   [NoSQL, NoProblem (Not Really… but it’s still awesome)](http://jeremypinkham.com/post/1587967351/nosql-noproblem-not-really-but-its-still-awesome#) by Jeremy Pinkham
*   [An Expert's Guide to Oracle Technology](http://it.toolbox.com/blogs/oracle-guide/acid-vs-base-25938) by Lewis Cunningham
*   [NoSQL vs SQL, Why Not Both?](http://www.cloudbook.net/resources/stories/nosql-vs-sql-why-not-both) By Alaric Snell-Pym 
*   [Databases: relational vs object vs graph vs document](http://www.cbsolution.net/roller/ontarget/entry/databases_relational_vs_object_vs) by On Target 
*   [Use cases are driving the divergence, and the convergence, of NoSQL solutions](http://blog.membase.com/use-cases-are-driving-divergence-and-convergence-nosql-solutions) by James  Phillips
*   [The beginning of the end of NoSQL](http://blogs.the451group.com/information_management/2010/11/12/the-beginning-of-the-end-of-nosql/) by Matthew Aslett 
*   [Going NoSQL with MongoDB](http://msdn.microsoft.com/en-us/magazine/ee310029.aspx) by Ted Neward
*   [NoSQL Ecosystem](http://www.rackspacecloud.com/blog/2009/11/09/nosql-ecosystem/) by Jonathan Ellis
*   [The New Dimension of NoSQL Scalability: Complexity](http://nosql.mypopescu.com/post/287581423/the-new-dimension-of-nosql-scalability-complexity) by Alex Popescu
*   [Quick Reference to Alternative data storages](http://themindstorms.blogspot.com/2009/05/quick-reference-to-alternative-data.html) by Alex Popescu
*   [6 Reasons Why Relational Database Will Be Superseded](http://www.havemacwillblog.com/2008/11/6-reasons-why-relational-database-will-be-superseded/)  by Robin Bloor
*   [NoSQL Misconceptions](http://www.viget.com/extend/nosql-misconceptions/) by Ben Scofield
*   [SQL Databases Don't Scale](http://adam.heroku.com/past/2009/7/6/sql_databases_dont_scale/) by Adam Wiggins 
*   [“One Size Fits All”: An Idea Whose Time Has Come and Gone](http://www.cs.brown.edu/~ugur/fits_all.pdf) by Michael Stonebraker and Uğur Çetintemel
*   [Future of RDBMS is RAM Clouds & SSD](http://www.igvita.com/2009/12/07/future-of-rdbms-is-ram-clouds-ssd/) by Ilya Grigorik
*   [To scale or not to scale: Key/Value, Document, SQL, JPA](http://www.slideshare.net/uri1803/to-scale-or-not-to-scale-keyvalue-document-sql-jpa-whats-right-for-my-app#)  by Uri Cohen    
