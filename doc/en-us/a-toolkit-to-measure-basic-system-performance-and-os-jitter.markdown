## [A Toolkit to Measure Basic System Performance and OS Jitter](/blog/2015/5/27/a-toolkit-to-measure-basic-system-performance-and-os-jitter.html)

    

    

![](https://farm6.staticflickr.com/5348/18091159212_9955d7db9d_o.jpg)

_Jean Dagenais published a great response on a [mechanical-sympathy thread](https://groups.google.com/forum/#!msg/mechanical-sympathy/GuYJZIifrZE/SbF5dQs8dR0J) to Gil Tene's article, [The Black Magic Of Systematically Reducing Linux OS Jitter](http://highscalability.com/blog/2015/4/8/the-black-magic-of-systematically-reducing-linux-os-jitter.html). It's full of helpful tools for tracking down jitter problems. I apologize for the incomplete attribution. I did not find a web presence for Jean. _

To complement the great information I got on the “Systematic Way to Find Linux Jitter”, I have created a toolkit that I now used to evaluate current and future trading platforms.

In case this can be useful, I have listed these tools, as well as the URLs to get the source code and a description of their usage. I am learning a lot by reading the source code, and the blog entry associated.

This is far from an exhaustive list, as every week I find either a new problem area or a new tool that improve my understanding of this beautiful problem domain ;)

These tools are grouped into these categories: 

1.  CPU, Memory, Disk, Network
2.  X86, Linux, and Java time resolution
3.  Context Switches & Inter Thread Latency
4.  System Jitter
5.  Application Building Blocks: distruptor, openHft, Aeron & Workload Generator
6.  Application Performance Testing

Happy Benchmarking and Jitter Chasing!

**1\. CPU, Memory, Disk, Network**

**1.1 lmbench - [http://www.bitmover.com/lmbench/](http://www.bitmover.com/lmbench/)** 

lmbench provides basic performance data for multiple aspects of the server, from instructions, fork, context switches, disk, network, and memory, and more

**1.2.1 Memory - lmbench – lat_mem_rd** 

The article describes how to measure memory access, and I used this approach to measure the performance of the memory subsystem under local and remote access (e.g. NUMA effect)

*   [Untangling memory access measurements - lat_mem_rd](https://www.ibm.com/developerworks/community/wikis/home?lang=en#!/wiki/W51a7ffcf4dfd_4b40_9d82_446ebc23c550/page/Untangling%20memory%20access%20measurements%20-%20memory%20latency) 

**1.2.2 Java Memory Access Patterns – Martin Thompson**

Great article and a useful tool that highlight impact of local/remote memory access and NUMA effects.

*   [Memory Access Patterns Are Important](http://mechanical-sympathy.blogspot.ca/2012/08/memory-access-patterns-are-important.html)

**1.3 Disk Performance - fio - [FIO Source Code](http://git.kernel.dk/?p=fio.git;a=summary)** 

**1.4 Network**

There are many tools available, and these are the ones I find the most useful. They are useful to tune network parameters (e.g. kernel bypass drivers) when measuring throughput, latency, and jitter.

*   netperf - [http://www.netperf.org/netperf/](http://www.netperf.org/netperf/)
*   iperf3 - [http://software.es.net/iperf/](http://software.es.net/iperf/)
*   sockperf - [https://code.google.com/p/sockperf/](https://code.google.com/p/sockperf/)
*   sfnettest - [Very useful tool to measure throughput/latency based on different message size](http://www.openonload.org/download/sfnettest/) 

**Java Ping - Nitsan Wakart network-measuring tools**

*   [Java Ping Performance Baseline Utility](http://psy-lob-saw.blogspot.ca/2012/12/java-ping-performance-baseline-utility.html)
*   [A Java Ping Buffer](http://psy-lob-saw.blogspot.ca/2013/07/a-java-ping-buffet.html)

Note: The c/c++ network tools will behave differently from Java (e.g. jit, garbage generation, behavior) as such they are very complementary and offer many options that can be tested easily.

**2\. X86, Linux, and Java time resolution**

**2.1 Measuring granularity and latency of currentTimeMillis() & nanoTime(); - Aleksey Shipilev**

Great article and use the jmh to test granularity and latency. 

*   [Nanotrusing Nanotime](http://shipilev.net/blog/2014/nanotrusting-nanotime/) 

**2.1 Measuring Latency in Linux**

Provides information about how Linux measure time, and a test program that displays the different timers available and their resolution.

*   [Clock Sources in Linux](http://btorpey.github.io/blog/2014/02/18/clock-sources-in-linux/)

**3\. Context Switches & Inter Thread Latency**

**3.1 This is a good write up on Stack Overflow** 

*   [Context Switches Much Slower on New Linux Kernel](http://stackoverflow.com/questions/12111954/context-switches-much-slower-in-new-linux-kernels)
*   [Source Code for Measuring Thread Context Switch Latency](http://pastebin.com/3sx8Vhf1)

**3.2 Java and C++ Inter Thread Latency – Martin Thompson**

*   [Inter Thread Latency](http://mechanical-sympathy.blogspot.com/2011/08/inter-thread-latency.html)

**4\. System Jitter**

**4.1 Sysjitter - **[SysJitter from Solarflare - OpenOnload](http://www.openonload.org/download/sysjitter/)

sysjitter measures the extent to which the system impacts on user-level code by causing jitter.  It runs a thread on each processor core, and when the thread is "knocked off" the core it measures how long for.  At the end of the run it outputs some summary statistics for each core, and optionally the full raw data.

**4.2 jHicckup – Gil Tene – Azul - **[jHiccup](http://www.azulsystems.com/jHiccup)

jHiccup is an open source tool designed to measure the pauses and stalls (or “hiccups”) associated with an application’s underlying Java runtime platform. The new tool captures the aggregate effects of the Java Virtual Machine (JVM), operating system, hypervisor (if used) and hardware on application stalls and response time. 

**4.3 MicroJitterSampler - Peter Lawrey - **[Micro Jitter Busy Waiting and Binding](http://vanillajava.blogspot.ca/2013/07/micro-jitter-busy-waiting-and-binding.html)

The micro jitter sampler looks at interrupts to a running thread.  It is similar to jHiccup but instead of measuring how delayed a thread is in waking up, it measures how delays a thread gets once it has started running.  Surprisingly how you run your threads impacts the sort of delays it will see once it wakes up.

**5\. Application Building Blocks: distruptor, openHft, Aeron, etc**

The Disruptor, openHft, and Aeron contain programs that are useful to measure the “raw” performance of each building block/library/framework before we incorporate them with our applications. 

It’s a lot more difficult to figure out the contribution/limitations of each component when doing a macro benchmark of 20 jvms, 5 physical servers, too many cores to count them, and 10,000’s of operations per second. 

*   Disruptor -[ ](http://goog_1863426505/)[The Disruptor is a general-purpose mechanism for solving a difficult problem in concurrent programming.](https://lmax-exchange.github.io/disruptor/)
*   openHft - [Millions of operations per second in a single thread with micro-second latencies](http://openhft.net/)
*   Aeron - [Efficient reliable unicast and multicast message transport](https://github.com/real-logic/Aeron)

I am quite impressed by Aeron, which is both an eye opener (e.g. It can process 11,000,000 messages per second @ 500MB/second on our server), and a major source of learning (from design to implementation) for me. It shows what’s possible to do in Java, when you understand and apply “current best practices”. 

Great work by Martin Thompson and team.

**5.2 tress-ng**

This tool is used to generate “synthetic workload” on a server. It’s useful for measuring “interferences” with a running app. E.g. what happen if we add this kind of workload on a server.

*   [http://kernel.ubuntu.com/~cking/stress-ng/](http://kernel.ubuntu.com/~cking/stress-ng/)
*   [http://kernel.ubuntu.com/~cking/tarballs/stress-ng/](http://kernel.ubuntu.com/~cking/tarballs/stress-ng/)

stress-ng will stress test a computer system in various selectable ways. It was designed to exercise various physical subsystems of a computer as well as the various operating system kernel interfaces.

**6\. Application Performance Testing**

It takes me about 1 day to run most of the tools mentioned before and collect performance data and analyze it. The baseline data is used to assess if the platform/server performance and stability (e.g. jitter) is good enough to go the next level of testing. In general the answer is no, and this will require the use of different tricks (e.g. stopping services, bios setup, numactl, taskset, os settings, and everything mentioned in the other threads) to get an acceptable level.

The application benchmark turns out to be the most valuable but very challenging, as any performance limitations/jitter of the underlying platform and services will now be amplified by the complexity of the application components and their interactions.

For example, these are a few things that are challenging to observe and understand

*   100’s of app & gc threads running on different cores, sockets, and servers
*   Linux scheduler moving threads around cores/socket/NUMA nodes
*   Different application threading models for reader, worker, and sender threads (BUSY_SPIN, WAIT, BLOCKED, ASYNC)
*   off-heap memory access and the bdflush process cleaning dirty pages and causing disk io bottleneck
*   Messaging infrastructure and appliances …

    