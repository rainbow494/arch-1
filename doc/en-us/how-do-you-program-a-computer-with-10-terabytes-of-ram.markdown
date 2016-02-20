## [How do you program a computer with 10 terabytes of RAM?](/blog/2015/8/5/how-do-you-program-a-computer-with-10-terabytes-of-ram.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, August 5, 2015 at 8:56AM</span></div>

<div class="body">

![](https://farm1.staticflickr.com/350/19695165133_6fee8eb029_n.jpg)

<span>How do you program a computer with 10 terabytes of RAM in a single address space?  When the great</span> [<span>Adrian Cockcroft</span>](https://twitter.com/adrianco) <span>was interviewed for</span> [<span>Enterprise Initiatives Episode blog</span>](http://www.cloudtp.com/2015/07/09/designing-the-it-architecture-of-the-future/)<span>, that’s one of the answers he gave to the question of “What’s the next big thing?”</span>

<span>Adrian says we are already taking big machines and running tiny little containers on them. He thinks another interesting workload is huge memory systems. Building computers with many terabytes of main memory will soon be affordable. We already know the JVM has problems garbage collecting on machines with 10s of gigabytes of RAM. What about machines with terabytes of RAM? We don’t really have the programming models worked out yet. It may be that garbage collected languages won't make the cut.</span>

<span>Sounds like a good idea for a post, right? Here’s the problem, I found surprisingly little on huge memory systems. If you have any ideas on good source please leave a comment. Here’s some of what I did find…</span>

## <span>SGI’s 64TB Computer</span>

<span>You may chuckle at the idea of a computer with 10TB of RAM. Well, one already exists. It’s from SGI. The</span> [<span>SGI UV (Ultra Violet) 3000</span>](http://www.sgi.com/products/servers/uv/uv_3000_30.html) <span>scales from 4 to 256 CPU sockets with up to 64TB of shared memory as a single system. It’s aimed squarely at the enterprise space, SGI is out of the cloud.</span>

<span>There’s a</span> [<span>good thread</span>](https://news.ycombinator.com/item?id=7302770) <span>on Hacker News that talks about some of the problems that occur when using so much RAM:</span>

*   **Cost**. There’s no cost information given, but high-end RAM is not cheap. Thoughts are it might be in the $4M range, which is pure conjecture. StackExchange has an interesting thread, [<span>If RAM is cheap, why don't we load everything to RAM and run it from there?</span>](http://superuser.com/questions/637302/if-ram-is-cheap-why-dont-we-load-everything-to-ram-and-run-it-from-there), that goes more into the cost aspects. Hard disks are still a lot cheaper than RAM.

*   <span>**Memory latency**</span><span>. Though from the programmer’s perspective the memory will look like a flat address space, under the good memory accesses will go through a</span> [<span>network</span>](https://en.wikipedia.org/wiki/NUMAlink) <span>that’s not all that different from a</span> [<span>networked cluster</span>](http://man7.org/linux/man-pages/man7/numa.7.html) <span>of machines. Martin Thompson talks about this problem with the counterintuitive idea that</span> [<span>memory is not truly random access</span>](http://highscalability.com/blog/2013/6/13/busting-4-modern-hardware-myths-are-memory-hdds-and-ssds-rea.html)<span>. Even if your memory looks flat you still have to worry about the penalty for accessing non-local RAM. Still, it’s faster than going to disk.</span>

*   <span>**Corruption/Consistency**</span><span>. The UV runs a single system image, which means in practice it looks like a big desktop computer. You have huge potential problems with buggy code scribbling all over memory. You also have a problem with programs that crash. If you are doing all your computing in RAM you can’t rely on restarting a process and reading everything back from disk to fix all your problems. Crashes mean inconsistency unless you code specifically around it.</span>

*   <span>**Problem to solve**</span><span>. You don’t want to use such a machine to run one instance of database. And you need to solve a problem that isn’t just as easily solved with a cluster. The star use case seems to be as an appliance for</span> [<span>SAP HANA</span>](https://en.wikipedia.org/wiki/SAP_HANA)<span>, an in-memory, column-oriented, relational database management system, which presumably benefits from not running its own cache coherence protocols over the network and has some way to partition work in a CPU-aware and NUMA-aware manner.</span>

*   <span>**Power**</span><span>. Big-iron uses a lot of power.</span>

*   <span>**Address space exhaustion**</span><span>. From</span> [<span>userbinator</span>](https://news.ycombinator.com/item?id=7304219)<span>: Now we have 64TB, which is 2^46, which means there's only 18 "unused" bits of address left - 256K. If you could connect only(!) 262,144 of these machines together and present the memory on them as one big unit, you would have exhausted the 64-bit address space. That is what I think is really incredible. What's next, 128-bit addresses? Or maybe we'll realise that segmented address spaces (e.g. something like 96-bit, split as 32:64) are naturally more suited to the locality of NUMA than flat ones?</span>

## <span>What about NVRAM?</span>

<span>Intel and Micron have announced</span> [<span>3D Xpoint memory</span>](http://www.bbc.com/news/technology-33675734)<span>,  which is 10x denser than conventional memory (DRAM), it is 1000x faster than NAND (Flash), and it has 1000x better endurance than NAND, which wears out.</span>

<span>Could something like this be the basis for our new huge memory systems? Dave Farley in</span> [<span>The Next Big Thing?</span>](http://www.davefarley.net/?p=254) <span>says not yet, 3D Xpoint memory is still 10 times slower than DRAM.</span>

<span>And even if it was as fast as DRAM, we still don’t know how to use it all yet. It would be a shame to just use it as a faster disk. Can’t we think of a way to really reconceptualize the entire computer platform to natively take advantage of large non-volatile memory stores?</span>

<span>That’s what HP is trying to do.</span>

## <span>HP’s Memory-Driven Computing</span>

<span>Memristors were going</span> [<span>to change everything</span>](http://highscalability.com/blog/2010/5/5/how-will-memristors-change-everything.html)<span>. Not so much. Memristors are now</span> [<span>off the roadmap</span>](http://www.hpcwire.com/2015/06/11/hp-removes-memristors-from-its-machine-roadmap-until-further-notice/)<span>.</span>

<span>What do you do when your key differentiating technology flops? You pivot and rebrand. That’s what HP is doing.</span>

<span>HP is now pitching the vision of</span> [<span>Memory-Driven Computing</span>](http://h30507.www3.hp.com/t5/Innovation-HP-Labs/Memory-Driven-Computing-How-will-it-impact-the-world/ba-p/185705)<span>. If you look at</span> [<span>The Machine</span>](http://highscalability.com/blog/2014/12/15/the-machine-hps-new-memristor-based-datacenter-scale-compute.html)<span>, which was the top to bottom rebuild of a computer system around memristors, the key ideas of the system don’t actually require memristors to work. The idea at its core was to rebuild a system around the idea of huge memory spaces. And that’s still possible. You can use RAM or any of the new memory technologies that are on the horizon. HP’s innovative photonic interconnect technology still makes it possible to have a very low latency network between memory and CPU. So The Machine is still possible, just differently possible.</span>

<span>Kirk Bresniker, HP Labs Chief Architect and HP Fellow,</span> [<span>makes HP’s case</span>](http://h30507.www3.hp.com/t5/Innovation-HP-Labs/Memory-Driven-Computing-How-will-it-impact-the-world/ba-p/185705#.VcIkDtNVhBc)<span>:</span>

> <span>The Machine will fuse memory and storage, flatten complex data hierarchies, bring processing closer to the data, embed security control points throughout the hardware and software stacks, and enable management and assurance of the system at scale. It may seem counter-intuitive, but by concentrating on massive pools of non-volatile memory, we expect to spur innovation in computation by allowing many different models of computation to work on the same massive data sets. Quantum, deep neural net, carbon-nanotube, non-linear analog – all of these models could be working in concert, connected to petabytes and exabytes of information derived from a world of intelligent devices.</span>

<span>What does the revamped Machine look like?</span> [<span>Kimberly Keeton</span>](http://www.hpl.hp.com/people/kimberly_keeton/)<span>, a researcher at HP Labs, in</span> [<span>Reimagining systems and application software for The Machine</span>](https://www.youtube.com/watch?v=WZbPyV5AnKM)<span>, gives a very nice overview of how they are rethinking their system in the context of huge memory systems. So far it’s the most serious source of thought leadership I’ve found on the topic. There’s more detail in the talk, but here are a few highlights.</span>

<span>HP is taking what they call a</span> <span>**Shared Something**</span> <span>approach in their computer architecture. Shared Everything is a big computer running a single operating system. A Shared Nothing is a cluster of individual computers connected via network, each node runs its own OS.</span>

<span>Shared Something is a middle ground. The Machine has a shared pool of non-volatile memory, but individual compute nodes run their own OS and communicate via shared memory. So</span> [<span>shared memory</span>](https://en.wikipedia.org/wiki/Shared_memory_(interprocess_communication)) <span>might be back.</span>

<span>The idea is their photonic interconnect technology will make the shared memory have both low latency and consistent latency. HP is creating a specialized version of Linux that is reworked to optimally handle global non-volatile shared memory management. A lot of overhead can be removed once you obliterate the assumption of slow media access times. The video goes into more detail on all the changes they are making to optimize the OS for this new computing paradigm. HP predicts, for example, that a database will perform 100x better.</span>

<span>You may not recall, but at one time shared memory is how different processes on Unix machines talked to each other. When sockets and TCP/UDP arrived that all changed and shared memory was something high performance computing people did. Now shared memory may be back. Which makes sense, there’s no reason to shuttle data around when it’s accessible in a flat space. This insight has big implications up and down the entire stack.</span>

<span>Let’s say you want to move beyond the disk based paradigm and just want to keep everything in memory. How do you handle consistency? The potential consistency problems that occur when a program crashes in a global shared memory system are dealt with by a system HP calls Atlas.</span>

<span>The idea is a program differentiates between persistent and transient data. The persistent data lives in a persistent region, which is mapped into the processes address space. The programmer writes the same multithreaded code they would normally write.  The runtime system will automatically annotate the code to do the logging and cache line flushes necessary to maintain consistency after a crash. I couldn’t tell if this was a type of</span> [<span>software transactional memory</span>](https://en.wikipedia.org/wiki/Software_transactional_memory)<span>.</span>

<span>HP has a pretty audacious vision. And they are putting a lot talent and money behind the effort. A lot of original thinking is going into making a high margin proprietary machine. Just like the good old days. Will this work? Or will commodity/open source computing win in the end?</span>

## <span>Wrapping Up</span>

<span>Huge memory systems are on the way. How are we going to program them? If you have any ideas feel free to make a comment. Your insight will be appreciated.</span>

## <span>Related Articles</span>

*   [On HackerNews](https://news.ycombinator.com/item?id=10010747)

*   [On Reddit](https://www.reddit.com/r/programming/comments/3fzgbm/how_do_you_program_a_computer_with_10_terabytes/)

*   [Flash Disruption Comes To Server Main Memory](http://www.theplatform.net/2015/08/05/flash-disruption-comes-to-server-main-memory/)

*   <span>[Busting 4 Modern Hardware Myths - Are Memory, HDDs, And SSDs Really Random Access?](http://highscalability.com/blog/2013/6/13/busting-4-modern-hardware-myths-are-memory-hdds-and-ssds-rea.html)</span>

*   [<span>numa - overview of Non-Uniform Memory Architecture</span>](http://man7.org/linux/man-pages/man7/numa.7.html)

*   [<span>If RAM is cheap, why don't we load everything to RAM and run it from there?</span>](http://superuser.com/questions/637302/if-ram-is-cheap-why-dont-we-load-everything-to-ram-and-run-it-from-there)

*   [<span>64 Terabyte RAM computer from SGI</span>](https://news.ycombinator.com/item?id=7302770)

*   [<span>Memory-Driven Computing – How will it impact the world?</span>](http://h30507.www3.hp.com/t5/Innovation-HP-Labs/Memory-Driven-Computing-How-will-it-impact-the-world/ba-p/185705)

*   [<span>The Next Big Thing?</span>](http://www.davefarley.net/?p=254)

*   [<span>The Machine: HP's New Memristor Based Datacenter Scale Computer - Still Changing Everything</span>](http://highscalability.com/blog/2014/12/15/the-machine-hps-new-memristor-based-datacenter-scale-compute.html)

*   [<span>Innovative Operating Memory Architecture for Computers using the Data Driven Computation Model</span>](http://www.uni-obuda.hu/journal/Vokorokos_Mados_Adam_Balaz_43.pdf)

*   [<span>HP Removes Memristors from Its ‘Machine’ Roadmap Until Further Notice</span>](http://www.hpcwire.com/2015/06/11/hp-removes-memristors-from-its-machine-roadmap-until-further-notice/)

*   [<span>Reimagining systems and application software for The Machine - A sneak peek from HP Labs</span>](https://www.youtube.com/watch?v=WZbPyV5AnKM)

*   [<span>HP Labs Peeks Under the Hood of The Machine</span>](https://www.youtube.com/watch?v=hTtY5hOLPXE)

*   [<span>SGI Launches UltraViolet Supers Ahead Of Schedule</span>](http://www.theplatform.net/2015/07/14/sgi-launches-ultraviolet-supers-ahead-of-schedule/)

*   [<span>Is It Time To Get Rid Of The Linux OS Model In The Cloud?</span>](http://highscalability.com/blog/2012/1/19/is-it-time-to-get-rid-of-the-linux-os-model-in-the-cloud.html)

*   <span>[Ask For Forgiveness Programming - Or How We'll Program 1000 Cores](http://highscalability.com/blog/2012/3/6/ask-for-forgiveness-programming-or-how-well-program-1000-cor.html)</span>

</div>