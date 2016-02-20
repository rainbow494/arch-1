## [Busting 4 Modern Hardware Myths - Are Memory, HDDs, and SSDs Really Random Access?](/blog/2013/6/13/busting-4-modern-hardware-myths-are-memory-hdds-and-ssds-rea.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, June 13, 2013 at 8:29AM</span></div>

<div class="body">

_"It’s all a numbers game – the dirty little secret of scalable systems"_

<div><iframe width="225" height="127" align="right" src="http://www.youtube.com/embed/MC1EKLQ2Wmg" frameborder="0" allowfullscreen=""></iframe></div>

[Martin Thompson](https://twitter.com/mjpt777) is a High Performance Computing Specialist with a real mission to teach programmers how to understand the innards of modern computing systems. He has many talks and classes (listed below) on caches, buffers, memory controllers, processor architectures, cache lines, etc.

His thought is programmers do not put a proper value on understanding how the underpinnings of our systems work. We gravitate to the shiny and trendy. His approach is not to teach people specific programming strategies, but to teach programmers to fish so they can feed themselves. Without a real understanding strategies are easy to apply wrongly.  It's strange how programmers will put a lot of effort into understanding complicated frameworks like Hibernate, but little effort into understanding the underlying hardware on which their programs run.

A major tenant of Martin's approach is to "lead by experimental observation rather than what folks just blindly say," so it's no surprise he chose a MythBuster's theme in his talk [Mythbusting Modern Hardware to Gain "Mechanical Sympathy."](http://www.youtube.com/watch?v=MC1EKLQ2Wmg) Mechanical Sympathy is term coined by [Jackie Stewart](http://en.wikipedia.org/wiki/Jackie_Stewart), the race car driver, to say you get the best out of a racing car when you have a good understanding of how a car works. A driver must work in harmony with the machine to get the most of out of it. Martin extends this notion to say we need to know how the hardware works to get the most out of our computers. And he thinks normal developers can understand the hardware they are using. If you can understand Hibernate, you can understand just about anything.

The structure of the talk is to take a few commonly held myths and go all MythBusters on them by seeing if they are really true. Along the way there's incredible detail on how different systems work, far too much detail to gloss here, but it's an absolute fascinating talk. Martin really knows what he is talking about and he is a good teacher as well.

The most surprising part of the talk is the counter intuitive idea that many of the devices we think of as random access, like RAM, HDDs, and SSDs, effectively become serial devices in certain circumstances. A disk, for example, is really just a big tape that's fast. It's not true random access. 

## Myth 1: CPUs are Not Getting Faster

*   The fundamental issue is CPUs can't get hotter, not that they can't get faster. As we run CPUs faster (higher clock speeds) they get hotter and hotter and heat dissipation at these small scales is incredibly difficult. 
*   Clock speed isn't everything. Example, word split the text of Alice and Wonderland. Intel Core 2 Duo, 2008, 2.40GHz, 1434 operations per second. Intel Core, 2011, 2.20GHz, 2674 operations per second. Clock speed is down but operations per second have nearly doubled. Trend is continuing.
*   Sandy/Ivy Bridge goes parallel inside instead of going faster. There are 3 ALUs and 6 ports for loading and storing data. More ports are needed to feed the ALUs so you can have up to 6 instructions per cycle happening in parallel. 
    *   There's only one divide and one jump. Highly branched or code with a lot of division doesn't go as fast as straightforward code using +, -, *.
*   CPUs have counters so they are easy to profile. On Linux access the counters using _perf stat_.
    *   Running perf stat on Nehalem 2.8GHz in the Alice and Wonderland test we notice that the processor is idle about a 1/3rd of the time so a faster processor wouldn't help. 
    *   On a later Sandy Bridge 2.4GHz the CPU is idle about 25% of the time. The reason CPUs are getting faster is not faster clock speed, but instructions are being fed into the CPU faster. 

## Myth 2: Memory Provides Random Access

*   At the end of the day it's a cost equation. To get to main memory is fairly expensive. We want to feed the CPUs really fast. How do we feed the CPU fast? We need the data to be close to them on something that's very very quick. Modern register to register copies don't cost anything because what is happening is a remapping, it doesn't even move.
*   Layers of caches. Caches get bigger and slower so it's speed versus cost, but also power is important. Power to access a disk is so much more than accessing a L1 cache. Modern processors are starting to transfer data from network cards into cache, skipping memory, to keep the thermals down by not involving the CPU. 
*   There's lot of detail on memory ordering and cache structures and coherence. The gist seems to be there's an immensely complicated circuitry between the different layers of memory and the CPU. If you can't make the memory sub-systems, caches, buses fast enough then you can't feed the CPU fast enough so there's no point in making CPUs faster.
*   This means software must be written to access memory in a friendly manner or you are starving the CPU. On Sandy Bridge sequentially walking through memory will take you 3 clocks for L1D, 11 clocks for L2, 14 clocks for L3, and 6ns for memory. In-page random is 3 clocks, 11 clocks, 18 clocks,  22ns. Full random access is 3 clocks, 11 clocks, 38 clocks, 65.8 ns. 
*   It's effectively free to walk through memory sequentially. How we access memory really really matters. 
*   Writing highly branched code causes more instruction misses because there's too much data to keep track of.
*   Since you can't walk memory sequentially you want to reduce coupling and increase cohesion. Keep things together. Good coupling and good cohesion makes this all just work. If your code branches everywhere and runs all over the heap it will make for slow code.

## Myth 3: HDDs Provide Random Access 

*   [Zone bit recording](http://en.wikipedia.org/wiki/Zone_bit_recording). There's a big difference between writing on the inner and outer parts of the disk. More sectors are put on the outer parts of the disk so you get greater density. For one revolution of the disk you are going to see more sectors so you'll get greater throughput.
*   On a 10K disk when sequentially reading the outer tracks you'll get 220 MB/s and when reading the innter tracks you'll get 140 MB/s.
*   The fastest disks are 15K and they haven't got any faster for many many years.
*   Hardware will prefetch and reorder queues as the head moves over the sectors. A sector is now 4K to get more data on a disk. If you read or write a byte the minimum transferred is 4K. 
*   What makes up an operation? 
    *   Command processing. Subsecond.
    *   Seek time. 0-6ms server drive, 0-15ms laptop drive.
    *   Rotational latency. For a 10K RPM disk rotation takes 6ms for an average of 3ms.
    *   Data transfer.  100-200MB/s.
*   For random access of a 4K block, the random latency is 10ms or 100 IOPS. Throughput at random is less than 1 MB a second, maybe 2 MB a second with really clever hardware. So randomly accessing a disk isn't practical. If you see fantastic transaction numbers then the data isn't going to disk. 
*   A disk is really a big tape that's fast. It's not true random access.

## Myth 4: SSDs Provide Random Access

*   SSDs gernerally have 2MB blocks arranged in an array of cells. SLC - single level, can store a bit. Has voltage or doesn't have a voltage. MLC - multiple voltages, so you can store 2 or 3 bits per cell.
*   Expensive to address individual cells so you can address a row at a time, which is called a page, pages are usually 4K or 8K. Reading or writing a random page sized thing is really fast, there's no moving parts.
*   When you delete you can only erase a whole block at a time. The ways SSDs work is they write every cell to be a one. When you put data into it you turn off the bits you don't want. Turning off a bit is easy because it's draining a cell. Turning on a bit by putting voltage into the cell tends to light up the cells around it so you can't accurately set a single bit. So you must delete a whole block at a time. Bits are marked as deleted because you don't want to erase a whole block at a time because there's a limited number times you can read and write a block. You don't want a disk to wear out. So bits are marked as deleted and the new data is copied to a new block. This has a cost. In time the disk ends up fragmented. Over time you have to garbage collect, compacting blocks.
*   Example SSD can read and write at 200 MB/s. When you starting deleting read performance looks good, but writes slow down because of the garbage collection process. For some disks performance falls off a cliff on writes and you need to reformat. There's also write amplification where small writes end up triggering a lot of copying. 
*   Reads have great random and sequential performance. If you only do append only writes then performance could be quite good. 
*   At 40K IOPs with 4K random reads and writes, average operation times are 100-300 microseconds with large up to half a second pauses during garbage collection.
*   Mutating in place causes poor performance.

## Related Articles

<div>

*   [On Hacker News](https://news.ycombinator.com/item?id=7955093)
*   [Slides for Video](http://gotocon.com/dl/goto-aar-2012/slides/MartinThompson_MythbustingModernHardwareToGainMechanicalSympathy.pdf)
*   [Mechanical Sympathy Blog](http://mechanical-sympathy.blogspot.com/)
*   [LMAX Disruptor and the Concepts of Mechanical Sympathy](http://www.youtube.com/watch?v=Qho1QNbXBso)
*   [Tackling the Folklore Surrounding High Performance Computing](http://yow.eventer.com/events/1004/talks/1026)
*   [Martin Thompson on Low Latency Coding and Mechanical Sympathy](http://www.infoq.com/interviews/martin-thompson-Low-Latency)
*   [Mechanical Sympathy Google Group](https://groups.google.com/forum/#!forum/mechanical-sympathy)
*   Review of [Martin Thompson’s Writing Concurrent Code with Lock Free Algorithms Course](http://www.heartysoft.com/martin-thompson-lock-free-algos) ([more on course](http://skillsmatter.com/course/home/martin-thompsons-writing-concurrent-code-with-lock-free-algorithms))
*   [Switch Your Databases To Flash Storage. Now. Or You're Doing It Wrong.](http://highscalability.com/blog/2012/12/10/switch-your-databases-to-flash-storage-now-or-youre-doing-it.html)

</div>

</div>