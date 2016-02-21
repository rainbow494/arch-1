## [The Secret to 10 Million Concurrent Connections -The Kernel is the Problem, Not the Solution](/blog/2013/5/13/the-secret-to-10-million-concurrent-connections-the-kernel-i.html)

    

    

<iframe width="250" height="188" align="right" src="http://www.youtube.com/embed/73XNtI0w7jA" frameborder="0" allowfullscreen=""></iframe>

    Now that we have the     [    C10K concurrent connection problem    ](http://www.kegel.com/c10k.html)     licked, how do we level up and support 10 million concurrent connections? Impossible you say. Nope, systems right now are delivering 10 million concurrent connections using techniques that are as radical as they may be unfamiliar.    

    To learn how it’s done we turn to     [    Robert Graham    ](https://twitter.com/ErrataRob)    , CEO of Errata Security, and his absolutely fantastic talk at     [    Shmoocon 2013    ](https://www.shmoocon.org/)     called     [    C10M Defending The Internet At Scale    ](http://www.youtube.com/watch?v=73XNtI0w7jA#!)    .    

Robert has a brilliant way of framing the problem that I’ve never heard of before. He starts with a little bit of history, relating how Unix wasn’t originally designed to be a general server OS, it was designed to be a control system for a telephone network. It was the telephone network that actually transported the data so there was a clean separation between the control plane and the data plane. The **problem is we now use Unix servers as part of the data plane**, which we shouldn’t do at all. If we were designing a kernel for handling one application per server we would design it very differently than for a multi-user kernel. 

    Which is why he says the key is to understand:    

*   The kernel isn’t the solution. **The kernel is the problem**.

    Which means:    

*   **Don’t let the kernel do all the heavy lifting**. Take packet handling, memory management, and processor scheduling out of the kernel and put it into the application, where it can be done efficiently. Let Linux handle the control plane and let the the application handle the data plane.

The result will be a system that can handle 10 million concurrent connections with 200 clock cycles for packet handling and 1400 hundred clock cycles for application logic. As a main memory access costs 300 clock cycles it’s key to design in way that minimizes code and cache misses.

    With a         **data plane oriented system**         you can process 10 million packets per second. With a control plane oriented system you only get 1 million packets per second.    

    If this seems extreme keep in mind the old saying:         **scalability is specialization**        . To do something great you can’t outsource performance to the OS. You have to do it yourself.    

    Now, let’s learn how Robert creates a system capable of handling 10 million concurrent connections...    

##     C10K Problem - So Last Decade     

    A decade ago engineers tackled the C10K scalability problems that prevented servers from handling more than 10,000 concurrent connections. This problem was solved by fixing OS kernels and moving away from threaded servers like Apache to event-driven servers like Nginx and Node. This process has taken a decade as people have been moving away from Apache to scalable servers. In the last few years we’ve seen faster adoption of scalable servers.    

###     The Apache Problem    

*       The Apache problem is the more connections the worse the performance.    

*       Key insight:         **performance and scalability or orthogonal concepts**        . They don’t mean the same thing. When people talk about scale they often are talking about performance, but there’s a difference between scale and performance. As we’ll see with Apache.    

*       With short term connections that last a few seconds, say a quick transaction, if you are executing a 1000 TPS then you’ll only have about a 1000 concurrent connections to the server.    

*       Change the length of the transactions to 10 seconds, now at 1000 TPS you’ll have 10K connections open. Apache’s performance drops off a cliff though which opens you to DoS attacks. Just do a lot of downloads and Apache falls over.    

*       If you are handling 5,000 connections per second and you want to handle 10K, what do you do? Let’s say you upgrade hardware and double it the processor speed. What happens? You get double the performance but you don’t get double the scale. The scale may only go to 6K connections per second. Same thing happens if you keep on doubling. 16x the performance is great but you still haven’t got to 10K connections. Performance is not the same as scalability.    

*       The problem was Apache would fork a CGI process and then kill it. This didn’t scale.    

*       Why? Servers could not handle 10K concurrent connections because of O(n^2) algorithms used in the kernel.    

    *       Two basic problems in the kernel:    

        *       Connection = thread/process. As a packet came in it would walk down all 10K processes in the kernel to figure out which thread should handle the packet    

        *       Connections = select/poll (single thread). Same scalability problem. Each packet had to walk a list of sockets.    

    *       Solution: fix the kernel to make lookups in constant time    

        *       Threads now constant time context switch regardless of number of threads.    

        *       Came with a new scalable epoll()/IOCompletionPort constant time socket lookup.    

*       Thread scheduling still didn’t scale so servers scaled using epoll with sockets which led to the asynchronous programming model embodied in Node and Nginx. This shifted software to a different performance graph.  Even with a slower server when you add more connections the performance doesn’t drop off a cliff. At 10K connections a laptop is even faster than a 16 core server.    

##     The C10M Problem - The Next Decade     

    In the very near future servers will need to handle millions of concurrent connections. With IPV6 the number of potential connections from each server is in the millions so we need to go to the next level of scalability.    

*       Examples of applications that will need this sort of scalability: IDS/IPS because they connection to a server backbone. Other examples: DNS root server, TOR node, Nmap of Internet, video streaming, banking, Carrier NAT, Voip PBX, load balancer, web cache, firewall, email receive, spam filtering.    

*       Often people who see Internet scale problems are appliances rather than servers because they are selling hardware + software. You buy the device and insert it into your datacenter. These devices may contain an Intel motherboard or Network processors and specialized chips for encryption, packet inspection, etc.    

*       X86 prices on Newegg as of Feb 2013 - $5K for 40gpbs, 32-cores, 256gigs RAM. The servers can do more than 10K connections. If they can’t it’s because you’ve made bad choices with software. It’s not the underlying hardware that’s the issues. This hardware can easily scale to 10 million concurrent connections.    

###     What the 10M Concurrent Connection Challenge means:    

1.      10 million concurrent connections    

2.      1 million connections/second - sustained rate at about 10 seconds a connections    

3.      10 gigabits/second connection - fast connections to the Internet.    

4.      10 million packets/second - expect current servers to handle 50K packets per second, this is going to a higher level. Servers used to be able to handle 100K interrupts per second and every packet caused interrupts.    

5.      10 microsecond latency - scalable servers might handle the scale but latency would spike.    

6.      10 microsecond jitter - limit the maximum latency    

7.      10 coherent CPU cores - software should scale to larger numbers of cores. Typically software only scales easily to four cores. Servers can scale to many more cores so software needs to be rewritten to support larger core machines.    

###     We’ve Learned Unix Not Network Programming     

*       A generation of programmers has learned network programming by reading Unix Networking Programming by W. Richard Stevens. The problem is the book is about Unix, not just network programming. It tells you to let Unix do all the heavy lifting and you just write a small little server on top of Unix. But the kernel doesn’t scale. The solution is to move outside the kernel and do all the heavy lifting yourself.    

*       An example of the impact of this is to consider Apache’s thread per connection model. What this means is the thread scheduler determines which read() to call next depending on which data arrives.         **You are using the thread scheduling system as the packet scheduling system**        . (I really like this, never thought of it that way before).    

*       What Nginx says it don’t use thread scheduling as the packet scheduler. Do the packet scheduling yourself. Use select to find the socket, we know it has data so we can read immediately and it won’t block, and then process the data.    

*   Lesson: **Let Unix handle the network stack, but you handle everything from that point on**.

###     How do you write software that scales?    

    How do change your software to make it scale? A lot of or rules of thumb are false about how much hardware can handle. We need to know what the performance capabilities actually are.    

    To go to the next level the problems we need to solve are:    

1.  packet scalability
2.  multi-core scalability
3.  memory scalability 

###     Packet Scaling - Write Your Own Custom Driver to Bypass the Stack    

*       The problem with packets is they go through the Unix kernel. The network stack is complicated and slow. The path of packets to your application needs to be more direct. Don’t let the OS handle the packets.    

*       The way to do this is to write your own driver. All the driver does is send the packet to your application instead of through the stack. You can find drivers: PF_RING, Netmap, Intel DPDK (data plane development kit). The Intel is closed source, but there’s a lot of support around it.    

*       How fast? Intel has a benchmark where the process 80 million packets per second (200 clock cycles per packet) on a fairly lightweight server. This is through user mode too. The packet makes its way up through to user mode and then down again to go out. Linux doesn’t do more than a million packets per second when getting UDP packets up to user mode and out again. Performance is 80-1 of a customer driver to a Linux.    

*       For the 10 million packets per second goal if 200 clock cycles are used in getting the packet that leaves 1400 clocks cycles to implement functionally like a DNS/IDS.    

*       With PF_RING you get raw packets so you have to do your TCP stack. People are doing user mode stacks. For Intel there is an available TCP stack that offers really scalable performance.    

###     Multi-Core Scalability    

    Multi-core scalability is not the same thing as multi-threading scalability. We’re all familiar with the idea processors are not getting faster, but we are getting more of them.    

    Most code doesn’t scale past 4 cores. As we add more cores it’s not just that performance levels off, we can get slower and slower as we add more cores. That’s because software is written badly. We want software as we add more cores to scale nearly linearly. Want to get faster as we add more cores.    

####     Multi-threading coding is not multi-core coding    

*       Multi-threading:    

    *       More than one thread per CPU core    

    *       Locks to coordinate threads (done via system calls)    

    *       Each thread a different task    

*       Multi-core:    

    *       One thread per CPU core    

    *       When two threads/cores access the same data they can’t stop and wait for each other    

    *       All threads part of the same task    

*       Our problem is how to spread an application across many cores.    

*       Locks in Unix are implemented in the kernel. What happens at 4 cores using locks is that most software starts waiting for other threads to give up a lock. So the kernel starts eating up more performance than you gain from having more CPUs.    

*       What we need is an architecture that is more like a freeway than an intersection controlled by a stop light. We want no waiting where everyone continues at their own pace with as little overhead as possible.    

*       Solutions:    

    *   Keep data structures per core. Then on aggregation read all the counters.
    *   Atomics. Instructions supported by the CPU that can called from C. Guaranteed to be atomic, never conflict. Expensive, so don’t want to use for everything.
    *   Lock-free data structures. Accessible by threads that never stop and wait for each other. Don’t do it yourself. It’s very complex to work across different architectures.
    *   Threading model. Pipelined vs worker thread model. It’s not just synchronization that’s the problem, but how your threads are architected.
    *   Processor affinity. Tell the OS to use the first two cores. Then set where your threads run on which cores. You can also do the same thing with interrupts. So you own these CPUs and Linux doesn’t.

## Memory Scalability

*       The problem is if you have 20gigs of RAM and let’s say you use 2k per connection, then if you only have 20meg L3 cache, none of that data will be in cache. It costs 300 clock cycles to go out to main memory, at which time the CPU isn’t doing anything.    

*       Think about this with our 1400 clock cycle budge per packet. Remember 200 clocks/pkt overhead. We only have 4 cache misses per packet and that's a problem.    

*       Co-locate Data    

    *       Don’t scribble data all over memory via pointers. Each time you follow a pointer it will be a cache miss: [hash pointer] -> [Task Control Block] -> [Socket] -> [App]. That’s four cache misses.    

    *       Keep all the data together in one chunk of memory: [TCB | Socket | App]. Prereserve memory by preallocating all the blocks. This reduces cache misses from 4 to 1.    

*       Paging    

    *       The paging table for 32gigs require 64MB of paging tables which doesn’t fit in cache. So you have two caches misses, one for the paging table and one for what it points to. This is detail we can’t ignore for scalable software.    

    *       Solutions: compress data; use cache efficient structures instead of binary search tree that has a lot of memory accesses     

    *       NUMA architectures double the main memory access time. Memory may not be on a local socket but is on another socket.    

*       Memory pools    

    *       Preallocate all memory all at once on startup.    

    *       Allocate on a per object, per thread, and per socket basis.    

*       Hyper-threading    

    *       Network processors can run up to 4 threads per processor, Intel only has 2\.    

    *       This masks the latency, for example, from memory accesses because when one thread waits the other goes at full speed.    

*       Hugepages    

    *       Reduces page table size. Reserve memory from the start and then your application manages the memory.    

###     Summary     

*       NIC    

    *       Problem: going through the kernel doesn’t work well.    

    *       Solution: take the adapter away from the OS by using your own driver and manage them yourself    

*       CPU    

    *       Problem: if you use traditional kernel methods to coordinate your application it doesn’t work well.    

    *       Solution: Give Linux the first two CPUs and you application manages the remaining CPUs. No interrupts will happen on those CPUs that you don’t allow.    

*       Memory    

    *       Problem: Takes special care to make work well.    

    *       Solution: At system startup allocate most of the memory in hugepages that you manage.    

    The control plane is left to Linux, for the data plane, nothing. The data plane runs in application code. It never interacts with the kernel. There’s no thread scheduling, no system calls, no interrupts, nothing.     

    Yet, what you have is code running on Linux that you can debug normally, it’s not some sort of weird hardware system that you need custom engineer for. You get the performance of custom hardware that you would expect for your data plane, but with your familiar programming and development environment.     

##     Related Articles     

*   Read on [Hacker News](https://news.ycombinator.com/item?id=5699552) or [Reddit](http://www.reddit.com/r/programming/comments/1e90q7/the_secret_to_10_million_concurrent_connections/), [Hacker News Dos](https://news.ycombinator.com/item?id=7697483)
*       [Is It Time To Get Rid Of The Linux OS Model In The Cloud?](http://highscalability.com/blog/2012/1/19/is-it-time-to-get-rid-of-the-linux-os-model-in-the-cloud.html)    
*   [    Machine VM + Cloud API - Rewriting The Cloud From Scratch    ](http://highscalability.com/blog/2010/10/21/machine-vm-cloud-api-rewriting-the-cloud-from-scratch.html)
*   [    Exokernel    ](http://en.wikipedia.org/wiki/Exokernel)        
*   [    Blog on C10M    ](http://c10m.robertgraham.com/)
*   [    Multi-core scaling: it’s not multi-threaded    ](http://erratasec.blogspot.com/search/label/Shmoocon2013)     with some good comment action.    
*       [Intel® DPDK: Data Plane Development Kit](http://dpdk.org/)    

    