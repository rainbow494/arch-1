## [Product: dbShards - Share Nothing. Shard Everything.](/blog/2010/6/23/product-dbshards-share-nothing-shard-everything.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, June 23, 2010 at 7:37AM</span></div>

<div class="body">

![](http://www.codefutures.com/img/dbshards-shardit.gif)

I met the CodeFutures folks, makers of [dbShards](http://www.codefutures.com/dbshards/), at [Gluecon](http://www.gluecon.com). They occupy an interesting niche in the database space, somewhere between [NoSQL](http://highscalability.com/blog/category/nosql), which jettisons everything SQL, and [high end analytics platforms](http://www.dbms2.com/) that completely rewrite the backend while keeping a SQL facade.

High concept: I think of dbShards as a sort of commercial OLTP mashup of features from [HSCALE](/blog/2008/5/5/hscale-handling-200-million-transactions-per-month-using-tra.html) (partitioning) + [MySQL Proxy](https://launchpad.net/mysql-proxy) (transparent intermediate layer) + [Memcached](http://highscalability.com/blog/category/memcached) (client side sharding) + [Gigaspaces](http://www.gigaspaces.com/) (parallel query) + MySQL (transactions).

You may find dbShards interesting if you are looking to keep SQL, need scale out writes and reads, need out of the box parallel query capabilities, and would prefer to use a standard platform like MySQL as a base. To learn more about dbShards I asked Cory Isaacson (CEO and CTO) a few devastatingly difficult questions (not really).

## Who are you, what is dbShards, and what problem was dbShards created to solve?

[Click to read more ...](/blog/2010/6/23/product-dbshards-share-nothing-shard-everything.html)

</div>