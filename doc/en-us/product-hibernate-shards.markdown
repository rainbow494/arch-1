## [Product: Hibernate Shards](/blog/2007/7/24/product-hibernate-shards.html)

    

    

If you want to adopt a shard architecture, but don't want to start from scratch, you may want to consider Hibernate's sharding system.

Hibernate Shards is a framework that is designed to encapsulate and minimize this complexity by adding support for horizontal partitioning to Hibernate Core.  

[Hibernate Shards](https://www.hibernate.org/414.html) key features:

*   Standard Hibernate programming model - Hibernate Shards allows you to continue using the Hibernate APIs you know and love: SessionFactory, Session, Criteria, Query. If you already know how to use Hibernate, you already know how to use Hibernate Shards.
*   Flexible sharding strategies - Distribute data across your shards any way you want. Use one of the default strategies we provide or plug in your own application-specific logic.
*   Support for virtual shards - Think your sharding strategy is never going to change? Think again. Adding new shards and redistributing your data is one of the toughest operational challenges you will face once you've deployed your shard-aware application. Hibernate Sharding supports virtual shards, a feature designed to simplify the process of resharding your data.
*   Free/open source - Hibernate Shards is licensed under the LGPL (Lesser GNU Public License)

    