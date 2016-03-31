## [Building Globally Distributed, Mission Critical Applications: Lessons From the Trenches Part 1](/blog/2015/8/31/building-globally-distributed-mission-critical-applications.html)

    

    

![](https://farm1.staticflickr.com/673/21015670451_fb1075527a_m.jpg)

_This is Part 1 of a guest post by [Kris Beevers](https://www.linkedin.com/pub/kris-beevers/4/a5/113), founder and CEO, [NSONE](https://nsone.net/), a purveyor of a next-gen intelligent DNS and traffic management platform. Here's [Part 2](http://highscalability.com/blog/2015/9/2/building-globally-distributed-mission-critical-applications.html)._

Every tech company thinks about it: the unavoidable – in fact, enviable – challenge of scaling its applications and systems as the business grows. **How can you think about scaling from the beginning, and put your company on good footing, without optimizing prematurely?** What are some of the key challenges worth thinking about now, before they bite you later on? When you’re building mission critical technology, these are fundamental questions. And when you’re building a distributed infrastructure, whether for reliability or performance or both, they’re hard questions to answer.

    Putting the right architecture and processes in place will enable your systems and company to withstand the common hiccups distributed, high traffic applications face. This enables you to stay ahead of scaling constraints, manage inevitable network and system failures, stay calm and debug production issues in real-time, and grow your company and product successfully.    

##     Who is this guy?    

    I’ve been building globally distributed, large scale applications for a long time.  Way back in the first dot-com boom, I bailed on college classes for a year and built backend infrastructure for a file-sharing startup which grew to millions of users – until the RIAA’s lawyers caught wind and sent us packing back to our dorm rooms. The business went bust, but I was hooked on scale.    

    More recently, at Voxel, an internet infrastructure provider that was acquired by Internap in 2011, I built global internet infrastructure used by many large web companies – we built globally distributed public cloud, bare metal as-a-service, content delivery networks, and much more. We learned a lot of scaling lessons, and we learned them the hard way.    

    Now, at NSONE, we’ve built a next-gen intelligent DNS and traffic management platform, which today services some of the largest properties on the Internet, including many companies who are themselves mission critical service providers.  This is truly globally distributed, mission critical infrastructure, and the lessons we learned at Voxel have served us well – and been reinforced time and again – as we’ve built and scaled the NSONE platform.    

    It’s time to share some of what we’ve learned, and with luck, maybe you can apply some of these lessons in your own applications – instead of learning them the hard way!    

##     Architecture first    

    When you’re writing code, it’s always tempting to focus on making it tight and super efficient, minimizing memory and CPU consumption, maximizing the “depth” of your software. Stop it.    

In a modern, heavily distributed system, your scaling constraints are hardly driven by the vertical “depth” of your systems and optimization of code within specific roles – especially early on. Instead, they’re about managing the communication between interacting systems, and the horizontal scalability of each role in the architecture. **Don’t optimize your code, optimize your architecture.**

[    Microservices    ](https://en.wikipedia.org/wiki/Microservices) are a good idea. They decouple independent roles in your application and enable you to scale each role or layer independently. **Plan to scale roles horizontally before you worry about depth. Servers are fast and cheap, and they’re always getting faster and cheaper. Developer time isn’t.** So enable yourself to scale horizontally whenever you can, and worry about code optimization only when you find the places it’s really going to matter.

    Take the time to think about your architecture before you write a line of code.  Draw diagrams, think about communication and data constraints, and have a plan.    

##     Think about your systems in terms of roles    

A microservices architecture more or less implies that you’re building an application composed of decoupled systems, each addressing a specific problem or acting in a particular role. But it bears repeating that you should think about your architecture this way. Not just b**ecause it decouples scaling constraints, but because it impacts the way you develop your code, deploy your systems, and grow your infrastructure**.

    An application composed of decoupled, inter-communicating roles can live on a single server as collocated processes, VMs, or containers. This enables you to deploy your application in local development environments, small staging systems, and large production infrastructure.    

**As you grow your application’s production infrastructure and scale with traffic and users, you can peel off roles that need dedicated systems or clusters of systems.**  But in your MVP, you can minimize costs and still be prepared to scale by collocating production roles. Our earliest alpha releases at NSONE were hosted on a single unicasted t2.small instance in AWS. Today, with essentially the same set of roles and subsystems with which we started (and many more we’ve added since), our production infrastructure spans hundreds of servers and nearly 30 different production facilities across every continent on the globe (except Antarctica – sorry guys). As we grew, we peeled key subsystems off to their own servers or clusters and added breadth and redundancy where we needed it.

##     Communication across a globally distributed system is hard    

    It’s one thing to build and scale an application composed of subsystems collocated initially on a single server, and eventually, in a single datacenter. It’s another thing entirely to solve the challenge of multi-datacenter application delivery. The moment your subsystems need to communicate outside the LAN, the reliability of that communication is drastically diminished and you need to plan carefully not just for server failures, but for communication failures.    

    Connectivity over the internet is fragile and unpredictable. At NSONE, we designed our architecture with the assumption that our edge DNS delivery facilities will lose connectivity with our core facilities frequently. This is especially the case in difficult markets like Africa, Brazil, and India. But we also needed to ensure rapid re-convergence when communication is restored. Careful consideration of command and control messaging and message queuing is key.    

It’s also important to think about inter-facility communication differently for different communication patterns. **If you’re pushing latency sensitive critical command and control messages to one or more facilities, you’ll probably want to look at robust message queueing systems** (for example, we make heavy use of RabbitMQ). If you’re shipping non-critical telemetry back to a dashboarding system, you might **consider something lighter weight but less robust to network splits**. If you’re pushing persistent application data that needs to be closely synchronized across all your delivery locations, **modern databases with robust WAN replication are worth a look**. Use the right tool for the right job when it comes to moving bits between facilities.

##     Consistent hashing is a killer tool for synchronization & sharding    

    It’s time to think about horizontal scaling. You’ve got a bunch of subsystems all communicating with each other both locally and across the Internet. Requests are flying into your systems and need to get distributed across backend nodes that can service them effectively, and you need a way to maximize “request locality”. All your servers in a particular role can’t be expected to service any request, maybe because the requisite data can’t all fit in RAM on a single box, or you need to stripe storage across servers effectively. How do you figure out which node in a horizontal layer should handle a request or task?    

    Naively, you could preconfigure your request routing: maybe you’ve got a request id, and all request ids starting with A-M get striped to one server, and starting with N-Z to another server. That might work, but is painful to manage and scale.  You could hash the request id to a server: something like         hash(id) mod N        , where N is the number of servers you’re striping across. That works well, until you add a new server and all your requests re-stripe, destroying data or cache locality and crushing your systems. You could implement some kind of complicated negotiation for distributed “agreement” – your various subsystems and servers communicating with each other to decide which requests should go where – and this is an approach that I’ve seen too often. It’s complex, and things will break.    

[    Consistent hashing    ](https://en.wikipedia.org/wiki/Consistent_hashing)     is a powerful way for distributed systems to agree on something like a mapping of requests to nodes, without needing to actually communicate. It’s easy to implement, and it scales gracefully with your infrastructure, minimizing re-striping as you add or remove nodes in a subsystem. In a complex architecture with many interacting roles, strategic use of consistent hashing to stripe messages and requests across sets of nodes can be a huge win.    

At NSONE, **we use consistent hashing across our stack in a host of ways, like routing DNS queries to specific CPU cores to maximize cache locality, negotiating tasking of our monitoring nodes without explicit communication, sharding of high volume data feeds across layers of aggregation nodes, and much more**.

##     Measure and monitor everything    

This is such a common piece of advice it almost seems cliché, but it always bears repeating. **What you don’t measure, you can’t understand**. Even when you’re far from encountering serious scaling challenges, building a deep understanding of the behavior of your systems and their interactions is critical to your knowledge of the scaling constraints you’ll face as you grow.

Don’t just gather system metrics. **Instrument your application to understand database response times, messaging delays, cache hit ratios, memory fragmentation, disk I/O, anything that might get you insight into the performance of your systems.** Graph it all, and build a guttural understanding of “nominal” behavior. When something exceptional happens – a load spike, a communication breakdown, some kind of anomaly in a subsystem – go back and review your metrics and understand the “signature” of the event. This will help you recognize what’s happening (and just as importantly, rule out what’s not) in anomalous situations in the future. It will help you understand when a new kind of workload or additional traffic is putting unexpected stress on your systems, so you can adjust your architecture or scale out subsystems appropriately – before they fail.

At NSONE, **we study our metrics.** They’re always on display (our offices are littered with monitors displaying NOC dashboards). When we see something out of whack, we ask why and dig deeper. The deep understanding we get from this approach means we can confidently service the DNS and traffic management needs of the largest customers on the internet, because we know how our platform will react to new traffic and workloads.

**Just as important as measuring is monitoring**. These are two different things: we measure to understand the behavior of our systems, and we monitor to detect anomalies in that behavior. Monitor and alert promiscuously early in your growth: it will help you understand anomalies that matter and ones that don’t. You’re just getting started, and you can miss out on some sleep. Later on, as the business grows and managing your systems becomes more of a marathon than a sprint, you’ll want to reduce the noise, focusing on anomalies that impact service and automating and re-architecting your systems to minimize them.

**Monitor from multiple vantage points** – not just physically (multiple geographies and networks), but logically. At NSONE, we monitor internal telemetry for anomalies, leverage third party services like Catchpoint and Raintank for outside-in monitoring, and generally draw on a wide array of data sources to observe platform and network events.

Just as importantly, **monitor more than just “upness” of systems**. What does it mean for a system to be providing “good” service?  Low latency response times?  High cache hit rates?  Monitor those metrics and alert when they fall out of bounds.

_Here's [Part 2](http://highscalability.com/blog/2015/9/2/building-globally-distributed-mission-critical-applications.html) of the article._

## Related Articles

*   [On Hacker News](https://news.ycombinator.com/item?id=10147420)

    