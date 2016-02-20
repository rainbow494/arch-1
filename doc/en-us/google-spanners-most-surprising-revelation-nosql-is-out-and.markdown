## [Google Spanner's Most Surprising Revelation: NoSQL is Out and NewSQL is In](/blog/2012/9/24/google-spanners-most-surprising-revelation-nosql-is-out-and.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, September 24, 2012 at 9:12AM</span></div>

<div class="body">

![](http://farm2.static.flickr.com/1278/5179816576_2b186d41b3.jpg)

Google recently released a [<span>paper on Spanner</span>](http://research.google.com/archive/spanner.html)<span>, their planet enveloping tool for organizing the world’s monetizable information. Reading the Spanner paper I felt it had that chiseled in stone feel that all of Google’s best papers have. An instant classic. Jeff Dean foreshadowed Spanner’s humungousness as early as</span> [<span>2009</span>](http://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf)<span>.  Now Spanner seems fully online, just waiting to handle “millions of machines across hundreds of datacenters and trillions of database rows.” Wow.</span>  

<span>The Wise have yet to weigh in on Spanner en masse. I look forward to more insightful commentary. There’s a lot to make sense of. What struck me most in the paper was a deeply buried section essentially describing Google’s motivation for shifting away from NoSQL and to</span> [<span>NewSQL</span>](http://www.readwriteweb.com/cloud/2011/04/the-newsql-movement.php)<span>. The money quote:</span>

> <span>**We believe it is better to have application programmers deal with performance problems due to overuse of transactions as bottlenecks arise, rather than always coding around the lack of transactions.**</span>

<span>This reads as ironic given Bigtable helped kickstart the NoSQL/eventual consistency/key-value revolution.</span>

<span>We see most of the criticisms leveled against NoSQL turned out to be problems for Google too. </span><span>Only Google solved the problems in a typically Googlish way, through the fruitful melding of advanced theory and technology. The result: programmers get the real transactions, schemas, and query languages many crave along with the scalability and high availability they require.</span>  

<span>The full quote:</span>

> <span>Spanner exposes the following set of data features to applications: a data model based on schematized semi-relational tables, a query language, and general purpose transactions. The move towards supporting these features was driven by many factors. The need to support schematized semi-relational tables and synchronous replication is supported by the popularity of Megastore [5].</span>
> 
> <span>At least 300 applications within Google use Megastore (despite its relatively low performance) because its data model is simpler to manage than Bigtable’s, and because of its support for synchronous replication across datacenters. (Bigtable only supports eventually-consistent replication across datacenters.) Examples of well-known Google applications that use Megastore are Gmail, Picasa, Calendar, Android Market, and AppEngine.</span>
> 
> <span>The need to support a SQLlike query language in Spanner was also clear, given the popularity of Dremel [28] as an interactive data analysis tool. Finally, the lack of cross-row transactions in Bigtable led to frequent complaints; Percolator [32] was in part built to address this failing.</span>
> 
> <span>Some authors have claimed that general two-phase commit is too expensive to support, because of the performance or availability problems that it brings [9, 10, 19].</span> <span>We believe it is better to have application programmers deal with performance problems due to overuse of transactions as bottlenecks arise, rather than always coding around the lack of transactions.</span> <span>Running two-phase commit over Paxos mitigates the availability problems.</span>

<span>What was the cost? It appears to be latency, but apparently not of the crippling sort, though we don’t have benchmarks. In any case, Google thought dealing with latency was an easier task than programmers hacking around the lack of transactions. I find that just fascinating. It brings to mind so many years of RDBMS vs NoSQL arguments it’s not even funny.</span>  

<span>I wonder if Amazon could build their highly available shopping cart application, said to a be a motivator for</span> [<span>Dynamo</span>](http://www.allthingsdistributed.com/2007/10/amazons_dynamo.html)<span>, on top of Spanner?</span>

## <span>Is Spanner the Future in the Same Way Bigtable was the Future?</span>

<span>Will this paper spark the same revolution that the original</span> [<span>Bigtable</span>](http://en.wikipedia.org/wiki/BigTable) <span>paper caused? Maybe not. As it is Open Source energy that drives these projects, and since few organizations need to support transactions on a global scale (yet), whereas quite a few needed to do something roughly Bigtablish, it might be awhile before we see a parallel Open Source development tract.</span>  

<span>A complicating factor for an Open Source effort is that Spanner includes the use of GPS and Atomic clock hardware. Software only projects tend to be the most successful. Hopefully we’ll see clouds step it up and start including higher value specialized services. A cloud wide timing plane should be a base feature. But we are still stuck a little bit in the cloud as Internet model instead of the cloud as a highly specialized and productive software container.</span>  

<span>Another complicating factor is that as Masters of Disk it’s not surprising Google built Spanner on top of a new Distributed File System called</span> [<span>Colossus</span>](http://highscalability.com/blog/2010/9/11/googles-colossus-makes-search-real-time-by-dumping-mapreduce.html)<span>. Can you compete with Google using disk? If you go down the Spanner path and commit yourself to disk, Google already has many years lead time on you and you’ll never be quite as good. It makes more sense to skip a technological generation and move to RAM/SSD as a competitive edge. Maybe this time Open Source efforts should focus elsewhere, innovating rather than following Google?</span>

## <span>Related Articles</span>

*   [<span>Cloudant Labs: On Google Spanner</span>](https://cloudant.com/blog/cloudant-labs-on-google-spanner/) <span>by Mike Miller</span>
*   [<span>Thread on G+</span>](https://plus.google.com/118227548810368513262/posts/J23q42YpYZ5) <span>by Jeff Dean</span>
*   [<span>SPANNER : GOOGLE’S GLOBALLY DISTRIBUTED DATABASE</span>](http://www.systemswemake.com/papers/spanner) <span>by Srihari Srinivasan</span>
*   [<span>On Slashdot</span>](http://developers.slashdot.org/story/12/09/18/041239/google-spanner-first-globally-scalable-database-with-external-consistency)
*   [<span>On Hacker News</span>](http://news.ycombinator.com/item?id=4526710)
*   [<span>F1 - The Fault-Tolerant Distributed RDBMS Supporting Google's Ad Business</span>](http://research.google.com/pubs/pub38125.html)
*   [<span>Spanner: Google's Globally-Distributed Database</span>](http://blog.sacaluta.com/2012/09/spanner-googles-globally-distributed.html) <span>by Rodrigo De Castro</span>
*   [<span>Google Spans Entire Planet With GPS-Powered Database</span>](http://www.wired.com/wiredenterprise/2012/09/google-spanner/all/) <span> by Cade Metz</span>
*   [Facebook Tackles (Really) Big Data With ‘Project Prism’](http://www.wired.com/wiredenterprise/2012/08/facebook-prism/) by Cade Metz
*   <div id="_mcePaste">[Alex Lloyd - Building Spanner](http://vimeo.com/43759726)</div>

</div>