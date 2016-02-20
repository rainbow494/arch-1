## [Anti-RDBMS: A list of distributed key-value stores](/blog/2009/8/5/anti-rdbms-a-list-of-distributed-key-value-stores.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, August 5, 2009 at 12:49AM</span></div>

<div class="body">

**Update 8:** [Introducing MongoDB](http://www.linux-mag.com/cache/7530/1.html) by [Eliot Horowit](http://www.linux-mag.com/author/734). 

**Update 7:** [The Future of Scalable Databases](http://www.ajatus.in/2009/10/the-future-of-scalable-databases/#) by Robin Mathew.

**Update 6:** [NoSQL : If Only it Was that Easy](http://bjclark.me/2009/08/04/nosql-if-only-it-was-that-easy). BJ Clark lays down the law on which databases are scalable: Tokyo - NO, Redis - NO, Voldemort - YES, MongoDB - Not Yet, Cassandra - Probably, Amazon S3 - YES * 2, MySQL - NO. _The real thing to point out is that if you are being held back from making something super awesome because you can’t choose a database, you are doing it wrong._  
**Update 5:** [Exciting stuff happening in Japan at this Key-Value Storage meeting in Tokyo](http://blog.plathome.com/2009/02/first-key-value-storage-meeting-held.html). Presentations on Groonga, Senna, Lux IO, Tokyo-Cabinet, Tx, repcached, Kai, Cagra, kumofs, ROMA, and Flare.

**Update 4:** [NoSQL and the Relational Model: don’t throw the baby out with the bathwater](http://matthew.yumptious.com/2009/07/databases/nosql-and-the-relational-model-dont-throw-the-baby-out-with-the-bathwater/) by Matthew Willson. _So my key point is, this kind of modelling is WORTH DOING, regardless of which database tool you end up using for physical storage._  
**Update 3:** [Choosing a non-relational database; why we migrated from MySQL to MongoDB](http://blog.boxedice.com/2009/07/25/choosing-a-non-relational-database-why-we-migrated-from-mysql-to-mongodb/#). An illuminating article explaining why Boxed Ice move to MongoDB over MySQL and other NoSQL options: easy install, PHP support, replication and master-master support, good doc, auto sharding on the road map. They still use MySQL for billing.  
**Update 2:** They are now called NoSQL databases. So keep up! Eric Lai wrote a good article in Computerworld [No to SQL? Anti-database movement gains steam](http://www.computerworld.com/action/article.do?command=printArticleBasic&taxonomyName=Databases&articleId=9135086&taxonomyId=173#) about the phenomena. There was even a [NoSQL conference](http://blog.oskarsson.nu/2009/06/nosql-debrief.html). It was unfortunately full by the time I wanted to sign up, but there are presentations by all the major players. Nice [Hacker News thread](http://news.ycombinator.com/item?id=683807) too.  
**Update:** [Some Notes on Distributed Key Stores](http://randomfoo.net/2009/04/20/some-notes-on-distributed-key-stores) by Leonard Lin. What's the best way to handle a fast growing system with 100M items that requires low latency and lots of inserts? Leanord takes a trip through several competing systems. The winner was: Tokyo Cabinet.  

Richard Jones has put together a [very nice list of various key-value stores](http://www.metabrew.com/article/anti-rdbms-a-list-of-distributed-key-value-stores/) around the internets. The list includes: Project Voldemort, Ringo, Scalaris, Kai, Dynomite, MemcacheDB, ThruDB, CouchDB, Cassandra, HBase, and Hypertable. Richard also includes some commentary and their basic components (language, fault tolerance, persistence, client protocol, data model, docs, community).  

There's an excellent discussion in the comments of Paxos vs Vector Clock techniques for synchronizing writes in the face of network failures.

</div>