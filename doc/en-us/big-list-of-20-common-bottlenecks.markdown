## [Big List of 20 Common Bottlenecks](/blog/2012/5/16/big-list-of-20-common-bottlenecks.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, May 16, 2012 at 9:15AM</span></div>

<div class="body">

![](http://farm6.staticflickr.com/5323/7207459230_8cbe334c4a_m.jpg)

In [<span>Zen And The Art Of Scaling - A Koan And Epigram Approach</span>](http://highscalability.com/blog/2012/2/27/zen-and-the-art-of-scaling-a-koan-and-epigram-approach.html), [<span>Russell Sullivan</span>](https://twitter.com/#!/jaksprats) offered an interesting conjecture: there are 20 classic bottlenecks. This sounds suspiciously like the idea that there only [<span>20 basic story plots</span>](http://www.tennscreen.com/plots.htm). And depending on how you chunkify things, it may be true, but in practice we all know bottlenecks come in infinite flavors, all tasting of sour and ash.  

One day [Aurelien Broszniowski](http://jsoftbiz.wordpress.com/) from Terracotta emailed me his list of bottlenecks, we cc’ed Russell in on the conversation, he gave me his list, I have a list, and here’s the resulting stone soup.  

Russell said this is his “I wish I knew when I was younger" list and I think that’s an enriching way to look at it. The more experience you have, the more different types of projects you tackle, the more lessons you’ll be able add to a list like this. So when you read this list, and when you make your own, you are stepping through years of accumulated experience and more than a little frustration, but in each there is a story worth grokking.

*   <span>Database:</span>
    *   <span>Working size exceeds available RAM</span>
    *   <span>Long & short running queries</span>
    *   <span>Write-write conflicts</span>
    *   <span>Large joins taking up memory</span>
*   <span>Virtualisation:</span>
    *   <span>Sharing a HDD, disk seek death</span>
    *   <span>Network I/O fluctuations in the cloud</span>
*   <span>Programming:</span>
    *   <span>Threads: deadlocks, heavyweight as compared to events, debugging, non-linear scalability, etc...</span>
    *   <span>Event driven programming: callback complexity, how-to-store-state-in-function-calls, etc...</span>
    *   <span>Lack of profiling, lack of tracing, lack of logging</span>
    *   <span>One piece can't scale, SPOF, non horizontally scalable, etc...</span>
    *   <span>Stateful apps</span>
    *   <span>Bad design : The developers create an app which runs fine on their computer. The app goes into production, and runs fine, with a couple of users. Months/Years later, the application can't run with thousands of users and needs to be totally re-architectured and rewritten.</span>
    *   <span>Algorithm complexity</span>
    *   <span>Dependent services like DNS lookups and whatever else you may block on.</span>
    *   <span>Stack space</span>
*   <span>Disk:</span>
    *   <span>Local disk access</span>
    *   <span>Random disk I/O -> disk seeks</span>
    *   <span>Disk fragmentation</span>
    *   <span>SSDs performance drop once  data written is greater than SSD size</span>
*   <span>OS:</span>
    *   <span>Fsync flushing, linux buffer cache filling up</span>
    *   <span>TCP buffers too small</span>
    *   <span>File descriptor limits</span>
    *   <span>Power budget</span>
*   <span>Caching:</span>
    *   <span>Not using memcached (database pummeling)</span>
    *   <span>In HTTP: headers, etags, not gzipping, etc..</span>
    *   <span>Not utilising the browser's cache enough</span>
    *   <span>Byte code caches (e.g. PHP)</span>
    *   <span>L1/L2 caches. This is a huge bottleneck. Keep important hot/data in L1/L2\. This spans so much: snappy for network I/O, column DBs run algorithms directly on compressed data, etc. Then there are techniques to not destroy your TLB. The most important idea is to have a firm grasp on computer architecture in terms of CPUs multi-core, L1/L2, shared L3, NUMA RAM, data transfer bandwidth/latency from DRAM to chip, DRAM caches DiskPages, DirtyPages, TCP packets travel thru CPU<->DRAM<->NIC.</span>
*   <span>CPU:</span>
    *   <span>CPU overload</span>
    *   <span>Context switches -> too many threads on a core, bad luck w/ the linux scheduler, too many system calls, etc...</span>
    *   <span>IO waits -> all CPUs wait at the same speed</span>
    *   <span>CPU Caches: Caching data is a fine grained process (In Java think volatile for instance), in order to find the right balance between having multiple instances with different values for data and heavy synchronization to keep the cached data consistent.</span>
    *   <span>Backplane throughput</span>
*   <span>Network:</span>
    *   <span>NIC maxed out, IRQ saturation, soft interrupts taking up 100% CPU</span>
    *   <span>DNS lookups</span>
    *   <span>Dropped packets</span>
    *   <span>Unexpected routes with in the network</span>
    *   <span>Network disk access</span>
    *   <span>Shared SANs</span>
    *   <span>Server failure -> no answer anymore from the server</span>
*   <span>Process:</span>
    *   <span>Testing time</span>
    *   <span>Development time</span>
    *   <span>Team size</span>
    *   <span>Budget</span>
    *   <span>Code debt</span>
*   <span>Memory:</span>
    *   <span>Out of memory -> kills process, go into swap & grind to a halt</span>
    *   <span>Out of memory causing Disk Thrashing (related to swap)</span>
    *   <span>Memory library overhead</span>
    *   <span>Memory fragmentation</span>
        *   <span>In Java requires GC pauses</span>
        *   <span>In C, malloc's start taking forever</span>

<span>If you have any more to add or you have suggested fixes, please jump in.</span>  

<span>Thanks to Aurelien and Russell for all their applied brain power.</span>

## <span>Related Articles</span>

*   [<span>The USE Method: Linux Performance Checklist</span>](http://dtrace.org/blogs/brendan/2012/03/07/the-use-method-linux-performance-checklist/) <span>by Brendan Gregg</span>
*   [<span>It’s Faster Because It’s C</span>](http://pl.atyp.us/wordpress/index.php/2010/07/its-faster-because-its-c/) <span>by Jeff Darcy</span>
*   [<span>Top 10 Performance Problems taken from Zappos, Monster, Thomson and Co</span>](http://blog.dynatrace.com/2010/06/15/top-10-performance-problems-taken-from-zappos-monster-and-co/) <span>by Andreas Grabner</span>
*   [<span>Java vs. C Performance….Again</span>](http://www.azulsystems.com/blog/cliff/2009-09-06-java-vs-c-performanceagain) <span>by Cliff Click</span>

</div>