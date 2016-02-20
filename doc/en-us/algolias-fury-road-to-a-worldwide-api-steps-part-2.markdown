## [Algolia's Fury Road to a Worldwide API Steps Part 2](/blog/2015/7/20/algolias-fury-road-to-a-worldwide-api-steps-part-2.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, July 20, 2015 at 9:13AM</span></div>

<div class="body">

<span id="docs-internal-guid-10d2cf41-a8d2-8db2-b090-65f7d08ab506">  
</span>

<span>![](https://lh4.googleusercontent.com/PeLIUSrdPpM24jTyxzQWxth4F38C59buuKLUVeVMcxDpymeq6CQa4AkwildQQh69tIeWcbcHdbXHS1_dHGKodFeYJG-gTVgswGwmR-DiVKvVTVjnbw8nC1TNc9vCpZFykphDGg)</span>

The most frequent questions we answer for developers and devops are about [our architecture](http://highscalability.com/blog/2015/3/9/the-architecture-of-algolias-distributed-search-network.html) and how we achieve such high availability. Some of them are very skeptical about high availability with bare metal servers, while others are skeptical about how we distribute data worldwide. However, the question I prefer is “How is it possible for a startup to build an infrastructure like this”. It is true that our current architecture is impressive for a young company:

*   <span>Our high-end dedicated machines are hosted in 13 worldwide regions with 25 data-centers</span>

*   <span>our master-master setup replicates our search engine on at least 3 different machines</span>

*   <span>we process over 6 billion queries per month</span>

*   <span>we receive and handle over 20 billion write operations per month</span>

<span>Just like Rome wasn't built in a day, our infrastructure wasn't as well. This series of posts will explore the 15 instrumental steps we took when building our infrastructure. I will even discuss our outages and bugs in order to you to understand how we used them to improve our architecture.</span>

<span>The</span> [<span>first blog post</span>](http://highscalability.com/blog/2015/7/13/algolias-fury-road-to-a-worldwide-api.html) <span>of the series focused on the early days of our beta. This blog post will focus on the first 18 months of our service from September 2013 to December 2014 and even include our first outages!</span>

## <span>Step 4: January 2014</span>

### <span>Deployment is a big risk for high availability</span>

<span>At this time, one of our major concerns was making sure our development was agile while not having it at the cost of stability. For this reason, we developed a test suite of 6,000+ unit tests and 200+ non-regression tests to ensure code changes did not introduce new bugs. Unfortunately, it wasn't enough. A new feature that passed all our tests introduced a bug in production that caused</span> [<span>eight minutes of indexing downtime</span>](https://blog.algolia.com/postmortem-todays-8min-indexing-downtime/)<span>. Thankfully, because our architecture was designed to keep search queries separate from indexing, no search queries were impacted.</span>

<span>This issue helped us identify several problems in our high availability setup:</span>

*   <span>Rollbacks need to be fast so we developed the ability to perform a rollback with a single command line.</span>

*   <span>Deployment scripts need to perform sanity checks and automatically rollback if there are errors so we added it to our deployment procedure.</span>

*   <span>We shouldn't deploy to all of our production clusters just because the tests passed</span><span>.</span> <span>After this issue, we put in place a sequence of deployment for clusters. We start with our test clusters, followed by the community clusters (free customers), and finally the paying user clusters.</span>

<span>Today, our deployment is a bit different when there is a new feature to test. We select a set of clusters to test and we deploy using the following steps:</span>

1.  <span>Deploy to the first machine in all clusters of the selected set.</span>

2.  <span>Monitor for 24 hours then deploy to the second machine in all clusters of the selected set.</span>

3.  <span>Monitor for 24 hours then deploy to the third machine in all clusters of the selected set.</span>

4.  <span>After a few days, we deploy to all our production clusters.</span>

<span>Using this approach, we were able to detect bugs within a few hours that were nearly impossible to find with unit tests. This detection was possible because of the billions of queries we have per month.</span>

# <span>Step 5: March 2014</span>

### <span>Manage high load of write operations</span>

<span>While our clusters in Canada/East and Europe/West were handling our growth very well, we moved to solving a new problem: latency. The latency of our clusters from Asia was too high for acceptable performance.</span> <span></span> <span>We decided to test the market first by deploying machines in AWS. We didn't like having to make this choice since the performance of search queries was 15% lower than our E5-2687W CPU even when using the best CPU offered by AWS (E5-2680 at that time)! We went this route in order to reduce the time to launch the experiment but made sure to not introduce any dependencies on AWS. This allowed us to easily migrate to another provider if the tests were successful. On paper, it all looked good. We ran into one issue when we discovered their virtualization did not support AVX instructions, which was an important feature for us. We had to generate binaries without the AVX instruction set (which reduced again a bit the performance of search queries).</span>

<span>While we were enjoying our recent launch in Singapore we started to receive some complaints from European customers about an increased latency in their search queries. We quickly identified it was correlated with a big spike of indexing operations, which was weird since we were confident our architectural split between indexing and search CPU usage was working correctly. After investigating, we found that the consensus algorithm for distributed coherency across the cluster that handled write operations had a potential bottleneck. When the bottleneck occurred, it would block the HTTP server threads. This would then lead to search queries needing to wait for a thread. We had accidentally created this issue by design!</span>

<span>We fixed this issue by implementing a queue before our consensus. It allowed us to receive a spike of write operations, batch them, and then send them to the consensus. The consensus was no longer in the sequence of the HTTP server so write operations could not freeze the HTTP server thread anymore. During normal operation, the queue is not doing anything except passing the job to the consensus. In cases where we have spikes, it allowed us to buffer and batch before calling the consensus. After we made this change, we have not had any clusters freeze.</span>

## <span>Step 6: April 2014</span>

### <span>Network high availability is close to impossible with one data center</span>

<span>In the beginning of April 2014, we started receiving complaints from our US-East customers that used a cluster in Canada/East. Our users in US-West were not impacted. We immediately suspected a network issue between Canada and US-East and discovered our provider was reporting the same issue. A car accident had broken a pipe containing 120 fibers between Montreal and New York. All the traffic was being routed on a different path through Chicago. Unfortunately the bandwidth was not good enough to avoid packet lost.  This outage was not only impacting our provider but all traffic between Canada and US-East. This is the type of accident that can break the Internet that we often forget about, but it can still happen. There was nothing we could do that day to help our customers except reach out to each one and notify them of the current situation.</span>

<span>It was on this day we kicked off a big infrastructure discussion. We needed to improve our high availability by relying on more providers, datacenters, and network providers. More importantly, we needed to have our infrastructure in the US. By serving the US from Canada, we were able to cut our costs but our high availability suffered. We needed to have our infrastructure truly distributed. We couldn’t continue having several availability zones on a single provider!</span>

## <span>Step 7: July 2014</span>

### <span>First deployment on two data centers</span>

<span>We had discovered our deployment in a single datacenter was not enough. We decided to start with one of our biggest customers by deploying a machine in two different datacenters (with more than 100 kilometers between them). These two datacenters were using the same provider (same Autonomous System) and already offered a level of high availability a bit above cloud provider multiple availability zones. This was due to the two datacenter locations being completely different in regards to network link and power units.</span>

<span>Based on our past experience, we took this time to design the next version of our hardware with next generation of CPUs and SSDs:</span>

*   <span>Xeon E5-1650v2 (6 cores, 12 threads, 3.5Ghz-3.9Ghz)</span>

*   <span>128G of RAM</span>

*   <span>Intel SSD S3500 (2x480G) or S3700 (2x400G). We use the intel S3700 for machines with high amounts of writes per day as they are more durable</span>

<span>The changes we made were mostly around the CPU. We were never using 100% of the CPU on our E5-2687W. The E5-1650v2 was the next generation CPU and provided higher clock speeds. The result was that our service was able to gain close to 15% in speed compared to the previous CPU. Milliseconds matter!</span>

## <span>Step 8: September 2014</span>

### <span>Presence in the US!</span>

<span>After spending a few months in discussions with several providers, we launched the service in US-East (Virginia) and US-West (California) in September 2014 with a new provider. This first step was a great way to improve our search experience in the US thanks to better latency and bandwidth. The improvement to high availability was not big but it did help us be more resilient to the past problems we had between Canada and US-East. We used the same approach as we did with previous locations: different availability zone within one provider (different network equipment and power unit).</span>

## <span>Step 9: October 2014</span>

### <span>Automation via chef</span>

<span>With a significant increase in the number of machines we needed to manage, we migrated our management to Chef. Previously, we managed our machines using shell scripts but fixing security issues like Heartbleed (OpenSSL vulnerability) was time consuming compared to automation with Chef.</span>

<span>The automation provided by Chef was very useful when configuring hundreds of machine, but it had it drawbacks. A few months after migrating to Chef, a typo in a cookbook caused some production servers to crash. Luckily, we were able to discover the problem early on. It didn’t impact much of the production thanks to the non-aggressive nature of the CRON job that launched the Chef client. We had enough time to react and fix the issue.</span>

<span>We got really lucky with this issue because the error could have broken production. In order to avoid this type of problem from happening again, we decided to split our production cookbooks into two versions:</span>

*   <span>The first version, the stable one, was deployed to the 1st and 2nd machine of all clusters</span>

*   <span>The second version, the production one, was deployed on the 3rd machine of all clusters</span>

<span>When any modification is made to our cookbook, we first apply the modification to the production version. After a few days of testing, we apply the modification to the stable cookbook version. High availability needs to be applied at all levels!</span>

## <span>Step 10: November 2014</span>

### <span>DNS is a SPoF in the architecture</span>

<span>Over time, we started to receive more and more feedback from users stating our service was intermittently slow, especially in Asia. After investigating, we found our usage of the .io TLD was the cause of this issue. It turned out the anycast network of the .io TLD has fewer locations than the main TLDs (.net, .com, and .org) and the DNS servers are overloaded. Users would sometimes encounter timeouts during DNS resolution.</span>

<span>We discussed this issue with CDN providers and they all told us the best practice was to use the .net TLD. We needed to migrate from .io to .net! While preparing for the release of our Distributed Search Network, we decided to move to another DNS provider. This new provider offered the linked domains feature, which meant they would handle the synchronization between</span> [<span>algolia.io</span>](http://algolia.io) <span>and</span> [<span>algolia.net</span>](http://algolia.net) <span>for us so we could maintain backwards compatibility easily.</span>

<span>We performed extensive testing from different locations for this migration and felt comfortable everything was working well. Unfortunately, after we completed the migration we discovered several problems that impacted some of our users. You can read a detailed report in</span> [<span>this blog post</span>](https://blog.algolia.com/black-thursday-dns-issue/)<span>. DNS is far more complex than we realized. There are many different behaviors of DNS providers and our tests were not covering all of them (e.g., resolution in IPv6 first, different behaviors on TTL, etc.)</span>

<span>This issue also changed our minds about identifying SPoF. Having only one DNS provider is a SPoF and the migration of this piece was highly critical since there was no fallback. We started working on a plan to remove any SPoF in our architecture.</span>

## <span>Next</span>

<span>At this point we have covered 10 of the 15 steps in this series. As you can see, the first 18 months after our launch have been anything but an easy ride. We endured a few outages and had to rethink several parts of our infrastructure. The goal of these improvements was to always make sure it would be near impossible for our infrastructure to face similar problems in the future.</span>

<span>The next and last part of this blog series will focus on DSN (Distributed Search Network), the feature we are most proud of, and improving our high availability. Both of these have a single objective: meet the expectations of big public companies when it comes to reliability and performance.</span>

_Here are all three parts of the series: [Part 1](http://highscalability.com/blog/2015/7/13/algolias-fury-road-to-a-worldwide-api.html), [Part 2](http://highscalability.com/blog/2015/7/20/algolias-fury-road-to-a-worldwide-api-steps-part-2.html), [Part 3](http://highscalability.com/blog/2015/7/27/algolias-fury-road-to-a-worldwide-api-part-3.html)_

</div>