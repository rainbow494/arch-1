## [Aeron: Do we really need another messaging system?](/blog/2014/11/17/aeron-do-we-really-need-another-messaging-system.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, November 17, 2014 at 9:26AM</span></div>

<div class="body">

![](https://farm8.staticflickr.com/7507/15617490120_4af847299f_o.jpg)

<span id="docs-internal-guid-cc656d1c-bea5-c984-acbb-560faef4d83e"></span>

<span>Do we really need</span> [<span>another</span>](https://www.youtube.com/watch?v=dq4aOaDXIfY) <span>messaging system? We might if it promises to move millions of messages a second, at small microsecond latencies between machines, with consistent response times, to large numbers of clients, using an innovative design.  </span>

<span>And that’s the promise of</span> [<span>Aeron</span>](https://github.com/real-logic/Aeron) <span>(the</span> [<span>Celtic god</span>](http://en.wikipedia.org/wiki/Aeron_%28Celtic_mythology%29) <span>of battle, not the chair, though tell that to the search engines), a new high-performance open source message transport library from the team of</span> [<span>Todd Montgomery</span>](https://twitter.com/toddlmontgomery)<span>, a multicast and reliable protocol expert,</span> [<span>Richard Warburton</span>](http://insightfullogic.com/blog/)<span>, an expert on compiler optimizations, and</span> [<span>Martin Thompson</span>](https://twitter.com/mjpt777)<span>, the pasty faced performance gangster.</span>

<span>The claims are Aeron is already beating the best products out there on throughput and latency matches the best commercial products up to the 90th percentile. Aeron can push small 40 byte messages at 6 million messages a second, which is a very difficult case.</span>

<span>Here’s a talk Martin gave on Aeron at Strangeloop:</span> [<span>Aeron: Open-source high-performance messaging</span>](https://www.youtube.com/watch?v=tM4YskS94b0)<span>. I’ll give a gloss of his talk as well as integrating in sources of information listed at the end of this article.</span>

<span>Martin and his team were in the enviable position of having a client that required a product like Aeron and was willing to both finance its development while also making it open source. So go git</span> [<span>Aeron on GitHub</span>](https://github.com/real-logic/Aeron)<span>. Note, it’s early days for Aeron and they are still in the heavy optimization phase.</span>

<span>The world has changed therefore endpoints need to scale as never before. This is why Martin says we need a new messaging system. It’s now a multi-everything world. We have multi-core, multi-socket, multi-cloud, multi-billion user computing, where communication is happening all the time. Huge numbers of consumers regularly pound a channel to read from same publisher, which causes lock contention, queueing effects, which causes throughput to drop and latency to spike. </span>

<span>What’s needed is a new messaging library to make the most of this new world. The</span> [<span>move to microservices</span>](https://groups.google.com/forum/#!topic/mechanical-sympathy/fr7moLcsuiI) <span>only heightens the need:</span>

<span>As we move to a world of micro services then we need very low and predictable latency from our communications otherwise the coherence component of</span> [<span>USL</span>](http://www.perfdynamics.com/Manifesto/USLscalability.html) <span>will come to rain fire and brimstone on our designs.</span>

<span>With Aeron the goal is to keep things pure and focused. The benchmarking we have done so far suggests a step forward in throughput and latency. What is quite unique is that you do not have to choose between throughput and latency. With other high-end messaging transports this is a distinct choice. The algorithms employed by Aeron give maximum throughput while minimising latency up until saturation.</span>

“Many messaging products are a Swiss Army knife; Aeron is a scalpel,” [<span>says</span>](https://groups.google.com/d/msg/mechanical-sympathy/fr7moLcsuiI/IMvQY_bCQf8J) Martin, which is a good way to understand Aeron. It’s not a full featured messaging product in the way you may be used to, like [Kafka](http://kafka.apache.org/). Aeron does not persist messages, it doesn’t support guaranteed delivery, nor clustering, nor does it support topics. Aeron won’t know if a client has crashed and be able to sync it back up from history or initialize a new client from history. 

<span>The best way to place Aeron in your mental matrix might be as a message oriented replacement for TCP, with higher level services written on top. Todd Montgomery</span> [<span>expands</span>](https://groups.google.com/d/msg/mechanical-sympathy/fr7moLcsuiI/XsKndzoR6ycJ) <span>on this idea:</span>

<span>Aeron being an ISO layer 4 protocol provides a number of things that messaging systems can't and also doesn't provide several things that some messaging systems do.... if that makes any sense. Let me explain slightly more wrt all typical messaging systems (not just Kafka and 0MQ).</span> 

<span>One way to think more about where Aeron fits is TCP, but with the option of reliable multicast delivery. However, that is a little limited in that Aeron also, by design, has a number of possible uses that go well beyond what TCP can do. Here are a few things to consider:</span> 

<span>Todd continues on with more detail, so please keep reading the article to see more on the subject.</span>

<span>At its core Aeron is a</span> <span>**replicated persistent log of messages**</span><span>. And through a very conscious design process messages are wait-free and zero-copy along the entire path from publication to reception. This means latency is very good and very predictable.</span>

<span>That sums up Aeron is nutshell. It was created by an experienced team, using solid design principles sharpened on many previous projects, backed by techniques not everyone has in their tool chest. Every aspect has been well thought out to be clean, simple, highly performant, and highly concurrent.</span>

<span>If simplicity is indistinguishable from cleverness, then there’s a lot of cleverness going on in Aeron. Let’s see how they did it...</span>

<span>Please note, there’s a lot going on</span> [<span>this talk</span>](https://www.youtube.com/watch?v=tM4YskS94b0) <span>by Martin Thompson. I did my best to capture the ideas, but what you don’t get is the feeling of how well it all fits together. Martin does a great job at conveying that sense of wholeness, which is why the talk is well worth watching.</span>

## <span>Miscellaneous</span>

*   _Todd Montgomery continued.._.One way to think more about where Aeron fits is TCP, but with the option of reliable multicast delivery. However, that is a little limited in that Aeron also, by design, has a number of possible uses that go well beyond what TCP can do. Here are a few things to consider:

    *   <span>individual identification, in time, of a "persisted" stream of data with full record boundaries. This is the {channel, sessionId, channelId, offset, length} tuples that is at the heart of log buffer strategy. This allows a non-volatile storage of the stream with arbitrary playback to fall out... which is quite interesting. Which leads to doing reconnect and persistence of data streams in a truly mechanical sympathetic way. I can't stress enough what this opens up. In fact, it also opens up true location transparency within the transport protocol. i.e. a local subscriber can read directly from the publishers log buffer while the driver transparently sends data to off-box subscribers. These are the thing we've identified so far, but I think this can also apply to unique ways to handle proactive/reactive forward error correction, carouseling, arbitrary replay, etc.</span>

    *   <span>Aeron doesn't have topics. It has individual non-contended streams. Most messaging systems provide a topic space. Which is a blessing and a curse. By keeping the stream space for Aeron deliberately bounded implementation-wise (but unbounded design-wise), Aeron allows topic spaces to be built on top (which allows 0MQ and a host of other systems to leverage it) without locking the implementation into wasting resources for those use cases where it is not needed.</span>

    *   <span>the reliable unicast design is a familiar, firewall traversable design that mimics TCP. But the reliable multicast design allows for pub/sub semantics with infinitely configurable flow control strategies. This opens up all kinds of use case possibilities beyond just normal messaging, streaming, etc.</span>

*   <span>Transmission mediums have changed. Messages are not just over TCP anymore. More multicast is happening. Infiniband is taking off in the high performance space.</span> [<span>PCI Express</span>](http://en.wikipedia.org/wiki/PCI_Express) <span>3.0 has a built-in memory model so it’s possible to transfer bytes from bus to bus between machines. This is where a lot of the high performance space is going.</span>

*   <span>We need to communicate between processes and sockets on the same machine. As machines are built with more and more cores they effectively become datacenters in a box. Intel’s new</span> [<span>Haswell CPU</span>](http://www.extremetech.com/computing/189518-intels-new-18-core-haswell-xeon-chips-will-try-to-preempt-the-arm-server-onslaught) <span>has 18 cores per socket, with two hyper-threads per core. With a possible 240 threads running at the same time on a machine we need a way to break up the work and communicate efficiently.</span>

*   <span>Written in pure Java 8 and exploits that version's newly introduced lambda expressions.</span>

*   <span>Peer-to-peer, not a brokered solution, which is one of the reasons it has such low-latency.</span>

*   <span>Uses UDP and will have SHM IPC and Infiniband soon.</span>

*   <span>The secret to building high performance systems is simplicity. Complexity kills performance. Drive towards clean simple designs.</span>

*   <span>Provides its own flow control with pluggable hooks from providing alternative algorithms.</span>

*   <span>Does not support clustering and archived messages, though a future project might add some of this functionality.</span>

*   <span>Reliable multicast delivery.</span>

*   <span>Aeron essentially replicates log buffers from one machine to another. The buffers are persistent in a functional sense in that the stored records are not mutated, not persisted to disk.</span>

*   <span>Offers reliable but not guaranteed delivery. If the subscriber dies and times out then the message delivery will not be retried. A protocol could be layered on top of Aeron to archive published messages so a subscriber could recover but that is not in the base functionality.</span>

*   <span>Does not provide transactional guarantees. So, when an offer is done, when it returns it is either rejected or it is placed in the shared memory log buffer to be sent by the driver. The driver does its best to send it as flow control allows. But, Aeron does try it's best to get that to the subscribers. It's a best effort delivery guarantee much like TCP, but a slight bit better in that it can handle certain glitches in connectivity that TCP can't. But it doesn't guarantee it.</span>

*   <span>To test capture a full latency histogram while trying cases like multiple threads concurrently publishing so contention occurs. Compare with a bunch of different messaging factors as well. For example IPC/Unicast/Multicast, different message sizes etc.</span> 

## <span>Basic Operation</span>

*   <span>Publishers send messages down a channel for subscribers to read. Within channels are streams to subscribers.</span>

    *   <span>A channel keeps messages ordered. Streams are independent of each other so they can’t block each other.</span>

    *   <span>Loss is detected and dealt with with a minimal impact on latency and throughput.</span>

    *   <span>Flow control and backpressure are used so as not to overwhelm clients.</span>

    *   <span>Congestion avoidance and control deal with packets that transit congested networks. It’s considered an optional service in a high performance space. When congestion occurs this service is needed, but in a scenario where latencies need to be as low as possible a congestion control algorithm will slow you down. An example is the</span> [<span>slow-start</span>](http://en.wikipedia.org/wiki/Slow-start) <span>problem with TCP because TCP is trying to avoid swamping a network with traffic.</span>

    *   <span>Streams are multiplexed and demultiplexed in a channel while dealing with very large messages, dealing with fragmentation, and avoiding</span> [<span>head-of-line blocking</span>](http://en.wikipedia.org/wiki/Head-of-line_blocking)<span>.</span>

*   <span>Not a framework, it’s a library. It’s a composable design so other layers can be built on top of it.</span>

*   <span>The design is about three things: System architecture, data structures, and protocol interactions.</span>

    *   <span>Get the architecture right and have it nice and clean.</span>

    *   <span>Data structures are very important.</span>

    *   <span>These days there’s less of an emphasis on good protocol work, which is fundamental when distributed agents are communicating and cooperating.</span>

## <span>Architecture</span><span style="font-size: 12px;"> </span>

*   Publishers publish and subscribers subscriber. You can have two way communication between two machines, but each need to register a publisher and a subscriber each. An single pair of publisher and subscriber is just one way communication.

*   <span>Communication happens over media like UDP, UDP multicast, Infiniband, PCI-e 3.0, RDMA.</span>

*   <span>Senders are responsible for sending data across the media and receivers receive data over the media. Senders and receivers are independent agents running in their own threads doing their own thing. Modern processors are really good at doing the same things over and over again because their caches are hot for instructions and data.</span>

*   <span>Conductors are independent agents that do all the work that’s not sending bytes between A and B. This includes housekeeping and admin like user setup, events for new publications, new subscriptions, and telling the system what’s going on.</span>

*   <span>Senders and receivers are kept really simple and clean so they can ship data as fast as possible between two points over a media using the most optimal means.</span>

*   <span>No data structures are shared in the same process. Communication is using message passing. Conductors communicate over non-blocking structures using messages. This allows the use of clean single threaded code that can go really fast. Avoided are concurrency and locks using queues.</span>

*   <span>A clean separation of concerns means there are options of where parts are located. All are independent agents, with its own thread, running its own code, with its own internal data structures that are not shared, so they don’t need to be concurrent, they don’t need to have locks, so they can perform incredibly fast.</span>

    *   <span>The Conductor is split into what is required to service senders and receivers and what is needed to service the client.  And since communication isn’t using shared memory, it’s over queues, these parts of the Conductor can be split into separate processes. A Media Driver can be in its own process, and a client can be in its own process. Or the Media Driver could be in the kernel. Or in an FPGA.</span>

## <span>Data Structures</span><span style="font-size: 12px;"> </span>

*   <span>The data structures in Java have far too much complexity, too much indirection, which means low performance, so they built their own maps, IPC ring buffers, IPC broadcast buffers, ITC queues, dynamic arrays, log buffers.</span>

*   <span>Ring buffers are used to communicate between Conductors, Clients, and Drivers.</span>

*   <span>There’s also a need to send events from drivers to multiple clients without slowing down the driver regardless of the number of clients, so a broadcast is used for events.</span>

*   <span>A number of data structures change on the fly, like the number of subscribers for a given subscription, the number of publishers for a publication, these need to be dynamic and non-blocking. A Log Buffer is used to move messages from publications to the subscriptions.</span>

## <span>Persistent Log</span>

*   Messages are stored along with a header in a persistent log structure. (more on the greatness of log structures [here](http://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying)).

*   <span>The log is mapped to disk, but lives in memory, using memory mapped files. Logs live in files because files are available across processes. Files are mapped into memory, which avoids having to make system calls to go to disk.</span>

*   <span>Immutable data structures can be safe to read without locks because they just grow over time.</span>

*   <span>Adding a message is described a protocol and a state machine. No locks are involved in adding a message.</span>

    *   <span>First, the tail is moved forward atomically so another thread can be adding a message at exactly the same time.</span>

    *   <span>Now there’s space to add in a message. The message is copied to the buffer.</span>

    *   <span>To signal that a message has been completed a single word operation is written to a field in the message header.</span>

    *   <span>The message header is completely written into the log, so to send a message to a publisher only requires writing contiguous bytes of the header and the message, nothing has to formulated on the fly, so it’s very efficient to send messages to a lot of clients.</span>

*   <span>A log is not just a single file that grows over time. The single file design while common, has a lot of problems. It increase VM pressure and page churn because page faults would be happening all of the time, and page faults are very expensive (here’s Linus Torvalds on</span> [<span>page fault costs</span>](https://plus.google.com/+LinusTorvalds/posts/YDKRFDwHwr6)<span>). Page faults have not been getting faster and processes have been getting faster so the gap is enormous (though this has always been true and why page faults have always been something to avoid).</span>

*   <span>The alternative to the single file approach is to keep three buffers: clean, dirty, active. Active is the buffer you are writing to. Dirty is the persistent history. Clean is the next buffer to become Active. Cleaning can occur in the background by Conductors, so there’s no latency penalty for writing to them. The buffers are all hot in cache so there’s no page caching going on. Another thread could archive the dirty buffer to another data store so the messages could be kept forever.</span>

*   <span>Here’s how wait-free is implemented. Let’s say two writers are trying to write messages X and Y into the log at the same time by two different publishers in two different threads, in possibly different processes writing to the same driver. Since the driver runs out of process it can be shared across multiple processes. Y wins the race to increment the tail. Then X increments the tail, but the tail runs past the end of the buffer. Y can use the space it allocated. A padding record is put in between the end of Y and the end of the buffer so the buffer is always contiguous and complete. X is responsible for rotating to the next buffer, where it writes the message from the beginning of the buffer to the new tail. All of this work did not cause any delay in the threads. It happened completely independently with no blocking operations. If a process takes an interrupt none of the other threads become blocked.</span>

## <span>Sending Messages</span><span style="font-size: 12px;"> </span>

*   <span>The header contains version information; flags; the message type; frame length; term offset, which is the offset into the buffer, which is where it will be replicated to on the other side; session ID, stream ID; term ID; encoded message.  Everything needed is in the header so it can be written directly to the network. When frame length bytes have been written the message has been sent.</span>

*   <span>An interesting implication of the header design is that every byte in a stream has a unique identifier across time, which is the composite key of streamId, sessionId, termId, termOffset. This feature will be used later so a receiver can tell a sender which region of the log was dropped and so the sender can resend from that point.</span>

*   Logs are replicated using a protocol of messages between the sender and the receiver. A sender when it wants to send a message sends a setup message to the receiver. The receiver send a status message back. The sender starts sending data to the receiver. The receiver sends more status messages telling the sender that it has X amount of space left so you can keep sending. Message aren't acked, NAKs instead. Most modern multicast protocols NAK instead of ACK in order to minimise [implosion effects](ftp://www-net.cs.umass.edu/pub/Rubenst98:Proact-TR-98-19.ps.gz). The receiver is telling the sender how much space it has left, which requires fewer message than an ack based approach. And it gives you a mechanism to implement both flow control and back pressure.

*   <span>The sender sends the last message it sent as a heartbeat to the receiver. This approach also handles the case where the last message was dropped (remember, UDP is being used).</span>

*   <span>When a receiver knows a message was dropped it sends the sender a NAK for the region of the term that’s gone missing. At this point its good to remember that what is being replicated is the log so the receiver can assume the entire log is available, not just a small window size.</span>

*   <span>The receiver has copies of the same data structures that are in the sender. Headers and messages are written into the log. There are two counters: completed and high water mark. Let’s say message 1 has been written and message 3 comes in before message 2 because there’s an ordering problem. The completed counter points to the end of message 1\. The high water mark points to the end of message 3\. And there’s a hole where message 2 should be. Completed points to a contiguous stream of messages. If message 2 arrives completed is moved forward to point to the end of message 3\.</span>

*   <span>With this approach you don’t need to keep skip lists of missing messages or where there are gaps in history. These extra data structures are complicated and slow things down. They cause concurrency issues and cache misses.</span>

*   <span>The log buffers are completely linear in memory. Accessing them requires just striding forward in memory, which is really hardware friendly. Pointer chasing is really bad on performance as they cause page faults.</span>

*   <span>This representation is also very compact. You can just scream through memory.</span>

*   <span>This design is a result of going back to basics. Thinking of how to solve the problems of loss, reordering, keeping a history, while being memory and concurrency friendly. The result was a persistent data structure that’s faster than any existing approaches. A</span> [<span>persistent data structure</span>](http://en.wikipedia.org/wiki/Persistent_data_structure) <span>is a data structure that always preserves the previous version of itself when it is modified. This is an idea from the functional side of the tracks. There’s a lot to learn when disciplines work together. It’s stupid to only do functional or ignore functional techniques.</span>

*   <span>The Conductor is always looking for a gap between completed and the high water mark. It will send a NAK so the sender will resend that part of the log. This is simple and easy to debug.</span>

*   <span>To know what messages are consumed makes use of counters that point to a location in the byte stream. Publishers, Senders, Receivers, and Subscribers all keep position counters in the byte stream. This makes it easy to monitor, apply flow control, and apply congestion control. These counters are made available in another memory mapped file so they are available to separate monitoring applications.</span>

*   <span>Protocols are hard. You need to watch out for self-similar behaviours at scale. For example, in a multicast world when something goes wrong you get something called self-similar behaviour where patterns start happening and resonances get set up. So randomization must be injected into the system. A NAK must be sent at some random point in the future. This prevents subscribers from all hammering the source at once.</span> 

## <span>Lessons Learned</span><span style="font-size: 12px;"> </span>

*   **Humans suck at estimatio**n. Even with the great experience of the team their estimations were all off. Estimation is just something humans aren’t good at.

*   <span>**Building distributed systems is hard**</span><span>. Only the people who don’t want to build a distributed system are qualified to build one as they at least have an appreciation for how hard the job will be. You just can’t say hey, let's make a distributed, concurrent system. Make a single threaded system first and the work the rest out later.</span>

*   <span>**We have more defensive code than feature code**</span><span>. On a distributed system things can go wrong on a massive scale and those failures need to be handled. This doesn’t mean code is riddled with exception handlers. They are aware of a lot of the failure cases, but still there’s a lot of defensive code.</span>

*   <span>**Building distributed systems is rewarding**</span><span>. It’s a beautiful thing to see a whole network of machines adjust to an injected fault and it all gets corrected.</span>

*   <span>**Design monitoring and debugging in from the start**</span><span>. Make everything visible from the beginning. One of the nice things about lock-free approaches is you can actually observe state from another thread, which makes it easier to debug.</span>

*   <span>**We are leaving 3x-4x performance on the table just because of configuration**</span><span>. The separation of programmers and systems admins is an anti-pattern. Developers don’t talk to the people who have root access on machines who don’t talk to the people that have network access. Which means systems are never configured right, which leads to lots of packet loss.</span> <span>Loss, throughput, and buffer size are all strongly related</span><span>. We need to workout how to bridge that gap, know what the parameters are, and how to fix them. So</span> <span>Know your OS network parameters and how to tune them</span><span>.</span>

*   <span>**Some parts of Java really suck**</span><span>. Unsigned types; the NIO system is riddled with locks; it’s difficult to work off-heap and get the required performance inside the sandbox model; string encoding shouldn’t require copying buffers three times; some people are grownups, let us map and unmap a memory mapped file and use sockets like a grownup; the garbage collection issues around hashmaps are painful; or’ing byte flags promotes the byte to an int so you can’t assign the result back to a byte;</span>

*   <span>**Nice parts of Java**</span><span>. Tooling. Java is the world’s worst language with the world’s best tool chain: IDEs, Gradle, HdrHistogram. Java’s lambdas and method handles are quite nice. Bytecode instrumentation is really useful for debugging. Unsafe in Java 8 is very nice, for example, that’s how counters are incremented lock-free and it scales well across CPUs and memory fences make it possible to build broadcast buffers. The optimiser is great. Garbage collection is nice for certain classes of algorithms.</span>

*   <span>**Averages mean absolutely nothing**</span><span>. Pay attention to percentiles.</span>

## <span>The Future</span><span style="font-size: 12px;"> </span>

*   <span>Now feature complete. Now into heavy profiling and optimization.</span>

*   <span>Things are looking very good. Already beating the best products out there on throughput. Can push small 40 byte messages at 6 million messages a second, which is a very difficult case. Latency is looking superb, up there with the best commercial products out there, up to the 90th percentile. There’s work to do beyond the 90th percentile, partially on their algorithms, but also on NIO, selectors, and locks.</span>

*   <span>C++ port coming next.</span>

*   <span>Infiniband and IPC.</span>

## <span>Related Articles</span><span style="font-size: 12px;"> </span>

*   [On Reddit](http://www.reddit.com/r/programming/comments/2ml0gb/aeron_do_we_really_need_another_messaging_system/) / [On Hacker News](https://news.ycombinator.com/item?id=8619735)

*   <span>[Aeron High-Performance Open Source Message Transport](http://gotocon.com/dl/goto-berlin-2014/slides/MartinThompson_AeronTheNextGenerationInOpenSourceHighPerformanceMessaging.pdf)</span>

*   [<span>"Aeron: Open-source high-performance messaging"</span> ](https://www.youtube.com/watch?v=tM4YskS94b0)<span>by Martin Thompson</span>

*   [<span>Aeron on GitHub</span>](https://github.com/real-logic/Aeron)

*   <span>Aeron</span> [<span>protocol specification</span>](https://github.com/real-logic/Aeron/wiki/Protocol-Specification) <span>and</span> [<span>design overview</span>](https://github.com/real-logic/Aeron/wiki/Design-Overview)<span>.</span>

*   [<span>The Log: What every software engineer should know about real-time data's unifying abstraction</span>](http://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying)

*   [<span>Google: Taming The Long Latency Tail - When More Machines Equals Worse Results</span>](http://highscalability.com/blog/2012/3/12/google-taming-the-long-latency-tail-when-more-machines-equal.html)

*   [<span>The Great Microservices Vs Monolithic Apps Twitter Melee</span>](http://highscalability.com/blog/2014/7/28/the-great-microservices-vs-monolithic-apps-twitter-melee.html)<span></span>

*   [<span>Simple Binary Encoding</span>](http://mechanical-sympathy.blogspot.com/)

*   [<span>Going to Strangeloop this week? Which talks are on your "must see" list?</span>](https://groups.google.com/forum/#!topic/mechanical-sympathy/fr7moLcsuiI)

*   [<span>Aeron perf testing</span>](https://groups.google.com/forum/#!topic/mechanical-sympathy/GrEce0gP7RU)

*   [<span>Strategy: Exploit Processor Affinity For High And Predictable Performance</span>](http://highscalability.com/blog/2012/3/29/strategy-exploit-processor-affinity-for-high-and-predictable.html)

*   [<span>12 Ways To Increase Throughput By 32X And Reduce Latency By  20X</span>](http://highscalability.com/blog/2012/5/2/12-ways-to-increase-throughput-by-32x-and-reduce-latency-by.html)

*   [<span>Busting 4 Modern Hardware Myths - Are Memory, HDDs, And SSDs Really Random Access?</span>](http://highscalability.com/blog/2013/6/13/busting-4-modern-hardware-myths-are-memory-hdds-and-ssds-rea.html)

*   [<span>Benchmarking Apache Kafka: 2 Million Writes Per Second (On Three Cheap Machines)</span>](http://engineering.linkedin.com/kafka/benchmarking-apache-kafka-2-million-writes-second-three-cheap-machines)

*   [<span>HdrHistogram</span>](https://github.com/HdrHistogram/HdrHistogram) <span>- A High Dynamic Range (HDR) Histogram.</span>

</div>