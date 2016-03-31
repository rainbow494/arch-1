## [Zen and the Art of Scaling - A Koan and Epigram Approach](/blog/2012/2/27/zen-and-the-art-of-scaling-a-koan-and-epigram-approach.html)

    

    

![](http://farm8.staticflickr.com/7181/6786591856_031955cb53_o.jpg) _This is a guest post derived from an email conversation with [    Russell Sullivan    ](https://twitter.com/#!/jaksprats), a computer architect and creator of [Alchemy Database,](http://code.google.com/p/alchemydatabase/) A Hybrid RDBMS/NOSQL-Datastore._

Russell (AKA Jak Sprats) has been pondering, considering, and implementing distributed databases for many years. In a recent email conversation he shared 44 of the lessons he has learned from developing the infrastructure for high performance / highly scalable systems. Some are well known, some are debatable, and some obviously result from a deep experience that is worth learning from:

1.      There are maybe 20 classic bottlenecks (CPU, NIC overload, memory fragmentation, disk seeks, swap, thread deadlock, packet loss, etc.), have a basic understanding of them, because each is a dark tunnel, and you need a specialised flashlight for each.    
2.  The true style of scalable architecture is rooted in the nature of bottlenecks. I always talk about [commutative operations](http://muratbuffalo.blogspot.com/2012/02/cap-12-years-later-how-rules-have.html) on a sharded relational model, and I figure if that is in place, whatever is built on top of it must scale. So if you can figure out where the endpoints are, then make those scalable, and the rest will already be scalable. This is the concept as not boxing yourself in. It’s hard to put into words...when I code something and I get the feeling that it is clean, it always scales, when something is nagging, telling me, that it is not clean, it eventually bites me in the ass.
3.      The thing is, the key to the whole thing is that finest granularity of locking. Once you have a lock deep down in your system, your whole system is a locked system. This "lock" is not a thread-lock, it could be a cross-shard-join, it is anything that is not inherently scalable.    
4.      RDBMSes have their place. Structured data is superior in performance, but inferior in flexibility. If data makes sense being in a RDBMS, if it normalises well, then do it, but don’t get locked in, and always view your data as being in a normalised schema at the moment, but something that could also be modeled differently. Again the lock-in, avoid lock-in, it leads to bottlenecks.    
5.      I can't seem to express this well in words, but I always know it when I code it, something in my head goes: remember this approach, it ends up messing with you in the end.    
6.      Scaling is about compromises. Statelessness scales forever, but does nothing. Commutative operation on a sharded RDBMS schema scales forever, but is hard to model data in. Doing 20 table joins is convenient, but it can not be made to scale horizontally.    
7.      I don’t know how to word this, but in C code, you can hard code something and it feels wrong, so you make it a #ifdef, and it feels better, then you make it an enum, and its now OK, eventually, it evolves into a configuration parameter that can be set via REST or a signal. Gut feeling needs to tell you how to cut an app into hardcoded values and REST-changeable values.    
8.      The art of scaling is getting done what needs to be gotten done, while staying as close to statelessness as possible, without introducing unneeded complexity by doing so.    
9.      Scalability is all about loose coupling, about things happening, and effecting other things, but loose cause and effect. Computers have such speed that when not bottlenecking they are always undersaturated.    
10.      Programming is funny, when you are 22, you can program 20 hours straight, consistently, and as time goes on, this capacity wanes. when you are 30, you have programmed most algorithms at least once, and start appreciating how brilliant most advances in the field are. I'm 35 and I can still code for many hours on end, but my most productive days are ones where I reflect and solve things largely in my subconscious. I have enjoyed this evolution and look forward to where it leads, many programmers drop off, can't take the pain of concentration ... they lose.    
11.      The future of computing is getting loads of 3.5Ghz cores connected by a real fast bus and sharing a constant speed L3 cache and a variable speed NUMA RAM to do stuff. In the future there are bound to be some alternative permanent storage options, but they may just be considered slower RAM.    
12.      Identify the single point of failure. There will be multiple points of failure.    
13.      The decision on if you eventually need to horizontally scale data storage is key. Key-value and sharded data will scale almost infinitely, try to follow those lines when designing your system and try to design so if you need cross shard joins, you don’t need them very often.    
14.      Profiling, tracing, etc. should be built in from the beginning, use a finished package to do this.    
15.      The right tool for the right job, but don’t use 1000 tools.    
16.      Once you geo-distribute, the game changes radically, everything needs to be loosely coupled. Geo-distribution is often OK as a natural disaster level failover, i.e. a cold standby    
17.      Static content is your friend.    
18.      Client side logic is your friend. There are many many client side CPU tricks. This is an important part of scaling, pushing logic to where it is abundant, and not repeating anything.    
19.      Caching is an art best served by monitoring.    
20.      Hardware vendors lie, appliance vendors lie. SSDs are great, but will not save you. Vendor lock-in is the anti-scale.    
21.      Open source is good and bad.    
22.      The cloud scales great, except for disk I/O intensive apps. Its costs can be huge, so can its savings.    
23.      Hadoop scales, learn hadoop    
24.      Benchmarks lie. Isolated rollouts give you real numbers.    
25.      Know the hardware market. Some things are slowing down, some aren’t. Some are getting cheaper, some aren’t.    
26.      Async message queues (rabbitMQ, zeroMQ), when properly implemented, are brilliant.    
27.      Deploy 10GigE in the right places, possibly consider InfiniBand. Use multiple NICs.    
28.      Use lightweight async in-RAM data storage when possible.    
29.      When you need to go to disk, make it for large objects.    
30.      Correct partitioning. Never share a frontend and analytics DB, use memcached, offload whatever you can. Identify your firehose (your bottleneck). If it is the frontend it will come down to horizontal data storage. If it is backend it will be Hadoop and it will also come down to horizontal data storage.    
31.  Compression can be used to keep more data in RAM, to lessen the amount of disk I/O, to reduce network traffic, to delay or minimize the need for horizontal scaling ... and you probably have enough free CPU ticks for it.
32.  A good single node and single core's performance is the key to scaling. everything you scale from that point will be at best linear and more likely exponential.
33.  A bottleneck is usually skinny, i.e. most of the time it is not happening but in a few places, so fix those and you are good for a while. if your bottlenecks are happening in more than a few places, you're toast, time to rearchitect.
34.  Threads scale funnily, sometimes they scale good on a few cores, sometimes they dont, and they almost never scale well to many cores ... they are effective when applied to yield on I/O waits, but so are events.
35.  Event driven languages are difficult to program in. Event driven programming should used as glue putting together events. If you need to do complicated nested algorithms, do them at an endpoint, devoid of I/O.
36.  If all your databases are behind the same switch, you dont need to worry about CAP, but you have an obvious SPOF.
37.  The idea of using your own servers and then firing off cloud instances to handle traffic spikes is a real crafty way to use the cloud cost effectively, but in practice can be really complicated if you need to sync data to the cloud: you introduce a whole-other level of data synchronisation and security issues to contend with.
38.  If I spend 3 months on a project, my previous experience is my greatest asset, at about 9 months, I start to get a feel for the nature of the system and data I am working with, at 18 months, my finger is fully on the pulse, and after that the changes I make are precise solutions to real world problems. Intuition and rumination provide the most important breakthroughs in CS.
39.  Data visualisation is immensely helpful for me, because I am a visual thinker, this characteristic might be a real advantage for a programmer who deals w/ data.
40.  Linux gurus are freaks ... whom I love working with, it takes a lifetime to understand what is going on w/ the linux ecosystem and I am not gonna spend it, so it's great having someone around who has, so I can badger them w/ questions.
41.  Most of the scaling stories you hear about seem to go: blah blah blah, lots of work & changes, took years, we survived. So all this back and forth we are doing on the subject is helping raise awareness. Yay Us.
42.  People being honest on the shortcomings of their platforms is just as helpful (and much more believable) than people hyping up their platforms.
43.  Long and short running queries can not exist in peace. Try to segregate your workloads. If you cant segregate, think of relaxing ACID for the long running queries (i.e. batch them).
44.      Scaling is a black black art.    

    