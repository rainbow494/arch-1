## [Switch your databases to Flash storage. Now. Or you're doing it wrong.](/blog/2012/12/10/switch-your-databases-to-flash-storage-now-or-youre-doing-it.html)

    

    

![](http://farm9.staticflickr.com/8319/7952195310_8078e8c9df_m.jpg)    This is a guest post by     [Brian Bulkowski](http://www.aerospike.com/brian-bulkowski/)    , CTO and co-founder of     [Aerospike](http://www.aerospike.com/)    , a leading clustered NoSQL database, has worked in the area of high performance commodity systems since 1989.    

##     Why flash rules for databases    

    The economics of flash memory are staggering.     **    If you’re not using SSD, you are doing it wrong.     **

    Not quite true, but close. Some small applications fit entirely in memory – less than 100GB – great for in-memory solutions. There’s a place for rotational drives (HDD) in massive streaming analytics and petabytes of data. But for the vast space between, flash has become the only sensible option.     

    For example, the Samsung 840 costs $180 for 250GB. The speed rating for this drive is rated by the manufacturer at 96,000 random 4K read IOPS, and 61,000 random 4K write IOPS. The Samsung 840 is not alone at this price performance. A 300GB Intel 320 is $450\. An OCZ Vertex 4 256GB is $235, with the Intel being rated as slowest, but our internal testing showing solid performance. Most datacenter chassis will accommodate four data drives, and adding four Samsung 840 creates a system with         **1TB of storage, 384,000 read IOPS, 248,000 random write IOPS, for a storage street cost of $720**         and adding an extra 0.3 watts to a server’s power draw.    

    If you have a dataset under 10TB, and you’re still using rotational drives,         **you’re doing it wrong**.         The new low cost of flash makes rotational drives useful only for the lightest of workloads.    

    Most operational non-analytic work loads require only a few IOPS per transaction. A good database should require just one.     

    HDD as a price of about $0.10 per GB – 10x cheaper than flash – but each spindle supports about 200 IOPS--- the number of seeks per second. Until the recent advent of flash, databases were IOPS limited, requiring large arrays to reach high performance. Estimating cost per IOP is difficult, as smaller drives provide the same performance for lower cost. But achieving performance similar to the 96,000 IOPS of a $180 Samsung 840 would require over 400 HDD at a price of hundreds of thousands of dollars.     

    Let’s compare the economics of memory. Dell is currently (December 2012) charging $20 per GB for DRAM (16GB DIMM at $315), and a fully loaded R720 with RDIMMs topping out at 384GB for $13,000—or $33 per GB, fully loaded. Memory doesn’t have IOPS, and main memory databases measured over 1M transactions per second. Memory is faster, but we’ll see that for most use cases, network bottlenecks will overcome RAM’s performance advantage.     

    Step back: $33 per GB for RAM, $1 per GB for flash. High density         **12T**         solutions can be built with the current Dell R720, compared to a high density 384GB memory system at about the same price ($13K/server). RAM’s power draw tips the equation even further.    

    Flash storage provides random access capabilities, which means your application developers are spending less time optimizing query patterns. All the queries go fast.  That fast random access results in architectural flexibility, and allows you to change your data patterns and applications rapidly. That’s priceless.    

##     The lure of main memory databases     

    Main memory sounds ideal - it’s blindingly fast for random data access patterns. Reads and writes are predictable, and memcache is one of the most loved balms to fix performance issues discovered in deployment.     

    It’s easy to write a new main memory database, and to simply cache data in your application. As a programmer, you never have to write an I/O routine and never have to deal with thread context switches. Using a standard allocator and standard threading techniques—without even optimizing for memory locality (NUMA optimizations)—a database built on main memory principles will be faster than 1G and 10G networking.    

    [In Russ’s blog post about 1M TPS on a single $5K server](http://highscalability.com/blog/2012/9/10/russ-10-ingredient-recipe-for-making-1-million-tps-on-5k-har.html)        , he showed blistering performance on an in-memory dataset. Clustered, distributed databases utilize k-safety and allow persistence to disk, so losing your data is not an issue. A Dell R520 with 96GB of memory can be had for $6,000, and if your business problem fits in a few hundred gigabytes, main memory is a great choice.    

    The problems come when you scale out. You start buying         a lot         of RAM, and you find interesting applications that are not cost-effective – where 100GB of data was a good start, but a few terabytes of storage would create a very compelling application.     

    I recently visited two modest social networking companies and found each had 4TB of memcache servers – a substantial main memory investment. As they broadened their reach and tried to build applications that spanned more variety for each user request, they just kept beefing up their cache tier. The CTOs at both companies didn’t complain about the cost. Instead, they were afraid to roll out the best user experience ideas they had—new features such as expanded friend-of-friend display—because that would expand cache requirements, thrash the caching layer, and bring down their service. Or require another several racks of servers.    

##     With the right database, your bottleneck is the network driver, not flash    

    Networks are measured in bandwidth (throughput), but if your access patterns are random and low latency is required, each request is an individual network packet. Even with the improvements in Linux network processing, we find an individual core is capable of resolving about 100,000 packets per second through the Linux core.     

    100,000 packets per second aligns well with the capability of flash storage at about 20,000 to 50,000 per device, and adding 4 to 10 devices fits well in current chassis.         **RAM is faster**         – in Aerospike, we can easily go past 5,000,000 TPS in main memory if we remove the network bottleneck through batching – but for most applications, batching can’t be cleanly applied.    

    This bottleneck still exists with high-bandwidth networks, since the bottleneck is the processing of network interrupts. As multi-queue network cards become more prevalent (not available today on many cloud servers, such as the Amazon High I/O Instances), this bottleneck will ease – and don’t think switching to UDP will help. Our experiments show TCP is 40% more efficient than UDP for small transaction use cases.     

    Rotational disk drives create a bottleneck much earlier than the network. A rotational drive tops out at about 250 random transactions per second. Even with a massive RAID 10 configuration, 24 direct attach disks would create a bottleneck at about 6,000 transactions per second. Rotational disks never make sense when you are querying and seeking. However, they are appropriate for batch analytics systems, such as Hadoop, which stream data without selecting.     

    With flash storage, even if you need to do 10 to 20 I/Os per database transaction, your bottleneck is the network. If you’re in memory, the bottleneck is still the network.     

    If you choose the main memory path, you’ve thrown away a lot of money on RAM; you’re burning money on powering that RAM every minute of every day, and – very probably - your servers aren’t going any faster.    

##     The top myths of flash    

**    1\.         Flash is too expensive.    **         

    Flash is 10x more expensive than rotational disk. However, you’ll make up the few thousand dollars you’re spending simply by saving the cost of the meetings to discuss the schema optimizations you’ll need to try to keep your database together. Flash goes so fast that you’ll spend less time agonizing about optimizations.     

    **2\. I don’t know which flash drives are good.**    

    Aerospike can help.         [We have developed and open-source a tool](http://github.com/aerospike/act)         (Aerospike Certification Tool) that benchmarks drives for real-time use cases, and we’re providing our measurements for old drives. You can run these benchmarks yourself, and see which drives are best for real-time use or see our [latest test results](http://www.aerospike.com/blog/act-ssd-benchmark/).    

    **3\. They wear out and lose my data.**    

    Wear patterns and flash are an issue, although rotational drives fail too. There are several answers. When a flash drive fails, you can still read the data. A clustered database and multiple copies of the data, you gain reliability – a server level of RAID. As drives fail, you replace them. Importantly, new flash technology is available every year with higher durability, such as this year’s Intel S3700 which claims each drive can be rewritten 10 times a day for 5 years before failure. Next year may bring another generation of reliability. With a clustered solution, simply upgrade drives on machines while the cluster is online.      

         **4\. I need the speed of in-memory**

<a name="id.gjdgxs"></a>    Many NoSQL databases will tell you that the only path to speed is in-memory. While in-memory is faster, a database optimized for flash using the techniques below can provide millions of transactions per second with latencies under a millisecond.    

##     Techniques for flash optimization    

    Many projects work with main memory because the developers don’t know how to unleash flash’s performance. Relational databases only speed up 2x or 3x when put on a storage layer that supports 20x more I/Os. Following are three programming techniques to significantly improve performance with flash.    

    **1\. Go parallel with multiple threads and/or AIO**    

    Different SSD drives have different controller architectures, but in every case there are multiple controllers and multiple memory banks—or the logical equivalent. Unlike a rotational drive, the core underlying technology is parallel.    

    You can benchmark the amount of parallelism where particular flash devices perform optimally with ACT, but we find the sweet spot is north of 8 parallel requests, and south of 64 parallel requests per device. Make sure your code can cheaply queue hundreds, if not thousands, of outstanding requests to the storage tier. If you are using your language’s asynchronous event mechanism (such as a co-routine dispatch system), make sure it is efficiently mapping to an OS primitive like asynchronous I/O, not spawning threads and waiting.    

    **2\. Don’t use someone else’s file system**     

    File systems are very generic beasts. As databases, with their own key-value syntax and interfaces, they have been optimized for a particular use, such as multiple names for one object and hierarchical names. The POSIX file system interface supplies only one consistency guarantee. To run at the speed of flash, you have to remove the bottleneck of existing file systems.     

    Many programmers try to circumvent the file system by using direct device access and the O_DIRECT flag. Linus Torvalds famously removed the DIRECT option from the Linux kernel saying it was braindamaged. Here’s how he said it in 2007:     

<a name="1769039bdc42f2ad3bdf222c02fa2f27e3049ee2"></a><a name="0"></a>

<table class="c14" cellspacing="0" cellpadding="0">

<tbody>

<tr class="c4">

<td class="c9">

    Date    

</td>

<td class="c15">

    Wed, 10 Jan 2007 19:05:30 -0800 (PST)    

</td>

</tr>

<tr class="c4">

<td class="c9">

    From    

</td>

<td class="c15">

    Linus Torvalds <>    

</td>

</tr>

<tr class="c4">

<td class="c9">

    Subject    

</td>

<td class="c15">

    Re: O_DIRECT question    

</td>

</tr>

</tbody>

</table>

    The right way to do it is to just not use O_DIRECT.  

The whole notion of "direct IO" is totally braindamaged. Just say no.  

        This is your brain: O  
        This is your brain on O_DIRECT: .  

        Any questions?    

         

    Our measurements show that the page cache dramatically increases latency. At the speeds of flash storage, the page cache is disastrous. Linus agreed substantially in his own post:    

         

    Side note: the only reason O_DIRECT exists is because database people are  
too used to it, because other OS's haven't had enough taste to tell them  
to do it right, so they've historically hacked their OS to get out of the  
way.  

As a result, our madvise and/or posix_fadvise interfaces may not be all  
that strong, because people sadly don't use them that much. It's a sad  
example of a totally broken interface (O_DIRECT) resulting in better  
interfaces not getting used, and then not getting as much development  
effort put into them.  

So O_DIRECT not only is a total disaster from a design standpoint (just  
look at all the crap it results in), it also indirectly has hurt better  
interfaces. For example, POSIX_FADV_NOREUSE (which _could_ be a useful and  
clean interface to make sure we don't pollute memory unnecessarily with  
cached pages after they are all done) ends up being a no-op ;/  

Sad. And it's one of those self-fulfilling prophecies. Still, I hope some  
day we can just rip the damn disaster out.    

    In a test scenario we tried, enabling the page cache causes some requests to complete in 16 to 32 milliseconds, and a substantial portion (3% to 5%) take more than 1ms. With O_DIRECT, no request took more than 2 ms and > 99.9 requests were under 1 ms. We did not test madvise; we also found it is not hooked up correctly in some versions of Linux.    

    Aerospike uses O_DIRECT, because it works.    

    **3\. Use large block writes and small block reads**    

    Flash storage is different from any other storage because it is asymmetric. Reads are different from writes, unlike RAM and unlike rotational disk. Flash chips are like an etch-a-sketch: you can draw individual lines, but to erase the entire screen requires shaking. Flash chips work in native block of sizes around 1MB, and writing at the same size as the fundamental block of the flash chip means the device keeps the simplest possible map.    

    Reads can be done anywhere on the device, unlike writes. You can exploit this characteristic by writing data together and reading randomly.    

    Over time, flash device firmware will improve, and small block writes will become more efficient and correct, but we are still early in the evolution of flash storage. Today, writing only in large blocks leads to lower write amplification and lower read latency.    

##     Flash is better than you think; use it and prosper    

    Knowing how to use flash and using flash-optimized databases can provide your designs with massive competitive benefit. The problems of social metadata, graph analysis, user profile storage, massive online multiplayer games with shared social gameplay, security threat pattern analysis, real-time advertising and audience analysis, benefit from immediately available, highly random database storage.    

##     Related Articles    

*   [On Hacker News](http://news.ycombinator.com/item?id=4900426)
*   [Anatomy of a Solid-state Drive](http://queue.acm.org/detail.cfm?id=2385276)
*   [Vertical Scaling Ascendant - How Are SSDs Changing  Architectures?](http://highscalability.com/blog/2012/7/25/vertical-scaling-ascendant-how-are-ssds-changing-architectur.html)
*   [It's The Fraking IOPS - 1 SSD Is 44,000 IOPS, Hard Drive Is 180](http://highscalability.com/blog/2011/6/22/its-the-fraking-iops-1-ssd-is-44000-iops-hard-drive-is-180.html)
*   [37signals Still Happily Scaling On Moore RAM And SSDs](http://highscalability.com/blog/2012/1/30/37signals-still-happily-scaling-on-moore-ram-and-ssds.html)
*   [The Five-Minute Rule 20 Years Later](http://cacm.acm.org/magazines/2009/7/32091-the-five-minute-rule-20-years-later/fulltext)
*   [Exploring the Relationship Between Spare Area and Performance Consistency in Modern SSDs](http://www.anandtech.com/show/6489/playing-with-op)

    