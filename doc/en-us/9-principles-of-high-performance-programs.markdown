## [9 Principles of High Performance Programs](/blog/2014/5/21/9-principles-of-high-performance-programs.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, May 21, 2014 at 8:54AM</span></div>

<div class="body">

![](https://farm6.staticflickr.com/5036/14028263590_848c224784_n.jpg)

[Arvid Norberg](http://www.linkedin.com/pub/arvid-norberg/2/977/ab8) on the [libtorrent blog](http://blog.libtorrent.org/) has put together an excellent list of [principles of high performance programs](http://blog.libtorrent.org/2012/12/principles-of-high-performance-programs/), obviously derived from hard won experience programming on bittorrent:

Two fundamental causes of performance problems:

1.  **Memory Latency**. A big performance problem on modern computers is the latency of SDRAM. The CPU waits idle for a read from memory to come back.
2.  **Context Switching**. When a CPU switches context "the memory it will access is most likely unrelated to the memory the previous context was accessing. This often results in significant eviction of the previous cache, and requires the switched-to context to load much of its data from RAM, which is slow."

Rules to help balance the forces of evil:

1.  **Batch work**. Avoid context switching by batching work. For example, there are vector versions of system calls like writev() and readv() that operate on more than one item per call. An implication is that you want to merge as many writes as possible.
2.  **Avoid Magic Numbers**. They don't scale. Waking a thread up every 100ms or when 100 jobs are queued, or using fixed size buffers, doesn't adapt to changing circumstances.
3.  **Allocate memory buffers up front**. Avoid extra copying and maintain predictable memory usage.
4.  **Organically adapt** your job-batching sizes to the granularity of the scheduler and the time it takes for your thread to wake up.
5.  **Adapt receive buffer sizes for sockets**, while at the same time avoiding to copy memory out of the kernel.
6.  **Always complete all work queued** up for a thread before going back to sleep.
7.  **Only signal a worker thread to wake up** when the number of jobs on its queue go from 0 to > 0\. Any other signal is redundant and a waste of time.

This is a very deep post with many more details than glossed here, so please read the [original article](http://blog.libtorrent.org/2012/12/principles-of-high-performance-programs/). There are also many other interesting articles on the site: [memory cache optimizations](http://blog.libtorrent.org/2013/12/memory-cache-optimizations/), [swarm connectivity](http://blog.libtorrent.org/2012/12/swarm-connectivity/), [asynchronous disk I/O](http://blog.libtorrent.org/2012/10/asynchronous-disk-io/).

</div>