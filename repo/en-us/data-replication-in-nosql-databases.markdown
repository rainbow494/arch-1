## [Data Replication in NoSQL Databases](/blog/2012/7/9/data-replication-in-nosql-databases.html)

    

    

<iframe width="244" height="150" src="http://www.youtube.com/embed/yjspIn_Mta8?rel=0" frameborder="0" align="RIGHT"></iframe>

    This is the third guest post (    [part 1](http://highscalability.com/blog/2012/4/25/the-anatomy-of-search-technology-blekkos-nosql-database.html)    ,     [part 2](http://highscalability.com/blog/2012/5/28/the-anatomy-of-search-technology-crawling-using-combinators.html)    ) of a series by Greg Lindahl, CTO of blekko, the spam free search engine. Previously, Greg was Founder and Distinguished Engineer at PathScale, at which he was the architect of the InfiniPath low-latency InfiniBand HCA, used to build tightly-coupled supercomputing clusters.    

blekko's home-grown NoSQL database was designed from the start to support a web-scale search engine, with 1,000s of servers and petabytes of disk. Data replication is a very important part of keeping the database up and serving queries. Like many NoSQL database authors, we decided to keep R=3 copies of each piece of data in the database, and not use RAID to improve reliability. The key goal we were shooting for was a database which degrades gracefully when there are many small failures over time, without needing human intervention.

## Why don't we like RAID for big NoSQL databases?

Most big storage systems use RAID levels like 3, 4, 5, or 10 to improve reliability. A conservative choice for a big NoSQL database might be to use a bunch of nodes with RAID, and then replicate the data to several nodes. That doesn't seem to be common among big NoSQL databases. Here are some reasons why RAID is disliked:

*   RAID requires more disks per server than I might want to buy. If I want single disk servers, RAID means I have buy twice as many disks.
*   RAID requires prompt maintenance when things fail. If I have one online spare in a RAID set, I need to replace a failed disk promptly to avoid a second failure, and an outage.
*   RAID rebuilds cause degraded performance for the whole RAID set for a long time when a single disk fails.
*   RAID degrades performance of both reads and writes, compared to the aggregate raw performance of all of the disks in the RAID set.
*   Single-server RAID doesn't provide protection against a server failure.

## R=3: An alternative

The alternative is to keep R=3 copies of each piece of data, on 3 different servers, and to use software consistency built into the database to keep the data consistent. This R=3 strategy fixes all of our RAID complaints:

*   R=3 scales down to 1 disk per server.
*   Rebuilds can use the entire cluster to finish quickly, without degrading performance of part of the cluster severely.
*   R=3 clusters can use 100% of the raw read capacity of all disks in a cluster.
*   R=3 protects against server failures. A failure of a single server means at least 2 copies of everything exist somewhere else in the cluster.
*   Finally, R=3 degrades gracefully as disks fail, if you get all of the details right.

Why 3 copies? It's a trade off between expense and availability. With R=3, my cluster is available as long as I can re-copy data on the failed disks before 3 failures happen.

## Isn't 3 copies of everything expensive?

It's true that keeping 3 copies of everything involves more disks than the typical RAID setup. However, if you stay in the sweet spot of direct-attached storage, the first 8-12 direct-attach disks are pretty inexpensive compared to what you're spending on cpus, ram, flash disk, and your network. Having more disks helps performance -- it gives you more read bandwidth. The extra disks don't help write bandwidth, since everything must be written R=3 times. Network bandwidth also takes a hit, although that's necessary in any scheme which protects against server failures.

## Rebuilds in the R=3 World

Rebuilds tend to be expensive in any system preserving reliability. A 5 disk, fully-used RAID-5 volume with a failure needs to read 4 disks completely and write one disk completely to return to full protection and performance. This can take quite a while, and is getting worse over time as disk capacities increase faster than disk bandwidth. In an R=3 cluster, a single disk contains chunks of data replicated to other disks elsewhere in the cluster. For rebuild purposes, it's nice to have at least 10 chunks on a disk, and it's also nice for the 2 replicas for these 10 chunks to be on as many servers as possible. A rebuild then involves copying 1/10 of a disk between 10 pairs of systems, and the entire rebuild can be done in 1/10 the time of reading an entire disk. The rebuild of all the data on a failed server with 10 disks would involve 100 pairs of servers. While this eats up network bandwidth in comparison to any rebuild that takes place within a single server, the performance impact can be spread out over an entire cluster, instead of creating a performance hot-spot. While we've chosen 10 chunks per disk at blekko, some other NoSQL databases have as many as a million chunks per disk. Having a small number of chunks per disk leads to slow rebuilds that have big performance impact to a small number of nodes, and a performance hot-spot. Failing to spread out chunks properly also increases the time to rebuild and leaves hot-spots.

## Some jargon: Arrr!

There's a fair bit of jargon which isn't standardized yet in the world of R=3 NoSQL databases, so let me give you a few terms that we use inside blekko. We call the R-level of a cluster the minimum replica count of any chunk in the cluster, but just to confuse things, we also refer to the R-level goal of the cluster (usually 3) or the R-level of an individual chunk of data. So if our R3 (a goal of 3 copies of everything) cluster has a single chunk that's R2 (2 copies of this chunk), we call the entire cluster R2 (the worst R-level of any chunk in the cluster.) While you might think this triple-usage is confusing, in practice it seems to work out OK. As long as we're at R1 or better, we can read all of the data in the database. R1 is a scary place to be, though, since a single failure might cause the cluster to go to R0, where some data can't be read. Also, at some point between R3 and R1, we stop being able to write new data into the cluster efficiently. If all chunks are at R1, for example, and we return to R3, a lot of data will need to be replicated for all the replica chunks to become current.

## Chunk placement

In an ideal world, each failure in an R3 cluster would only reduce the overall R-level of the cluster by one. Failures aren't random: they happen at the disk, server, PDU (power circuit), and network switch levels. If we let several replicas of a chunk live on the same disk, same server, same PDU, or same non-redundant network switch domain, we reduce the resilience of our database. Improving resilience in this circumstance is easy: let's make a rule that only one replica of a chunk can live on any disk, server, PDU (1 rack in our datacenter) or leaf network switch (3 racks in our datacenter.) Our central switch infrastructure is redundant, and brings everything down when it fails, so we don't have to worry about it in this context. Now that we've made the cluster behave more gracefully in response to failures, let's also consider administrative convenience. It's nice to be able to take down a large fraction of the cluster at once in order to do things like add RAM or upgrade software. We already have the ability to take down 3 racks (1 leaf network switch) at once without reducing the R-level by more than 1, but in theory we should be able to take down up to 1/3 of our cluster at once. To make this possible, we divide the cluster into 3 or 4 "zones", and make sure no chunk is replicated twice in a zone.

## Properties of the Repair Daemon

Our NoSQL has a separate daemon, the Repair Daemon, responsible for duplicating, moving, and removing chunks in the database, subject to the placement rules in the previous section. This daemon also has some additional abilities related to "draining" a server or disk known to be failing. The information that something is about to fail can come from sources such as disk errors and ECC errors in syslog, SMART daemon complaints, and failed fans or high temperature events reported by the bmc. "draining" chunks off before the disk or server fails completely can reduce the time a cluster spends at R2, which makes it less likely that the cluster will go to R1 or R0 due to an unlucky rapid series of failures. The repair daemon drains a disk or server by making a temporary 4th copy of the chunks involved, and then deleting the chunks on the drained disk or server. When all chunks are removed, the disk or server can be repaired. Failing disks happen often enough in our environment that we have fully automated the decision to drain a disk, or an entire server if the failing disk is the system disk.

## Failures which can easily cause R0

With all this talk of reliability, it's important to remember that there will always be classes errors which can easily cause R0\. One such problem is bugs in the NoSQL database itself; if data in a row causes the database to fail, all 3 replicas of the chunk containing the row can fail. This sort of bug has happened to us several times.

## Other NoSQL-related storage topics

There are a few storage-related topics that I'd like to briefly talk about, even though they aren't directly related to R=3 databases.

*   Checksums. Most systems rely on the storage system to give back the same bits that you wrote a long time ago. While there is a lot of ECC and other error detection in disks, anyone with a large number of disks is very aware that undetected errors happen. We use strong checksums to protect all of the data we write to spinning rust or flash disks. A failed checksum causes the entire chunk to fail. Having checksums also allows scrubbing data, similar to how RAID systems scrub disks in the background.
*   Network checksums. The same arguments about disk checksums apply to network checksums -- TCP and IP have weak checksums, but with modern systems, checksum offload means that you aren't protected against errors between the checksum offload engine and the CPU.
*   Booting. In an R=3 system, it's important that servers with failed disks -- disk failures often happen at boot or reboot -- come up with as many disks as possible, instead of waiting for admin intervention due to a single failed disk. We use a custom initscript to mount our data disks. It also runs a short performance test on each disk, to find the occasional disk which has become slow despite not reporting errors.
*   Trash. Like most NoSQL databases, ours stores the table pieces in chunks in a series of files, occasionally merging several files into a new file, deleting the old ones. Deleting big files is a high-latency operation in Linux and most other OSes. We avoid this issue by renaming files to be deleted into a trash directory, where a dedicated trash daemon for each disk takes one file at a time, truncating it slowly down to nothing. This spreads the load of file deletion over a long enough period of time that the latency hit disappears.

## blekko's track record

You can probably tell from reading this post that we've learned a lot of lessons the hard way. We've run the system as described for about 4 years now, starting with 150 servers and growing to 1,500, scaling our raw disk storage from 1 petabyte to 15\. During this time, we have never gone below R1 due to hardware failures. We've had site downtime, but it's always been related to software bugs (the "baseball game incident" was very unfortunate), or network issues.

    