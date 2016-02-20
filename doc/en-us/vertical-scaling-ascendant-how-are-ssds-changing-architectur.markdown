## [Vertical Scaling Ascendant - How are SSDs Changing  Architectures?](/blog/2012/7/25/vertical-scaling-ascendant-how-are-ssds-changing-architectur.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, July 25, 2012 at 9:17AM</span></div>

<div class="body">

![](http://farm9.staticflickr.com/8421/7644126224_24ffc4885f_m.jpg)

With Amazon[ <span></span> <span>announcing new High I/O 2TB SSD instances</span>](http://aws.typepad.com/aws/2012/07/new-high-io-ec2-instance-type-hi14xlarge.html) <span>the age of SSD has almost arrived. I say almost because the $27K a year price tag for the hi1.4xlarge on demand instance type is outside the budget of many. Yet even at the full on demand rate the</span> <span>price per IOP for the high IO instance is attractive:</span> <span>27 cents ($27K/100K IOPS) per vs</span> [<span>$1.25 for disk</span>](http://www.youtube.com/watch?v=zQdDi9pdf3I)<span>.</span> <span></span> <span>With the obvious benefits of giant SSD machines combined with 10 Gbps networking, it’s interesting to consider: what architecture decisions might you make differently in the future?</span>

## <span>More Headroom for Vertical Scaling Simplifies Everything</span>

<span>The beauty of higher hardware performance is it shifts effort away from the programmer which allows developers to focus on the business of business, minimizing trickeration. This has always been the allure of</span> [<span>vertical scaling</span>](http://highscalability.com/blog/category/vertical-scaling) <span>and is well realized by SSDs through a combination of high throughput, low latencies, and just as important, high densities.</span>  

<span>We have a few early examples showing the performance punch of the new High IO instance:</span>

*   <span>Mike Krieger of Instagram reports:</span>
    *   [<span>Very cool</span>](https://twitter.com/mikeyk/status/227655820362518529)<span>--striping the SSD drives together in the new EC2 instance type yields ~0.5 ms avg latency even under pretty heavy load.</span>
    *   [<span>Graph of the day</span>](https://twitter.com/mikeyk/status/226826115644264449)<span>: disk latency for EC2 SSD drive (in orange) vs EBS (in green), while the SSD does about 4000x the IOPS</span> [<span>http://d.pr/i/3QZ2</span>](http://d.pr/i/3QZ2)
*   <span>Colin Howe from Conversocial</span> [<span>wrote about their experiences</span>](http://www.colinhowe.co.uk/2012/jul/23/ssds-on-aws--impact-on-conversocial/) <span>transferring MongoDB to a hi1.4xlarge and found:</span>
    *   <span>Average response time is</span> <span>43% faster</span> <span>(392ms to 274ms).</span>
    *   <span>One random IO heavy view data is now</span> <span>74% faster</span> <span>(877ms to 504ms).</span>
    *   <span>CPU usage went from around iowait 90% to 3%.</span>
*   Netflix has already [published a very thorough post](http://techblog.netflix.com/2012/07/benchmarking-high-performance-io-with.html) on the wonderfulness of SSD for both performance and taming the [long latency tail](http://highscalability.com/blog/2012/3/12/google-taming-the-long-latency-tail-when-more-machines-equal.html):
    *   They see [100K IOPS or 1GByte/sec](http://news.ycombinator.com/item?id=4266747) on a untuned system.
    *   <span>The hi1.4xlarge configuration is about half the system cost for the same throughput.</span>
    *   <span>The mean read request latency was reduced from 10ms to 2.2ms.</span>
    *   <span>The 99th percentile request latency was reduced from 65ms to 10ms.</span>

<span>But not all is bliss in the garden. Jeff Darcy wrote in</span> [<span>Testing [GlusterFS] on Amazon’s New SSD Instances</span>](http://www.gluster.org/2012/07/testing-on-amazons-new-ssd-instances/) <span> that they’ve found real world performance unimpressive and even worse in some cases, concluding:</span>

> They’re certainly a welcome improvement for this worst-case kind of workload, but I’ve seen their ilk before so the only thing that’s really new to me is the high price tag.

## <span>Practical Implications of the High IO Instances</span>

*   **Turbocharging**. When your performance is going to heck and you need help now, the High IO instance is a little light in the dark. Maciej Dobrzanski  from dba square [<span>ran some benchmarks</span>](http://www.dbasquare.com/2012/07/20/amazon-ec2-now-powered-by-high-performance-storage-ssd/) and concluded:  This can actually make EC2 usable even for large and busy MySQL databases, which up until now often wasn’t a viable option.
*   <span>**Delay horizontal architecture/sharding**</span><span>. That step to a</span> [<span>horizontal architecture</span>](http://highscalability.com/blog/2011/10/24/stackexchange-architecture-updates-running-smoothly-amazon-4.html) <span>is a big one. If you are worried about all your data fitting into RAM, 2 TBs is a lot of headroom. Do you still really need a disk? Stuff it all on SSDs.</span>
*   <span>**Less time spent optimizing**</span><span>. SSDs are often used as a magic performance sauce. Use an SSD and performance instantly improves. That, along with the ability to use a vertical architecture means programmers can do much of less of the finicky optimization type programming that wastes so much time.</span>
*   <span>**Remove caching layers**</span><span>. Netflix was able to remove a memcached layer on 48 instances and replace it with 15 I/O instances with no intervening cache, while maintaining a lower overall latency. Using RAM to offload IO from the database was no longer necessary with the SSD backed Cassandra nodes and there was still plenty of CPU headroom remaining on the nodes.</span>
*   <span>**Large memory algorithms**</span><span>.</span> [<span>Mbreeze</span>](http://news.ycombinator.com/item?id=4264962) <span>gives a good example why this is helpful:</span>
    *   <span>I do genome mapping where our indexes won't entirely fit in memory. It would be very handy to be able to spin up a few of these instances, load the indexes from an EBS volume onto the local SSDs, then run for a couple of hours or so. This is a very I/O intensive job that we need to run about once a week, but then the rest of the time could be idle. SSDs would make our jobs run significantly faster. So much so that we've toyed with the idea of adding SSDs to our in-house cluster, but couldn't quite justify the costs. This might actually shift the cost savings to get our lab to migrate to EC2 as opposed to our in-house or university cluster.</span>
*   <span>**Peak usage handling**</span><span>. From </span>[<span>pdeshen</span>](http://pdeschen/)<span>:</span>
    *   <span>We use ec2 within our telecom infrastructure which manages peaks of several thousands of calls with full duplex call recording on (think IO here) We are using the elasticity of ec2 to scale on demand. This kind of instance is a nice addition for us as far as our auto scaling is concerned.</span>
*   <span>**Skip EBS entirely**</span><span>.</span> [<span>Fizzer</span>](http://news.ycombinator.com/item?id=4264925) <span>makes an interesting point about EBS:</span>
    *   <span>This is a game changer for big sites on EC2\. The key word here is local: 2 TB of local SSD-backed storage. In this</span> [<span>video</span>](http://www.10gen.com/presentations/MongoNYC-2012/MongoDB-at-foursquare)<span>, Foursquare says the biggest problem they're facing with EC2 is consistency in I/O performance. They say that the instance storage simply isn't fast enough for them, and while EBS is fast enough when RAIDed, it isn't consistent since it isn't local (EBS is traffic goes over the network). Foursquare says in that video they're planning to migrate off of EC2, in part due to I/O performance. I'll be interested to hear whether or not this instance type changes their minds.</span>
*   <span>**Changes to database selection criteria**</span><span>. Given the garbage collection overhead of SSDs, databases that use</span> [<span>log structured update strategies may be preferred</span>](http://blog.empathybox.com/post/24415262152/ssds-and-distributed-data-systems) <span>over databases that use in-place update strategies. Cassandra, for example, uses LSM Trees to stream data to disk, which also happens to reduce</span> [<span>write amplification</span>](http://www.youtube.com/watch?v=zQdDi9pdf3I)<span>. Now you just need to have enough CPU to drive all that IO. Though</span> [<span>Vladimir Rodionov asserts</span>](http://techblog.netflix.com/2012/07/benchmarking-high-performance-io-with.html)<span>: Cassandra is not able to utilize the random read IO capacity of 100K of 4K blocks per second using 8 CPU cores of new Amazon High I/O instance.  </span>
*   **Consolidation**. A number of people have mentioned using the high-capacity servers to reduce their server count. Netflix calculated they could shrunk their server count by replacing 48 m2.4xlarge and 36 m2.xlarge with 15 hi1.4xlarge nodes.
*   <span>**Shift to server side processing**</span><span>. The high IO instances are so fast it’s a waste to get data, bring it to a different machine, perform a computation, and then write it back again. Use all those CPUs by using coprocessors/plugins to shift computations to the server instead of out to an intermediate cluster.</span>
*   <span>**Design to be IO bound on reads**</span><span>. Not a new idea, but SSDs make architecting towards reads even more attractive as there is no penalty for random reads. One idea is to precalculate on writes so getting results are simply read operations.</span>

## <span>Fewer Machines Means a Lower Total Overall Cost</span>

*   <span>**Half the cost**</span><span>. Netflix found that the hi1.4xlarge configuration is about half the system cost for the same throughput, even though the individual instance costs are higher.</span>
*   <span>**Reserved price tuning**</span><span>. Using reserved instances drops prices considerably. A</span> [<span>one year lease</span>](http://news.ycombinator.com/item?id=4264831) <span>reservation for a "heavy utilization" instance comes out to $7,280 per year + $0.621 per hour for a total of $12,719\. Reserving for three years</span> [<span>brings down the cost</span>](http://news.ycombinator.com/item?id=4264925) <span>to $656 a month, though</span> [<span>some argue</span>](http://news.ycombinator.com/item?id=4264968) <span>locking in for this amount of time</span> [<span>will rob you</span>](http://news.ycombinator.com/item?id=4266311) <span>of subsequent price discounts as new hardware comes along.</span>

## <span>High Availability is Still Necessary</span>

*   <span>**SSDs are persistent but they are not a magic reliability sauce**</span><span>. A resilience architecture is still needed to handle failure and recovery. Replication and write behind to EBS/S3 still need to be in your toolbox. SSDs can be corrupted just like disk. If a hard failure occurs, what state is the SSD in when the node comes back up? Keep in mind your connection to S3 is 1Gb/s so restoring from S3 will take a considerable amount of time, which makes replication a necessity for interactive workloads.</span>
*   **[<span>Adrian Cockcroft</span>](https://twitter.com/adrianco/status/225955849628176384) <span>(of Netflix) brings up an advantage of SSDs for HA</span>**<span>: faster net and disk greatly reduces repair time and impact so we can load up the instances with far more data.</span>
*   <span>**Traditionally SSD performance degrades over time**</span><span>. Wonder if it makes sense to fail over to different machines periodically to “reformat” SSD drives?</span>

## <span>It’s Not All Peaches and Cream</span>

*   <span>**What’s a programmer to do?**</span> <span>There’s a general theme that our experience with programming SSDs is neither deep or wide, so there isn’t a lot of best practices information available. Do you want to use append only algorithms? Is writing in place prefered? Is sequential IO fast? How long can you run an active system on SSDs? Are B-trees still a go to data storage structure? There’s still lots of debate about these kind of questions.</span>
    *   <span>For example, the folks at Acnu have found</span> [<span>working with SSDs is a very different experience</span>](http://www.acunu.com/2/post/2011/08/why-theory-fails-for-ssds.html)<span>: Our simple tests show that the Disk Access Model (DAM) and its derivatives are not very good predictors of the performance of write sequences on SSDs. We found a scheme to efficiently use SSDs for the Acunu storage core only once we freed ourselves of the ideas that the DAM planted in our heads.</span>
*   <span>**SSDs from different vendors differ quite a bit**</span><span>. This means parameters and algorithms will need experimentation to decide what works best. And what works best on Amazon may not work best on another machine. This adds another interesting twist to cloud portability and vendor lock-in.</span>
*   <span>**Faster IO pushes bottlenecks elsewhere in the system**</span><span>. Low level problems in the TCP stack or IRQ handling will be uncovered. Higher up the stack is also a problem. For example,</span> [<span>The Voldemort team found</span>](http://engineering.linkedin.com/voldemort/voldemort-solid-state-drives) <span>Java and SSDs were a troubling mix: On SSDs, we believe that the garbage collection overhead will have a significant performance impact for Java based systems, since IO is no longer the bottleneck.</span>
*   **Need more lower-end instance types over more regions**. These things come in time.
*   <span>It would be great to have direct access to flash without having to pretend all the world is a disk.</span>

## <span>Related Articles</span>

*   [<span>New High I/O EC2 Instance Type - hi1.4xlarge - 2 TB of SSD-Backed Storage</span>](http://aws.typepad.com/aws/2012/07/new-high-io-ec2-instance-type-hi14xlarge.html) <span>(</span>[<span>On Hacker News</span>](http://news.ycombinator.com/item?id=4264754)<span>)</span>
*   <span>Netflix:</span> [<span>Benchmarking High Performance I/O with SSD for Cassandra on AWS</span>](http://techblog.netflix.com/2012/07/benchmarking-high-performance-io-with.html) <span>(</span>[<span>On Hacker News</span>](http://hackerne.ws/item?id=4264877)<span>)</span>
*   [<span>Expanding The Cloud – High Performance I/O Instances for Amazon EC2</span>](http://www.allthingsdistributed.com/2012/07/high-performace-io-instance-amazon-ec2.html) <span>by Werner Vogels</span>
*   [<span>Log-structured file systems: There's one in every SSD</span>](http://lwn.net/Articles/353411/)
*   [<span>I/O Performance (no longer) Sucks in the Cloud</span>](http://perspectives.mvdirona.com/CommentView,guid,4478b347-0d30-439a-85d8-fca0db0d95d4.aspx) <span>by James Hamilton</span>
*   [<span>Testing on Amazon’s New SSD Instances</span>](http://www.gluster.org/2012/07/testing-on-amazons-new-ssd-instances/) <span>by Jeff Darcy</span>
*   [<span>Solid-state revolution: in-depth on how SSDs really work</span>](http://arstechnica.com/information-technology/2012/06/inside-the-ssd-revolution-how-solid-state-disks-really-work/) <span>by Lee Hutchinson</span>
*   [<span>Quora: Do append-only DB log structures benefit from Flash memory and SSDs?</span>](http://www.quora.com/Do-append-only-DB-log-structures-benefit-from-Flash-memory-and-SSDs)
*   [<span>SSDs and Distributed Data Systems</span>](http://blog.empathybox.com/post/24415262152/ssds-and-distributed-data-systems) <span>by Jay Kreps</span>
*   [<span>Stratified B-trees and versioning dictionaries</span>](http://arxiv.org/abs/1103.4282v2)
*   [<span>Why theory fails for SSDs</span>](http://www.acunu.com/2/post/2011/08/why-theory-fails-for-ssds.html) <span>by Irit Katriel</span>
*   [<span>Log file systems and SSDs – made for each other?</span>](http://www.acunu.com/2/post/2011/04/log-file-systems-and-ssds-made-for-each-other.html) <span>by Andy Twigg</span>
*   [<span>Flashing Databases: Expectations and LImitations</span>](about:blank)
*   <span>Quora:</span> [<span>Are algorithms optimized for regular hard drives *not* optimized for solid state drives?</span>](http://www.quora.com/Algorithms/Are-algorithms-optimized-for-regular-hard-drives-*not*-optimized-for-solid-state-drives)
*   [<span>Velobit Blog on SSD, Application Performance and Storage</span>](http://www.velobit.com/storage-performance-blog/?Tag=SSD%20optimization)
*   [<span>Fusion-io shoves OS aside, lets apps drill straight into flash</span>](http://www.theregister.co.uk/2012/04/19/fusion_io_sdk/)
*   [<span>What every developer should know about database scalability</span>](http://blip.tv/pycon-us-videos-2009-2010-2011/pycon-2010-what-every-developer-should-know-about-database-scalability-21-3280648) <span>by Jonathan Ellis</span>
*   [<span>Voldemort on Solid State Drives</span>](http://engineering.linkedin.com/voldemort/voldemort-solid-state-drives) <span>by Vinoth Chandar</span>

</div>