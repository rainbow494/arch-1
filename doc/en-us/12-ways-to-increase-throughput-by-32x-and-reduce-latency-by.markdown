## [12 Ways to Increase Throughput by 32X and Reduce Latency by  20X](/blog/2012/5/2/12-ways-to-increase-throughput-by-32x-and-reduce-latency-by.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, May 2, 2012 at 9:15AM</span></div>

<div class="body">

![](http://farm5.static.flickr.com/4102/4781494225_a6abce3a02_m.jpg)

[Martin Thompson](http://www.linkedin.com/in/martinjthompson), a high-performance technology geek, has written an awesome post, <span>[Fun with my-Channels Nirvana and Azul Zing](http://mechanical-sympathy.blogspot.co.uk/2012/03/fun-with-my-channels-nirvana-and-azul.html). In it Martin</span><span> shows the process and techniques he used to take an existing messaging product, written in Java, and increase throughput by 32X and reduce latency by  20X. The article is very well written with lots of interesting details that make it well worth reading.</span>

## <span>The Techniques</span>

<span>The techniques are a good mix of Java independent approaches, things you may have heard of before, things you probably haven’t heard of  before, and Java specific heroics. The obvious winning technique is to create a lock free flow through the system. This shouldn’t be surprising as Martin teaches what sounds like a really good</span> [<span>lock-free concurrency course</span>](http://instil.co/courses/writing-concurrent-code-with-lock-free-algorithms/)<span>.</span>

1.  <span>Do not use locks in the main transaction flow because they cause context switches, and therefore latency and unpredictable jitter.</span>
2.  <span>Never have more threads that need to run than you have cores available.</span>
3.  <span>Set affinity of threads to cores, or at least sockets, to avoid cache pollution by avoiding migration.  This is particularly important when on a server class machine having multiple sockets because of the NUMA effect.</span>
4.  <span>Ensure uncontested access to any resource respecting the</span> [<span>Single Writer Principle</span>](http://mechanical-sympathy.blogspot.de/2011/09/single-writer-principle.html) <span>so that the likes of biased locking can be your friend.</span>
5.  <span>Keep call stacks reasonably small.  Still more work to do here.  If you are crazy enough to use Spring, then check out your call stacks to see what I mean!  The garbage collector has to walk them finding reachable objects.</span>
6.  <span>Do not use finalizers.</span>
7.  <span>Keep garbage generation to modest levels.  This applies to most JVMs but is likely not an issue for Zing.</span>
8.  <span>Ensure no disk IO on the main flow.</span>
9.  <span>Do a proper warm-up before beginning to measure.</span>
10.  <span>Do all the appropriate OS tunings for low-latency systems that are way beyond this blog.  For example turn off C-States power management in the BIOS and watch out for RHEL 6 as it turns it back on without telling you!</span>
11.  <span>Macro-benchmarking is much more valuable than micro-benchmarking.</span>
12.  <span>Amazing results are achieved by truly agile companies, staffed by talented individuals, who are empowered to make things happen. Make sh*t happen is more important than following a process.</span>

## <span>The Process</span>

<span>Of equal interest to the techniques for me is the process he followed to figure out what was going on. To come in cold on a complex application is difficult. It would be interesting to learn more about that part of the gig, but here’s some of what was extractable.</span>

*   <span>Ran initial tests to get a general feel for the state of the product.</span>
    *   <span>Determined the product actually performed well.</span>
*   <span>Created appropriate load tests and got the profilers running.  </span>
    *   <span>Lock-contention was the biggest culprit limiting both latency and throughput.  </span>
*   <span>By the</span> [<span>Theory of Constraints</span>](http://en.wikipedia.org/wiki/Theory_of_constraints) <span>the idea is to work on the most limiting factor because when it is removed the list below it can change radically as new pressures are applied.</span>
    *   <span>A new lock-free</span> [<span>Executor</span>](http://docs.oracle.com/javase/1.5.0/docs/api/java/util/concurrent/Executor.html) <span>was created to replace the standard Java implementation and was 10x better.  </span>
    *   <span>The system can now cope with 16X more throughput, and the latency histogram is much more compressed.</span>
*   <span>Azul Zing had by far the best latency profile with virtually no long tail and for many tests had the greatest throughput.</span>
*   <span>The next big bottleneck TCP between processes over the loopback adapter.</span>
    *   <span>Created a new IPC transport based on shared memory via memory-mapped files in Java.  Worked closely with Azul to get the best performance.</span>
    *   <span>Achieved 40 million+ messages per second at latencies just over 100ns for the new IPC transport.</span>
*   <span>Went on removing latency problems. No details were provided, but may be coming later.</span>

</div>