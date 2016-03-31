## [Paper: Immutability Changes Everything by Pat Helland](/blog/2015/1/26/paper-immutability-changes-everything-by-pat-helland.html)

    

    

![](https://farm8.staticflickr.com/7286/16185984428_7c4253e309_m.jpg)

I was excited to see that [Pat Helland](https://www.linkedin.com/profile/view?id=1302556) has published another thought provoking paper: [Immutability Changes Everything](http://www.cidrdb.org/cidr2015/Papers/CIDR15_Paper16.pdf). If video is more your style, Pat gave a wonderful talk on the same subject at RICON2012 ([video](http://vimeo.com/52831373), [slides](http://cloud.berkeley.edu/data/immutability.pptx)).

It's fun to see how Pat's thinking is evolving over time as he's worked at Tandem Computers (TransactionMonitoring Facility), Amazon, Microsoft (Microsoft Transaction Server and SQL Service Broker), and now Salesforce.

You might have enjoyed some of Pat's other visionary papers: [Life beyond Distributed Transactions: an Apostate’s Opinion](http://www-db.cs.wisc.edu/cidr/cidr2007/papers/cidr07p15.pdf), [The end of an architectural era: (it's time for a complete rewrite)](http://dl.acm.org/citation.cfm?id=1325851.1325981), and [Idempotence Is Not a Medical Condition](http://dl.acm.org/citation.cfm?id=2187821).

This new paper is a high level overview of why immutability, the idea that destructive updates are not allowed, is a huge architectural win and because of cheaper disk, RAM, and compute, it's now financially feasible to keep all the things. The key insight is that without data updates, coordination in a distributed system becomes a much simpler problem to solve. RedHat [is linking](http://developerblog.redhat.com/2015/01/20/microservice-principles-and-immutability-demonstrated-with-apache-spark-and-cassandra/) microservices and containers with immutability.

Immutability is an architectural concept that's been gaining steam on several fronts. Facebook is using a declarative [immutable programming model](https://www.youtube.com/watch?v=XhXC4SKOGfQ) in both the model and the view. We are seeing the idea of [immutable infrastructure](http://highops.com/insights/immutable-infrastructure-what-is-it/) rise in DevOps. [Aeron](http://highscalability.com/blog/2014/11/17/aeron-do-we-really-need-another-messaging-system.html) is a new messaging system that uses a persistent log to good advantage. The [Lambda Architecture](http://lambda-architecture.net/) makes use of immutability. [Datomic](http://www.datomic.com/overview.html) is a database data that treats data as a time-ordered series of immutable objects.

If that's of interest, then you'll like the paper.

**Overview:**

> There is an inexorable trend towards storing and sending immutable data. We need immutability to coordinate at a distance and we can afford immutability, as storage gets cheaper. This paper is simply an amuse-bouche on the repeated patterns of computing that leverage immutability. Climbing up and down the compute stack really does yield a sense of déjà vu all over again
> 
> It wasn’t that long ago that computation was expensive, disk storage was expensive, DRAM was expensive, but coordination with latches was cheap. Now, all these have changed using cheap computation (with many-core), cheap commodity disks, and cheap DRAM and SSD, while coordination with latches gets harder because latch latency loses lots of instruction opportunities. We can now afford to keep immutable copies of lots of data, and one payoff is reduced coordination challenges.
> 
> Designs are driving towards immutability. We need immutability to coordinate at ever increasing distances. We can afford immutability given room to store data for a long time. Versioning gives us a changing view of things while the underlying data is expressed with new contents bound to a unique identifier

## Related Articles

*   [Pat Helland's Weblog](http://blogs.msdn.com/b/pathelland/) while at Microsoft
*   [Normalization Is for Sissies](http://blogs.msdn.com/b/pathelland/archive/2007/07/23/normalization-is-for-sissies.aspx) by Pat Helland
*   [Memories, Guesses, and Apologies](http://blogs.msdn.com/b/pathelland/archive/2007/05/15/memories-guesses-and-apologies.aspx) by Pat Helland
*   [Idempotence Is Not a Medical Condition](http://dl.acm.org/citation.cfm?id=2187821) by Pat Helland
*   Video of [Immutability Changes Everything](http://vimeo.com/52831373) talk at RICON2012\. ([slides](http://cloud.berkeley.edu/data/immutability.pptx))
*   [Trip Report -- HPTS (High Performance Transaction Systems) Workshop -- Part 1 of the Trip Report](http://blogs.msdn.com/b/pathelland/archive/2009/11/02/trip-report-hpts-high-performance-transaction-systems-workshop-part-1-of-the-trip-report.aspx) by Pat Helland
*   [7 Design Patterns For Almost-Infinite Scalability](http://highscalability.com/blog/2010/12/16/7-design-patterns-for-almost-infinite-scalability.html)
*   [Scaling Secret #2: Denormalizing Your Way To Speed And Profit](http://highscalability.com/blog/2007/8/16/scaling-secret-2-denormalizing-your-way-to-speed-and-profit.html)
*   Comments on the paper [on Reddit](http://www.reddit.com/r/programming/comments/2tm9uu/immutability_changes_everything_pdf) and [on Hacker News](https://news.ycombinator.com/item?id=8955130)
*   [The Road to Akka Cluster and Beyond](http://www.slideshare.net/jboner/the-road-to-akka-cluster-and-beyond)
*   Get your [own bumper sticker](http://www.zazzle.com/immutability_changes_everything_bumper_sticker-128708485991573242)
*   [Why aren't all data immutable?](http://scale-out-blog.blogspot.com/2014/02/why-arent-all-data-immutable.html)
*   [If immutable objects are good, why do people keep creating mutable objects?](http://programmers.stackexchange.com/questions/151733/if-immutable-objects-are-good-why-do-people-keep-creating-mutable-objects)
*   [Log-structured merge-tree](http://en.wikipedia.org/wiki/Log-structured_merge-tree)
*   [irmin](http://openmirage.org/blog/introducing-irmin) - a distributed database that follows the same design principles as Git
*   "I am the Lord, I change not; therefore ye sons of Jacob are not consumed."—Malachi 3:6

    