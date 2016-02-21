## [How We Scale VividCortex's Backend Systems](/blog/2015/3/30/how-we-scale-vividcortexs-backend-systems.html)

    

    

![](https://farm8.staticflickr.com/7585/16354200273_9d2bdc7058_o.png)

_This is guest post by [Baron Schwartz](https://twitter.com/xaprb), Founder & CEO of [VividCortex](https://vividcortex.com/about-us/), the first unified suite of performance management tools specifically designed for today's large-scale, polyglot persistence tier._ 

VividCortex is a cloud-hosted SaaS platform for database performance management. Our customers install agents that measure the work their servers perform (queries, processes, etc) and generate metrics and events from that at high frequency. The agents send the resulting data to our APIs, where we host our analysis backend. The backend system is a collection of databases, internal services (quasi-microservices), and web-facing APIs. These APIs also power our AngularJS frontend application.

We deal with a lot of data. We ingest metrics and events at high speed. We also perform analytics that touch large amounts of data interactively. We are not unique and I don't want to imply we are somehow impressive in the scheme of things. We don't yet operate at "web scale." Nevertheless, our workload has some relatively unusual characteristics, and we've been able to scale as far as we have, while remaining pretty efficient in terms of cost and infrastructure. And my career in consulting has taught me that building systems like this is usually a challenge for a company (as it has been for us). Our story might be useful to others. For that reason I will go into unnecessary detail on specific parts of our workload and the challenges it brings.

## What We Do

VividCortex is a sophisticated system with a lot of functionality, but Top Queries (and the data we capture to enable it) is easily the most important feature that impacts our backend significantly. Top Queries is a table of query categories, ranked by a selected dimension in descending order, with a catchall row that shows categories that didn't fit into the top N.

[![top-queries](https://camo.githubusercontent.com/2f0d8f8246190511ec9d7a5b99ed48b3d939fdc1/68747470733a2f2f636c6f75642e67697468756275736572636f6e74656e742e636f6d2f6173736574732f3237393837352f363838393234382f38353663663566612d643636302d313165342d383565312d6131393330653962636139662e706e67)](https://camo.githubusercontent.com/2f0d8f8246190511ec9d7a5b99ed48b3d939fdc1/68747470733a2f2f636c6f75642e67697468756275736572636f6e74656e742e636f6d2f6173736574732f3237393837352f363838393234382f38353663663566612d643636302d313165342d383565312d6131393330653962636139662e706e67)

The screenshot (from our own workload, by the way) shows this feature, stripped down to its essentials. Top Queries is actually a specific example of a general class of task our app can achieve: you can rank, sort, filter, slice and dice measurements from queries, users, processes, databases, and so on and so forth. You can do this across one or many hosts simultaneously, so you can get a global view and then drill down to specific hosts or groups of hosts. And you can choose from many different dimensions to do this. All of these essentially take categories of metrics and rank them by the dimension, keep the first N, and collapse the rest into the catchall row at the end.

There's a lot of other stuff we do as well, but most of that is not very different in its backend implementation from what a lot of metrics-and-monitoring systems do. And in comparison with Top Queries, it's not a significant engineering challenge.

## Timeseries Data

Our timeseries metrics are timestamped float64 values that belong to series. The series are identified by the metric name and the host where they were measured. Timestamps are currently uint32 unix timestamps in UTC.

Metric names are dot-separated strings. We gather a lot of metrics from a lot of sources, all at 1-second resolution.

*   Every database status metric. For MySQL, for example, this includes several hundred counters from SHOW STATUS, as well as hundreds more from other sources.
*   Operating system metrics for each CPU, each network device and each block/disk device, memory, and so on.
*   Process-level metrics from the OS, including each process's CPU, memory, IO, open filehandles, network sockets, and so on.

But most significantly, we sniff network traffic and decode the wire protocol for popular database products. Currently we offer GA support for MySQL and PostgreSQL, with Redis and MongoDB in beta and more on the way. We measure inbound queries (or requests/commands/etc as the case may be; varies by product) and time the response latency, as well as extracting a large variety of useful details such as errors and the like. In cases where we can't sniff the network, we can use built-in instrumentation, such as MySQL's `PERFORMANCE_SCHEMA` and PostgreSQL's `pg_stat_statements` views. That lets us support deployment scenarios such as Amazon RDS.

> As an aside, capturing this data presents interesting problems itself, and we have blogged about that before (two examples: [netlink](https://vividcortex.com/blog/2014/09/22/using-netlink-to-optimize-socket-statistics/), [perf schema](https://vividcortex.com/blog/2014/11/03/mysql-query-performance-statistics-in-the-performance-schema/)). Go is a big asset to us here.

The most obvious challenge is probably the sheer number of series we collect. We classify similar queries together by digesting out variable parts, then taking an md5sum of the abstracted query. We hex-encode this (a silly decision I regret and take responsibility for personally) and embed the result into the metric name, e.g. `host.queries.c.1374c6821ead6f47.tput`. The last component is the dimension--in this case, throughput (rate per second), but it could be one of many things such as latency, error rates, number of rows affected, and the like. The variable parts in this particular metric name are the `c`, which indicates what kind of operation this was (query, prepared statement execution, etc) and the `1374c6821ead6f47` which is the checksum of the digested query.

As you can likely guess, the variable portions of the metric names can be high-cardinality. Some of our customers generate tens of millions of distinct types of queries, which result in a combinatorial explosion of time series. In the worst case it's a cross product of every variable portion of the metric name, which commonly includes 2, 3, 4 or sometimes more variable parts.

We also generate and store a lot of other data. Descriptive metrics about queries and the like isn't enough; we augment that with individual samples of the query events themselves and a variety of other kinds of things, such as events raised in response to system faults or configuration changes. Scaling these has not proven to be as difficult as the timeseries metrics, primarily because the read workloads for those are not unusually hard to scale.

## Write Workload

The largest part of our backend's write workload, by raw number of facts stored, is the timeseries metrics. The query samples and events constitute a lot more data on disk, but that's because it's a lot less compact than the timeseries data. Lots of small data can be harder to handle than a medium amount of big data.

Students of database internals and storage engines will understand that read-optimized and write-optimized storage are in conflict in some ways. If you want to have read-optimized data, you may have to do extra work at write time. We absolutely need some read optimizations, as you'll see later.

The write characteristics are fairly straightforward: data arrives pretty much in timestamp order, and the main challenge is just persisting it fast enough, along with the amplification needed for read optimizations. As you'll see, we do a variety of things to sidestep this.

We store 1-second-resolution data for 3 days. We also downsample to 1-minute resolution and keep that longer. Both retention limits are negotiable in the customer contract. In our own systems we keep 10 days of 1-second resolution.

## Read Workload

We have two primary read workloads for our timeseries metrics. They reveal one of the universal truths about databases: many different kinds of data can typically fit into a database, even a special-purpose one. Bits of data that live next to each other may be important for completely different purposes, and involved in the answers to completely different questions. This is something to beware of.

Workload #1, which I'll call range reads, is a read of one or more individually specified metrics along a time range at a desired resolution. The obvious example is rendering a timeseries chart with a single metric: 600px wide, last week's worth of data; give me metric X from host Y between timestamps 1426964208 and 1427569008 at 600 intervals of 1008 seconds. The read path isn't terribly complex, in principle: figure out where the data lives and what resolution is appropriate for it (in this case, 1-minute data is OK to use), find the beginning, and read points until the end. Resample as needed and stream it back to the client.

In reality there are lots of nuances, but they don't fundamentally change what I just described. Nuances include multiple metrics (sometimes hundreds or so), knowing what ranges of time have been downsampled completely, adjusting the sampling parameters, reading ranges of data concurrently from many sources (including replicas which may be delayed), and so on. None of these is a big deal.

As long as the data is read-optimized, there is nothing particularly hard about the range read workload. It generally is not a huge amount of data, even over long time ranges and multiple metrics. We do not find it difficult to respond rapidly to lots of requests for these kinds of reads. I'll talk later about how we make sure the data is read-optimized.

Workload #2 is harder. This is the Top Queries use case, and I'll call it bulk reads. It looks like this: read a very large set of not-individually-specified metrics from timestamp A to B and sum them up, sort them, return the top N, and include data needed to compute a catch-all row. Followup data, which is needed to show the sparklines for each of the results in the top N for example, can be fetched via the means explained for range reads. The Top Queries workload also includes the same considerations such as using coarser granularity where available, reading concurrently, and so on.

What's hard about the bulk read workload is that it touches a lot of metrics--potentially tens of millions or more. It's fast to read individual metrics--fast enough that we can render pages full of charts way faster than our customers are used to. (We often get comments about how fast our charts are.) But tens of millions of them? Even if it's a microsecond per metric, which it isn't, we're already talking about tens of seconds. The data that we touch with such bulk reads is also quite large. This is when "dung gets nonfictional," to paraphrase.

Notice also that although these metrics are subject to bulk reads, they're also subject to individual range reads. It isn't either-or, so we can't make simple optimizations such as storing metrics that belong to categories separately from metrics that are always looked up by a fully qualified metric name. We could duplicate them, but that comes with its own nontrivial costs.

Finally, some of the challenging characteristics of the bulk reads might seem almost incidental. The catchall row, for example, doesn't seem like a big deal, but it is--both for the user interface, where it's vital to provide contextual information about the application workload the user is analyzing, as well as for our backend, where it can represent huge amounts of work. Notice in the screenshot we're showing 159 other queries grouped together into the catchall row. That number could be as large as many millions.

## Data Distribution Characteristics

In addition to the interesting read characteristics presented by the bulk read workload, the data itself has some unusual properties, particularly in the distribution and density of the values.

Most monitoring systems and timeseries databases are built for dense metrics such as CPU or memory utilization. A dense metric belongs to something whose existence you can count on, and you will always get a value for it when you measure it. Most use cases for monitoring data implicitly assume that all metrics are dense. This explains the design of RRDTool, Graphite's Whisper backend, and so on.

Modern application architectures already present significant challenges to these kinds of timeseries databases, because everything is becoming highly dynamic and ephemeral. At its current extreme, we have things like Docker and microservices, and eventually we'll have lots of systems that operate like Amazon's Lambda, where the existence of a compute node itself is highly ephemeral. The challenges of monitoring in this kind of environment are largely un-grappled-with at the moment. I don't think there's a popular timeseries database that can handle it.

But at VividCortex, highly sparse and dynamic data is not a theoretical or emerging use case, it's our present reality. Queries, processes, and all the rest of the diverse and variable activity we monitor, generates monitoring data that has those characteristics. If there's one thing we're dealing with that's rather unusual, this is likely it. (Again, I don't claim uniqueness, just unusuality).

It's also a matter of degree. We're not unique in _having_ this kind of data, but we probably get hit harder with it than other companies/products that seem similar. That's partially because most products that measure things like queries do so from the application's point of view. Applications code is finite and largely not ad-hoc, which generates a limited number of distinct types of queries and other things to measure.

We have agents sitting on servers, however, and we're seeing the server's entire workload, not just that which comes from an instrumented application. This distinction might not seem like a big deal until you see how much workload typical servers get from sources that aren't monitored. This includes human and other ad-hoc activity, monitoring systems themselves, backend apps, cron jobs, and so on. The typical APM tool that just measures app-generated queries actually has significant blinders on.

Regardless, the bulk of our series (by cardinality, that is) are sparse, sometimes only registering points at intervals as widely separated as days or weeks.

Because this constitutes most of the series but only a small fraction of the data, any kind of system that dedicates a block/page/extent to a series, assuming it would be filled later, wouldn't work. It'd be filled with mostly empty space.

## Read or Write Optimization?

There's a huge, existential, life-or-death question to answer with storage and retrieval of the kind of data we've been discussing: read-optimized or write-optimized? You could easily build a career out of studying the tradeoffs in that.

If it's read-optimized, there's a significant write-scaling challenge to address. If it's write-optimized, there will be read challenges. Let's see why.

If we consider the host and metric to really constitute the series ID together, then the question is equivalent to asking whether we want to store the data timestamp-sorted or series-sorted.

For (conceptually) append-only immutable storage like our timeseries data, which we'll read in timestamp order (right?), write-optimization seems ideal at first, because the data will be stored in timestamp order automatically. Just append points to the end of the data:

      v host  metric          ts  value
      |    1  a.b.c.d 1426964208    123
      |    1  d.e.f.g 1426964208  87654
      t    2  a.b.c.d 1426964208  19283
      i    2  d.e.f.g 1426964208    183
      m    3  a.b.c.d 1426964208   9876
      e    4  l.m.n.o 1426964208  72736
      |    5  a.b.c.d 1426964208  17364
      |    1  a.b.c.d 1426964209    129
      |    1  d.e.f.g 1426964209  87656
      v    5  q.q.q.q 1426964209   9988

The problem with this is that data we want to read together isn't clustered together. If I want to read host 1, metric `a.b.c.d` from a given timestamp to another timestamp, I'm going to scan through _lots_ of unwanted data to do it. With the sparseness characteristics I described above, you can easily calculate that I'm likely to read several orders of magnitude more data than I care about. This is _read amplification._

Naturally, indexing is the solution for this. This is what indexes were invented to do.

But indexing duplicates data and writes, and introduces random I/O for writes. B-tree index performance and overhead also degrades significantly as the working set exceeds memory, and our data is _much_ bigger than memory. The new generation of indexes, such as LSM trees, doesn't solve all of these problems. No matter how we look at it, we're not going to get read optimization and write optimization for free. There's bound to be some read, write, and/or space amplification.

Given that purely write-optimized storage isn't going to work, the question of the right tradeoff is paramount to answer. How do you read-optimize the data without suffering too badly on writes? What is the right organization of the data for read-optimization, in other words?

I've partially given away an early version of our answer, by showing the schema of the timeseries table in the above example. We define a clustered primary key on our data, in `(host, metric, timestamp)` order. So the points are grouped together by host and metric, and within that, they're ordered by timestamp. This optimizes the data for the range read workload of an individually identified set of series, over a range of time.

This clustering does nothing, however, for the bulk read workload. For that workload, we're reading large amounts of data over a range of timestamps, and as I mentioned, these series constitute most of the data. So, the most efficient thing to do might be just to scan the whole range, discarding data that isn't of interest. A secondary index, timestamp-first, might be the trick. We went down this path, and it turned out that due to the diversity of metrics we're capturing, the math doesn't work out here either. Because we capture many different groups of bulk metrics, but only look at one group of them at a time, we're still examining a minority of the data, and we'd still be amplifying our reads greatly.

In our earlier days, as we continued to add more and more data to the set that we captured, this problem just got worse. Recall that this is our customers' most valuable use case. That makes it a very important problem to solve. I'll return to this problem and explain our solution to it later. For now I want to explore more of our architecture and set some context for various other interesting things.

## Sharding and Partitioning

We have a multi-tenant, sharded storage architecture, as you'd expect. Sharding helps address several needs for us: write scaling, read scaling, data isolation and tenancy, and security.

Write scaling, in general, can be achieved only by sharding. In fact, this is really the only reason to shard in the big picture. Read scaling can be addressed nicely by "horizontal scale-out" with replication, which makes more read capacity available.

Data isolation is important for us. Sometimes SaaS vendors demonstrate things like realtime analysis across all of their customers. That capability implies a level of commingling and lack of isolation that I find a bit unnerving. We're striving to safeguard against the possibility of someone getting broad-based, unfettered access to data they're not supposed to see. On the balance of security versus convenience, we tend to swing the needle away from convenience. (We also [encrypt sensitive data](https://vividcortex.com/blog/2014/11/11/encrypting-data-in-mysql-with-go/) such as samples of SQL end-to-end, in flight and at rest. [I will speak about that at the upcoming Percona Live MySQL conference](https://www.percona.com/live/mysql-conference-2015/sessions/encrypting-data-mysql-go).)

To those ends, our unit of sharding is the customer's "environment" -- think of staging, production, and the like. Hosts belong to one and only one environment, and all API access is segregated to a single environment. API tokens are defined to have access to only one environment, so at both a logical level and a physical level, each environment is isolated from all other environments. Sometimes we talk about resharding by environment and host, but thus far it hasn't been a problem to handle even our largest customers' environments.

Sharding is really partitioning, and I've just described partitioning by environment. We also partition our data by time ranges, for several reasons. One is to enable easy purge of expired data. Deleting data is far too costly and in many systems would at least double the system's write workload, if not more; unlinking files is much better. It's essentially free in the scheme of things.

Partitioning by time is one of the ways we address the bulk-read workload requirements. Partitions by time are essentially a coarse-grained, low-overhead, timestamp-first index. At first we partitioned a day at a time; now we partition an hour at a time for our 1-second data and a day at a time for the 1-minute data.

The way we originally did partitioning was another of the mistakes I'll claim personal responsibility for. In the beginning, we used native MySQL partitioning. This has some benefits, which I thought would outweigh any downsides:

*   MySQL natively prunes partitions that don't contain requested timestamps
*   Partitioned tables hide complexity from your app code
*   MySQL 5.6 added a number of nice features to partitioning, such as the ability to pull out a partition and manipulate it separately, then swap it back in (EXCHANGE PARTITION)

I also thought that, with the improvements in MySQL 5.6, we wouldn't run into the problems I had seen with earlier versions of MySQL partitioning. I was right, we didn't. We ran into others instead! Oops. Partition maintenance jobs (to drop old partitions and create new ones to hold future data) would block on long-running queries, bringing everything to a halt and running the server out of connections. Schema changes, which we had to do several times while figuring out that the bulk-read workload wasn't working and trying to fix it, ran into similar problems. Schema changes also consumed a ton of time and could never be a fully lights-out operation due to metadata locking and the like. This was all boring predictable stuff that only happened because I didn't think about it deeply enough.

So we basically ran into "death by ALTER TABLE" and "death by partition maintenance" a few times, and then we switched to do what we should have from the beginning. We created separate tables per time range, and used the table name to encode the schema version, data resolution, and time range. A sample table name with schema version 1 for 1-second data is `observation_1_s_1424444400_1424448000`. (We call our timeseries metrics "observations" because naming is hard, and I couldn't think of "points" at the time.)

New schema versions just get written into new tables, and when the old data is purged from all environments, we can eventually drop the code that's designed to handle that schema version. We don't do ALTER anymore. Old data will live on in infamy for a while.

This is similar to how I saw things done at New Relic, and I think there are earlier posts on this very blog that discuss this (and I used to consult at New Relic too), but I thought the improvements in partitioning had rendered it unnecessary for us. Unfortunately, this is one of the downsides of being a consultant. You think you know things about systems, but you don't know jack, because you've only been near the system--you haven't operated it yourself. Live and learn.

## Compact Storage

Our schema doesn't really look like what I've shown before. If you think about a schema like the following pseudo-code, and the data it contains, you'll notice massive inefficiencies:

    observation (
      host number
      metric string
      ts number
      value number
      primary key(host, metric, ts)
    )

There's going to be a lot of duplication of hosts, to begin with. Next you'll have metrics, which you'll also duplicate like crazy. Timestamps will also be mostly similar. And most timeseries values are just zeroes.

Finally, and most importantly, you'll have one row per point. This is going to be a crazy amount of what is essentially row headers with a little bitty value appended at the end.

To address this and store our data more compactly, we do several things:

*   We use host and metric IDs, not text strings like hostnames and metric names.
*   We batch values together in vectors, so it's not one row per second, but one row per longer time range (currently per minute, but could change in the future). This amortizes the cost of the keys in the row and is a technique I've used for many years. It does make some things less convenient, but it's worth it because it decreases the number of rows by 60x.
*   We don't always store exact values. We use a custom compression algorithm that takes advantage of the characteristics of the timeseries data we see. This reduces our values' storage space even further. Some high-dynamic-range metrics can have a small fraction of a percentage of imprecision occasionally, but nobody cares.
*   We store deltas for most metrics, which are usually small and easy to compress. Nobody cares about the actual value of a counter like the number of queries. It resets and restarts anyway. People care about rates for this type of data, not values. We generate rates by computing deltas over time ranges (such as per-second). Some kinds of data we compute as both values and rates, but that's a small minority.
*   We don't store zeroes. A "heartbeat" metric, such as a system timestamp or uptime counter, is enough to figure out whether we measured a batch of values at a moment in time, so we can tell the difference between zeroes and missing values. This helps with slowly-changing metrics, of which there are a lot.
*   We compute various summary metrics, bitmaps, and the like, and store them along with the vectors. This is useful for operations that don't need to access every point, such as the bulk reads; it also permits us to do things like downsampling without making stupid math mistakes such as the "average of averages" problem.

## APIs and Services

Our external and internal APIs are largely JSON over HTTPS. Some internal services use protocol buffers instead of JSON, but those additional dependencies and schema definitions are things we add only as needed, and after we think things have settled enough that we're not going to burn extra programmer time redoing stuff that we didn't get right.

We initially had a single monolithic API process that handled everything. This, of course, was hardly more than a prototype, and we moved away from it quickly. Monolithic APIs suffer from lots of problems, such as being a single point of failure.

Some general good API practices I'd like to suggest;

*   Think a lot about API design. Don't design APIs for yourself, design them for someone who might want to create a hypothetical application to do something very different from your own. See the [Interagent guidelines](https://github.com/interagent/http-api-design).
*   Separate out your read and write paths. Read and write workloads are often very different. For a service such as ours, it's of paramount importance that we not lose incoming data. Long-running, heavy requests that hit some edge case can easily cause a situation where bad reads impact writes and you lose data as a result. Use separate processes, and perhaps even separate servers, for reads and writes.
*   Build 12-factor apps. I didn't understand or agree with the 12-factor principles at first, especially the logging, daemonizing, and configuration guidelines. I have repented my sins.
*   Use a "clean" architecture, with layers like an onion. Refuse to allow anything from an interior layer to know about, much less reach out and interact with, something outside of itself. This means an internal job or service should never interact with your public endpoints. Apply the same principle to your code: dependencies in code should only point inwards, never outwards.

What we've moved to over time is a set of external APIs that are quasi-microservice, with (usually small) groups of related endpoints living in a single binary and process, but separated by reads and writes. We reuse some code by building both read and write paths in some codebases, but activating only one set via a) configuration that enables exactly one of reads or writes and b) proxy rules.

When such a service becomes something we want to build upon internally, we do a pushdown refactor, taking the guts of this API and making it available as an internal service. The external service then delegates most of its work to this internal service, which we can also use elsewhere internally.

We also treat special kinds of data and workloads on a case-by-case basis. We have separate sets of APIs and internal services specifically for timeseries data ingest, to make sure that we can quickly and reliably persist the data and ack the write back to the agent or other API client that sent the data. These APIs are fairly thin frontends: they send the writes to Kafka and then they're done.

Kafka consumers then stream the data back out of Kafka and persist it, mostly into MySQL. There is a small amount of Redis as well.

Kafka is great. I wish it didn't use Zookeeper, and there have been some smashed thumbs with partition management and failovers, but in general it is simple and reliable. Far more important, though, is the architectural pattern it enables and encourages. If you haven't studied [Jay Kreps's blog post on the log-based architecture](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying), I encourage you to do so. Especially if you don't feel like your message queue/bus is making your overall architecture much simpler and saner, you should consider the ideas in that article, independently of Kafka itself.

We really appreciate the Go Sarama client for Kafka. Many thanks to the Shopify team and others for writing it. Without it, Kafka wouldn't be a usable solution for us.

We have found and solved tons of issues with VividCortex by using VividCortex. We use staging to monitor production. I highly recommend this pattern. Production should never monitor itself, because then if there's an issue you don't have access to the data to diagnose it.

We also rely on a number of external services, including GitHub, HipChat, Dyn, Pingdom, CircleCI, VictorOps, BugSnag, and lots more that I will probably feel bad about omitting. Two of the most crucial are GitHub and CircleCI. We also have an almost completely automated test, build, and deploy system, which we control through our Hubot chatbot (chatops). It interacts with various services, including custom Go internal services, Jenkins, and Ansible. New code immediately gets tested by CircleCI--did I mention how amazing they are--and if it passes, gets deployed either to development or staging, depending on whether it's in a branch.

We created a system we call "Pooroku", after "poor man's Heroku," to push everyone's dev branches to appropriately named subdomains of our dev servers. Merges to master may auto-deploy to production, depending on the criticality of the code in question and the potential consequences. If not auto-deployed to production, a quick `/deploy foo to prod` takes care of it.

All of this happens in our chat channel, so everyone sees exactly what's going on and learns how to do it. If you're not familiar with the benefits of ChatOps I recommend [Owen's blog post](https://vividcortex.com/blog/2014/06/02/chatops-at-vividcortex/) on it. We deploy code dozens of times a day with the awesome systems Owen and others have built. We also do tons of other stuff. Until I experienced ChatOps and continuous delivery, I had no idea what I was missing.

## Using Go

We were Go users from the start, and we're proud to support and contribute to the Go community in various ways. All of our external and internal APIs, services, Kafka consumers, workers and other system programs are written in Go. It is a big reason we've been able to scale to the point we've reached. Its efficiency, support for concurrency, simplicity and clarity, great standard library, and statically compiled binaries make it a dream come true in many different ways.

If it were not for Go, we'd probably be only half as far along in product development, paying five times the EC2 bill, or both.

We wrote an [ebook](https://vividcortex.com/resources/building-database-driven-apps-with-go/) on Go and databases. We made a lot of mistakes and that book is largely about what we learned in the process.

## Hardware

We host our entire application in EC2\. We deliberately use relatively weak instances with limited CPU, RAM, and EBS volumes with moderate numbers of provisioned IOPS. This is so wrong in so many ways. It flies in the face of the advice most people give about running databases in EC2\. Notably, it's exactly the kind of scenario I myself denounced 5 years ago. Nevertheless, it works well and it's a financial discipline. It's also a way to avoid solving a problem with hardware short-term and suddenly realizing that we're so far from being able to scale to meet demand that we have a serious issue.

I have not looked recently to see how much data we were ingesting (it's one of those inconvenient cross-environment tasks), but as of a few months ago we ingested an average of 332,000 timeseries points per second, or more than 28 billion per day. At that time we had three shards, so in essence we were storing roughly 100,000 points per server per second. The servers were almost completely idle except for when a fan-out bulk query executed. Keep in mind this is fully ACID transactional storage, not hope-and-pray.

We also replicate each shard locally and across regions for read scaling, failover/DR and for taking backups, so there are actually more than 3 servers in our infrastructure, of course!

It may be an interesting exercise for the reader to compare this number to similar blog posts written about this kind of topic and see how we fare in raw hardware efficiency as well as costs. I think one of the takeaways from comparing us to others is that there's a lot of optimization you can often do, and it can gain one or two orders of magnitude of efficiency, maybe more. At the same time, my honest assessment is that we're neither super-efficient nor grossly inefficient relative to our peers; we're kind of middle of the road.

This is good. But there's a price to be paid for running lean, and you have to optimize the business overall, not just the EC2 bill. Speed to market is a big factor in that.

## No Silver Bullet

Much of the time when I describe our system, particularly our timeseries backend systems, people ask "why don't you just use X?" The suggestion often sounds reasonable until you understand our requirements more deeply. This is not all that surprising, since we didn't know our requirements for a long time ourselves.

Many of the requirements are explained in a [blog post I wrote](http://www.xaprb.com/blog/2014/06/08/time-series-database-requirements/). That post seems to resonate deeply with a lot of people and companies. I think it's worth understanding some of the problems we're trying to solve before asking why we didn't use a specific solution. (Vendors would be well advised to read that post, and do the operations-per-second-per-node math, before pitching me silly solutions that would have us using 100,000 times as many servers as we currently do.)

I don't want to seem defensive, as though my ego makes me unwilling to consider the possibility of a much better solution. But keep in mind that there are literally hundreds of possible solutions to consider and/or evaluate. That's impractical. A quick elimination is my go-to strategy.

Some of the many silver bullets people have proposed include HBase, OpenTSDB, Cassandra, and Hadoop. This is not to criticize any of them. A combination of math, benchmarks, and examination of the requirements we've discovered have led me to conclude that some of these are worse, some are about the same and sometimes even better, but none are good enough out-of-the-box to justify switching.

Some of the solutions could be a win, but only at the cost of further complicating things with a lot of caching and batching--in other words, not turnkey solutions. Sure, we don't use MySQL as a turnkey solution, but it still sets a high bar. Also, keeping the technology surface area simple and clean is a big win for us in itself.

I do continue to keep an eye on specific solutions or combinations, and on emerging work in some databases. I am particularly interested in NDB (MySQL Cluster, the most incredible distributed realtime database you've never heard of), InfluxDB, Cassandra, and Spark.

It would take a long time to naysay all of the options in full detail, but as a _sample_ of a specific rebuttal, using Cassandra as it's usually used for timeseries data would require transferring hundreds of gigabytes (or more) over the network for some of our customers. That's simply because it doesn't yet support evaluation of expressions within the database itself. Similar objections usually exist for most systems that people propose. The explanation is usually that when people have applied solution X to what seems like a similar problem and had success with it, one of the dimensions of scale was O(1) in their situation, whereas it's a cross-product factor in ours. A lot of things work well in many cases but become difficult when you throw an extra cross-product or three into the mix.

There are existence proofs of other companies doing bulk-read type of queries over very large datasets and making it fast. For example, [Scalyr has a good blog post on intelligent brute-force data processing](http://blog.scalyr.com/2014/05/searching-20-gbsec-systems-engineering-before-algorithms/). New Relic Analytics (nee Rubicon) uses brute force. Some of my advisors are also familiar with internal systems that do this kind of analysis at massive scale inside particular very large companies. We've discussed the techniques, costs, and results in detail.

There are a few things to note about this. One is that we already do a lot of fan-out ourselves. Go makes this quite easy to do. When we do a bulk-read query, a lot of compute and storage gets busy, and we do it quite efficiently, so I don't think there's a lot of untapped potential left. The other thing, though, is that there are real limits to how much of 100% fanout you can do. If a giant query stops the world for everyone except one user by design, you have a scalability problem. And this is how at least some of those systems work; the Scalyr blog post does a good job explaining some of the nuances and tradeoffs here. Finally, massive fan-out automatically requires massive data duplication, which exacerbates the write scaling issues.

In the end, although for a little while I thought maybe switching to brute-force would work for the bulk read queries, I convinced myself it wouldn't. Later, one of my advisors introduced me to a storage expert who knows more about the physical capabilities of hardware than I do. We redid the math on the whiteboard and found a similar answer--it would take several thousand of the most expensive machines available running the most advanced PCIe storage at unthinkable cost, to brute-force it fast enough for our requirements. As he expressed it, "you are way too many zeroes away from a workable solution."

## The Evolution of Bulk-Read Optimizations

With the above context, you can probably see that we're basically using MySQL rather simply as a storage engine for a large-scale timeseries database built as a distributed set of Go services. We take advantage of SUM() and a couple of other functions to push computation as close as possible to the data and avoid network transfers, and we take advantage of InnoDB's clustered indexes, which are a huge win. Otherwise we do much of the heavy lifting outside of MySQL.

Hopefully some of the challenges of the bulk reads are obvious to you now. You can probably do some math and figure out rough numbers for how much data we store, how many rows we have, and the like. However, the actual implementation of the bulk read operations has a lot of specifics that matter. At the risk of exposing too much of how the sausage is made, I'll dig deeper into some of these.

To recap:

*   Many kinds of things can be ranked by many dimensions
*   Lots of sparse metrics
*   Lots of hosts
*   Large time ranges
*   Top-N with catchall

Our first implementations of this, back in the earliest days when it was just a prototype, had big inefficiencies everywhere. We basically took the most straightforward nested-loop approach to it you can imagine, something like this:

1.  Query the metrics table and find all possible metrics that match the desired pattern.
2.  For each host, for each metric,
3.  Fetch and resample the metric for the time range.
4.  Use insertion sort to keep the top N.
5.  Sum up the rest for the catch-all.
6.  Return.

Even for a small dataset, this takes a long time and tends to run things out of memory. One of the biggest inefficiencies we noticed right away was that most of the time step 3 would return no data. Another obvious problem was that doing so many queries to the database is really time consuming even if they're all very fast.

The next step was to stop doing single-metric reads and use combinations of metric/host pairs in the queries, like `metric IN(...) and host IN(...)` These lists, however, were extremely large. MySQL basically treats each combination of such lists as separate queries, much as we did before we pushed this into the database. That's fine except that the planning process for such queries replans every combination one at a time and isn't cheap in InnoDB. I wrote more about this in High Performance MySQL. These queries took a long time to plan, even after we did them in smaller batches. And again, most of the combinations yielded no rows for any given time range, due to the sparse nature of the data again.

It was clear that we needed coarse-grained timestamp-first indexing just to eliminate combinations of queries from consideration so we didn't try to look for them. We were still spending most of our time looking for data that didn't exist.

Knowing it wasn't really a big win, but not yet seeing a much better way, we introduced what is known internally to this day simply as "select distinct." There could be many SELECT DISTINCT queries against our database, but this one is so notorious that those two words suffice to identify it. This query found the distinct combinations of metrics and hosts that existed during a time range. It was a little while before we found a solution that improved upon this a lot, so we suffered with this long enough to be scarred.

To fix this, we needed a kind of timeseries fact that said "during time range X, metric Y on host Z has values." Ideally, we'd be able to ask the question "for time range X and hosts Z, what metrics exist?" One way to do this would be a secondary index on the timeseries metrics tables, but that's a super-expensive way to do it. Another would be to create coarse buckets of time--say, by hour--and simply record "yes" for every combination of host/metric that entered our APIs during that hour. This act, of setting something true even if it's already true, is brutally inefficient in MySQL because it's still a transaction, it still gets logged, and it does a lot of other work. We added Redis to handle this index. This improved things greatly, and allowed us to make specific customers' Top Queries much faster.

We also added firstseen/lastseen columns to the metrics table, which we used during Step 1 of the nested-loop algorithm to prune even further. We used Redis as a write-buffer for the updates to these columns, too, because updating those rows with every incoming metric would be way too much work.

Another improvement was in the process of resampling and generating the top N and catchall. We eliminated the resampling and just looked for the top N biggest sums of the desired dimension. This is simple and efficient to push into MySQL directly, with a query similar to

    SELECT host, metric, SUM(val) FROM tbl
    WHERE ts BETWEEN x AND y
    GROUP BY host, metric
    ORDER BY SUM(val) DESC LIMIT N

We took advantage of summary columns for the vectors and several other minor things to improve this as much as possible, but the big win came when we started computing per-category summary series and using them to avoid generating the catchall. Previously we needed the catchall so we could show how much of the total each series constituted. This is important because without it you don't know whether you're [trying to optimize something that doesn't matter](https://vividcortex.com/resources/practical-query-optimization/), among other things. With a total series that was just the sum of all the series in the category, we were able to eliminate this step by adding the top N together and subtracting them from the total. This removed the vast majority of the accesses to individual series, because most of the data is in the long tail for most customers.

Another optimization came from observing that most customers either look at Top Queries across all of their hosts, or at just one or a few hosts. The worst case, of course, is looking at all of them. To solve this, we introduced an all-hosts fast path enabled by an all-hosts aggregation of the series. With this optimization, Top Queries is almost as efficient for 1000 hosts as it is for a single host. (It's an exercise for the reader to figure out why it's not quite as efficient.)

Finally, we eliminated the inefficiency related to the lookups of metrics that match a category pattern, as well as finding a way to cluster the series' data together when they belong to a category. We did this by recognizing and numbering the categories of metrics, and adding a category column as part of the primary key to the "observations" tables. This again relies on InnoDB's clustered primary key storage for its effectiveness. It also made Redis obsolete as an "existence" index external to MySQL, which eliminates entire classes of failure scenarios and inconsistencies. Redis is great, but less is more.

There are many, many details to all of these optimizations, and I'm glossing over some details and introducing inaccuracies, but I'm already going into a lot of detail, so I'll stop there.

The gist of all this is that we're now able to support bulk reads across huge numbers of sparse metrics cross-producted with huge numbers of hosts, as well as read-optimizing individual metrics, by taking advantage of patterns in the data, various kinds of summary metrics, math, and InnoDB's clustered indexes.

## Clustered Primary Keys

Clustered primary keys are one of the huge benefits of MySQL+InnoDB. I [wrote elsewhere](https://vividcortex.com/blog/2014/04/30/why-mysql/) about why we use MySQL.

The way we store data in MySQL, letting it index the data for read-optimization while making the writes as efficient as possible, works better than we have a right to expect. One of the reasons it works so well is that InnoDB is a very sophisticated storage engine. Relative to a lot of databases and storage engines that you might think are more efficient, you'd be surprised how advanced InnoDB's algorithms really are and how much benefit we get for free by using it. This includes transaction logging, write optimizations, checkpointing, and buffer pool management among other things. If we had to build these things ourselves, or use something like one of the new generation of LSM storage engines, we would almost surely run into things like horrible edge cases, massive write stalls, and terribly inconsistent performance. One of the reasons we designed VividCortex the way we did is [so we can detect exactly those kinds of problems](https://support.vividcortex.com/faults/). It's a testament to the InnoDB and MySQL engineers that we get great performance on low-end EC2 instances.

Another of the reasons it works well is the way we partition our data. This keeps our index trees small enough that we don't run into write cliffs when their working sets get larger than memory. The upshot of this, combined with InnoDB's sophisticated flushing algorithms, is that our read optimizations don't really penalize our writes much.

Finally, InnoDB's clustered indexes. If we were not using a database that physically clusters the data in index order, it wouldn't work nearly as efficiently. There are probably ways to do what we do in, say, MongoDB or Postgres (and there are features in Postgres, such as partial indexes, that would enable additional optimizations for us). But without clustered primary keys, it is difficult to imagine how another bread-and-butter database could be made to be as efficient as InnoDB is for us.

Some things in MySQL and InnoDB still aren't as good as they could be, though. There's the inherent inefficiency of the storage engine interface and lots of various kinds of locking; there's extra overhead for lots of ACID stuff we don't exactly need; and there are things that would benefit us if they worked better, such as data compression (which isn't a win for us in InnoDB, but could be in a better implementation). The list of such inefficiencies is very long, and I suspect the opportunity cost is several orders of magnitude. Still, compared to nearly every other widely used database technology that I've examined, MySQL/InnoDB seems to be on the good side of the bell curve.

## Conclusions

I hope you've found this to be a useful and interesting journey into the process of how our service has scaled to meet our growing customer base and our unusual requirements.

We are always improving our systems. We have a lot of dry powder left, so to speak, but we do not prematurely build for problems we can neither see beginning to happen nor calculate are inevitable.

In the end, it's not magic. There's lots of creativity and lots of math, mostly of the napkin variety. Data is subject to the laws of physics, and we can largely predict both the size of our challenges and the limitations of the technology that could be brought to bear to solve them. What's much harder is predicting the kinds of use cases customers will find for a product, and the kinds of questions we need to ask of the database as a result.

I'm continually impressed by the amazing team at VividCortex, who solve hard problems every day and make it seem easy. Big thanks to them, our investors and advisors, lots of generous friends, and customers.

## Related Articles

*   [On Hacker News](https://news.ycombinator.com/item?id=9291237)
*   [GopherCon 2014 Building Database Applications with Database/SQL](https://www.youtube.com/watch?v=m879N2rzn2g) by Baron Schwartz
*   [Baron Schwartz: MySQL, SQL, NoSQL, and Open Source in 2014 and Beyond](https://www.youtube.com/watch?v=f1hSvfDIfbo)
*   [Time-Series Data in MySQL](https://www.youtube.com/watch?v=kTD1NuQXr4k) by Baron Schwartz
*   [Go database/sql tutorial](http://go-database-sql.org/)
*   [VividCortex/dbcontrol](https://github.com/VividCortex/dbcontrol) - A wrapper around Go's database/sql package that provides connection pool limits 

    