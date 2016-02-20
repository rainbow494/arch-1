## [Product: Hbase](/blog/2009/7/2/product-hbase.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, July 2, 2009 at 12:43AM</span></div>

<div class="body">

**Update 3:** Presentation from the [NoSQL Conference](http://blog.oskarsson.nu/2009/06/nosql-debrief.html): [slides](http://static.last.fm/johan/nosql-20090611/hbase_nosql.pdf), [video](http://vimeo.com/5198411).  
**Update 2:** Jim Wilson helps with the [Understanding HBase and BigTable](http://jimbojw.com/wiki/index.php?title=Understanding_Hbase_and_BigTable) by explaining them from a "conceptual standpoint."  
**Update:** InfoQ interview: [HBase Leads Discuss Hadoop, BigTable and Distributed Databases](http://www.infoq.com/news/2008/04/hbase-interview). "MapReduce (both Google's and Hadoop's) is ideal for processing huge amounts of data with sizes that would not fit in a traditional database. Neither is appropriate for transaction/single request processing."  

[Hbase](http://hadoop.apache.org/hbase/) is the open source answer to BigTable, Google's highly scalable distributed database. It is built on top of Hadoop ([product](http://highscalability.com/product-hadoop)), which implements functionality similar to Google's GFS and Map/Reduce systems. 

Both Google's GFS and Hadoop's HDFS provide a mechanism to reliably store large amounts of data. However, there is not really a mechanism for organizing the data and accessing only the parts that are of interest to a particular application.  

Bigtable (and Hbase) provide a means for organizing and efficiently accessing these large data sets.  

Hbase is still not ready for production, but it's a glimpse into the power that will soon be available to your average website builder.  

Google is of course still way ahead of the game. They have huge core competencies in data center roll out and they will continually improve their stack.  

It will be interesting to see how these sorts of tools along with [Software as a Service](http://highscalability.com/tags/saas) can be leveraged to create the next generation of systems.

</div>