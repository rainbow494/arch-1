## [Paper: Log-structured Memory for DRAM-based Storage - High Memory Utilization Plus High Performance](/blog/2014/3/20/paper-log-structured-memory-for-dram-based-storage-high-memo.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, March 20, 2014 at 9:55AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7383/13290626565_dc21d694a5_o.png)

Most every programmer who gets sucked into deep performance analysis for long running processes eventually realizes memory allocation is the heart of evil at the center of many of their problems. So you replace malloc with something less worse. Or you tune your garbage collector like a fine ukulele. But there's a smarter approach brought to you from the folks at [RAMCloud](https://ramcloud.stanford.edu/wiki/display/ramcloud/RAMCloud), a Stanford University production, which is a large scale, distributed, in-memory key-value database.

What they've found is that typical memory management approaches don't work and using a log structured approach yields massive benefits:

> Performance measurements of log-structured memory in RAMCloud show that it enables high client through- put at 80-90% memory utilization, even with artificially stressful workloads. In the most stressful workload, a single RAMCloud server can support 270,000-410,000 durable 100-byte writes per second at 90% memory utilization. The two-level approach to cleaning improves performance by up to 6x over a single-level approach at high memory utilization, and reduces disk bandwidth overhead by 7-87x for medium-sized objects (1 to 10 KB). Parallel cleaning effectively hides the cost of cleaning: an active cleaner adds only about 2% to the latency of typical client write requests.

And for your edification they've written an excellent paper on their findings. [Log-structured Memory for DRAM-based Storage](https://www.usenix.org/system/files/conference/fast14/fast14-paper_rumble.pdf):

[Click to read more ...](/blog/2014/3/20/paper-log-structured-memory-for-dram-based-storage-high-memo.html)

</div>