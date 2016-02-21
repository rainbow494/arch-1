## [Product: Hadoop](/blog/2009/5/17/product-hadoop.html)

    

    

**Update 5:** [Hadoop Sorts a Petabyte in 16.25 Hours and a Terabyte in 62 Seconds](http://developer.yahoo.net/blogs/hadoop/2009/05/hadoop_sorts_a_petabyte_in_162.html) and has its [green cred questioned](http://databeta.wordpress.com/2009/05/14/bigdata-node-density/) because it took 40 times the number of machines Greenplum used to do the same work.  
**Update 4:** [Introduction to Pig](http://www.cloudera.com/hadoop-training-pig-introduction). Pig allows you to skip programming Hadoop at the low map-reduce level. You don't have to know Java. Using the Pig Latin language, which is a scripting data flow language, you can think about your problem as a data flow program. 10 lines of Pig Latin = 200 lines of Java.  
**Update 3**: Scaling Hadoop to [4000 nodes at Yahoo!](http://developer.yahoo.com/blogs/hadoop/2008/09/scaling_hadoop_to_4000_nodes_a.html). 30,000 cores with nearly 16PB of raw disk; sorted 6TB of data completed in 37 minutes; 14,000 map tasks writes (reads) 360 MB (about 3 blocks) of data into a single file with a total of 5.04 TB for the whole job.  
**Update 2**: Hadoop [Summit and Data-Intensive Computing Symposium Videos and Slides](http://research.yahoo.com/node/2104). Topics include: Pig, JAQL, Hbase, Hive, Data-Intensive Scalable Computing, Clouds and ManyCore: The Revolution, Simplicity and Complexity in Data Systems at Scale, Handling Large Datasets at Google: Current Systems and Future Directions, Mining the Web Graph. and Sherpa: Hosted Data Serving.  
**Update**: [Kevin Burton](http://developer.yahoo.com/blogs/hadoop/) points out Hadoop now has a [blog](http://developer.yahoo.com/blogs/hadoop/) and an [introductory video](http://us.dl1.yimg.com/download.yahoo.com/dl/ydn/eric14_ipod.m4v) staring Beyonce. Well, the Beyonce part isn't quite true.  

[Hadoop](http://hadoop.apache.org/) is a framework for running applications on large clusters of commodity hardware using a computational paradigm named map/reduce, where the application is divided into many small fragments of work, each of which may be executed on any node in the cluster. It replicates much of Google's [stack](http://highscalability.com/google-architecture), but it's for the rest of us. [Jeremy Zawodny](http://developer.yahoo.net/blog/archives/2007/07/yahoo-hadoop.html) has a wonderful overview of why Hadoop is important for large website builders:

_For the last several years, every company involved in building large web-scale systems has faced some of the same fundamental challenges. While nearly everyone agrees that the "divide-and-conquer using lots of cheap hardware" approach to breaking down large problems is the only way to scale, doing so is not easy.  

The underlying infrastructure has always been a challenge. You have to buy, power, install, and manage a lot of servers. Even if you use somebody else's commodity hardware, you still have to develop the software that'll do the divide-and-conquer work to keep them all busy  

It's hard work. And it needs to be commoditized, just like the hardware has been._  

Hadoop also provides a distributed file system that stores data on the compute nodes, providing very high aggregate bandwidth across the cluster. Both map/reduce and the distributed file system are designed so that node failures are automatically handled by the framework. Hadoop has been demonstrated on clusters with 2000 nodes. The current design target is 10,000 node clusters.  

The obvious question of the day is: should you build your website around Hadoop? I have no idea.  

There seems to be a few types of things you do with lots of data: process, transform, and serve.  

Yahoo literally has petabytes of log files, web pages, and other data they process. Process means to calculate on. That is: figure out affinity, categorization, popularity, click throughs, trends, search terms, and so on. Hadoop makes great sense for them for the same reasons it does Google. But does it make sense for your website?  

If you are YouTube and you have petabytes of media to serve, do you really need map/reduce? Maybe not, but the clustered file system is great. You get high bandwidth with the ability to transparently extend storage resources. Perfect for when you have lots of stuff to store.  

YouTube would seem like it could use a distributed job mechanism, like you can build with [Amazon's services](http://highscalability.com/build-infinitely-scalable-infrastructure-100-using-amazon-services). With that you could create thumbnails, previews, transcode media files, and so on.  

When they have [Hbase](http://highscalability.com/product-hbase) up and running that could really spike adoption. Everyone needs to store structured data in a scalable, reliable, highly performing data store. That's an exciting prospect for me.  

I can't wait for experience reports about "normal" people, familiar with a completely different paradigm, adopting this infrastructure. I wonder what animal O'Reilly will use on their Hadoop cover?

## See Also

*   Site: [Hadoop](http://lucene.apache.org/hadoop/)*   [Open Source Distributed Computing: Yahoo's Hadoop Support](http://developer.yahoo.net/blog/archives/2007/07/yahoo-hadoop.html) by Jeremy Zawodny*   [Yahoo!'s bet on Hadoop](http://radar.oreilly.com/archives/2007/08/yahoos_bet_on_h.html) by Tim O'Reilly*   [Hadoop Presentations](http://wiki.apache.org/lucene-hadoop/HadoopPresentations)*   [Running Hadoop MapReduce on Amazon EC2 and Amazon S3](http://developer.amazonwebservices.com/connect/entry.jspa?externalID=873&categoryID=112)    