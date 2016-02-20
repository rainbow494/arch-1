## [The Stunning Scale of AWS and What it Means for the Future of the Cloud](/blog/2015/1/12/the-stunning-scale-of-aws-and-what-it-means-for-the-future-o.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, January 12, 2015 at 9:05AM</span></div>

<div class="body">

![](https://farm8.staticflickr.com/7522/16238755496_4a3014ebbb_m.jpg)

[<span>James Hamilton</span>](http://www.mvdirona.com/jrh/work/), VP and Distinguished Engineer at Amazon, and long time [blogger](http://perspectives.mvdirona.com/) of interesting stuff, gave an enthusiastic talk at [<span>AWS re:Invent 2014</span>](https://reinvent.awsevents.com/) on [<span>AWS Innovation at Scale</span>](https://www.youtube.com/watch?v=JIQETrFC_SQ). He’s clearly proud of the work they are doing and it shows.

James shared a few eye popping stats about AWS:

*   1 million active customers
*   All 14 other cloud providers combined have 1/5th the aggregate capacity of AWS (estimate by Gartner in 2013)
*   449 new services and major features released in 2014
*   Every day, AWS adds enough new server capacity to support all of Amazon’s global infrastructure when it was a $7B annual revenue enterprise (in 2004).
*   S3 has 132% year-over-year growth in data transfer
*   102Tbps network capacity into a datacenter.

<span>The major theme of the talk is the cloud is a different world. It’s a special environment that allows AWS to do great things at scale, things you can’t do, which is why the transition from on premise x86 servers to the public cloud is happening at a blistering pace. With so many scale driven benefits to the public cloud, it's a transition that can't be stopped. The cloud will keep getting more reliable, more functional, and cheaper at a rate that you can't begin to match with your limited resources, generalist gear, bloated software stacks, slow supply chains, and outdated innovation paradigms.</span>

<span>That's the PR message at least. But one thing you can say about Amazon is they are living it. They are making it real. So a healthy doubt is healthy, but extrapolating out the lines of fate would also be wise.</span>

One of the fickle finger of fate advantages AWS has is resources. At one million customers they have the scale to keep the engine of expansion and improvement going. Profits aren't being taken out, money is being reinvested. This is perhaps the most important advantage of scale.

But money without smarts is simply waste. Amazon wants you to know they have the smarts. We've heard how Google and Facebook build their own gear, Amazon does too. They build their own networking gear, networking software, racks, and they work with Intel to get faster processor versions of processors than are available on the market. The key is they know everything and control everything about their environment, so they can build simpler gear that does exactly what they want, which turns out to be cheaper and more reliable in the end.

<span>Complete control allows quality metrics to be built into everything. Metrics drive a constant quality increase in all parts of the system, which is why against all odds AWS is getting more reliable as the pace of innovation quickens. Great pools of actionable data turned into knowledge is another huge advantage of scale.</span>

<span>Another thing AWS can do that you can't is</span> the Availability Zone architecture itself. Each AZ is its own datacenter and AZs within a region are located very close together. This reduces messaging latencies, which means state can be synchronously replicated between AZs, which greatly improves availability compared to the typical approach where redundant datacenters are very far apart. 

It's a talk rich with information and...well, spunk. The real meta-theme of the talk is how Amazon consciously uses scale to their competitive advantage. For Amazon scale isn't just an expense to be dealt with, scale is a resource to exploit, if you know how.

Here's my gloss of James Hamilton's incredible talk...

## <span>Everything in the Talk has a Foundation in Scale</span>

*   <span>Every day, AWS adds enough new server capacity to support all of Amazon’s global infrastructure when it was a $7B annual revenue enterprise (in 2004).</span>

*   <span>365 days a year component manufacturers have to get gear to server and storage manufacturers, the server and storage manufacturers have to produce the gear and push it into the logistics channel, it has to get from the logistics channel over to the correct datacenter, it has to arrive at the loading dock, people have be there to wheel the racks to the right place in the DC, there has to be power, cooling, networking, the app stack has to be loaded up, it has to be tested, it has to be released to customers.  </span>

*   <span>S3 usage: 132% year-over-year growth in data transfer; EC2 usage: 99% YoY usage growth; AWS overall business: over 1 million active customers.</span>

*   <span>All 14 other cloud providers combined have 1/5th the aggregate capacity of AWS (estimate by Gartner in 2013)</span>

*   <span>With over a million customers it means you are in a rich ecosystem. You have your pick of software vendors, if you have a problem someone has likely had it before, it’s easier to get your job done fast.</span>

*   <span>Such high growth means Amazon has the resources to keep reinvesting and innovating by increasing breadth and depth of services they offer.</span>

*   <span>Big Transitions generally occur when the economics are far superior, like mainframes to UNIX servers and then UNIX servers to x86 servers. These transitions usually take 10+ years. What’s different about the x86 on premise transition to the cloud is the speed at which it is happening.  The speed of the cloud transition is a function of great economic value along with the low friction for adoption. You don’t need software, you don’t hardware, you can just do it.</span>

## <span>There are Big Cost Problems in Networking</span>

*   <span>Networking is a red alert situation across the industry. It’s the perfect storm.</span>

*   **Problem #1**: The cost of networking is escalating relative the cost of all other equipment. It’s anti-Moore’s law. All other gear is going down in cost, networking is getting relatively more expensive over time. Relative monthly costs: servers: 57%; networking equipment: 8%; power distribution and cooling: 18%; power: 13%; other: 4%.

*   **Problem #2**: At the same time networking is getting more expensive, the ratio of networking to compute is going up. That’s partly because Moore’s law is working (still) with servers and compute density is going up. Partly it's because as the cost of compute falls the amount of advanced analytics performed goes up and analytics are network intensive. Solving big problems using a large number of servers requires a lot of networking. Network traffic has moved east-west rather than the traditional [<span>north-south direction</span>](http://highscalability.com/blog/2012/9/4/changing-architectures-new-datacenter-networks-will-set-your.html).

*   Amazon’s solution 5 years ago was data driven and radical: **they built to their own networking designs**. Special routers were built. A team was hired to build the protocol stack all the way to the top. And they deployed all this themselves in their network. All services worldwide run on this gear.

    *   **This strategy turned out to be a lot cheaper**. Just the support contract for networking gear was running 10s of millions of dollars.

    *   **Availability went up**. The source of the improvement was simplicity. The problem AWS was trying to solve was simpler than the problem enterprise gear tries to solve. Enterprise gear must adhere to a lot of complicated specs that go unused and only make the system more fragile. By implementing just the functionality that was required meant a much simpler system which lead to higher availability. Any way to win is a good way to win.

    *   **A cornucopia of metrics**. They measure everything. The rule is if a customer has a bad experience using their system their metrics must show it. This means metrics are getting better all the time because customer problems drive better metrics. Once you have metrics that accurately reflect customer experience then goals can be set on making the system better. Every week improvements are made to make things better. If the code didn’t start off better, it gets better over time.

    *   **Testability**. Their gear was better because they tested it better. Enterprise gear is hard to test at scale. They created a $40 million test bed of 8000 servers (3 megawatts). But since this was the cloud they effectively rented the servers for a few months, so it was relatively cheap.

## <span>Networking Explained Layer by Layer, from the very top to the Network Interface Card</span>

### <span>AWS Worldwide Network Backbone</span>

*   <span>11 AWS regions worldwide. Choose which ones to use by nearness to customers or required jurisdictional boundaries.</span>

*   <span>Private fiber links interconnect most of the major regions. This avoids all the capacity planning problems (Amazon can do better capacity planning), peering issues, and buffering problems that occur on public links. So it’s faster to run their own network, it’s more reliable, cheaper, and lower latency.</span>

### <span>Example AWS Region (US East ((Northern Virginia))</span>

*   <span>All regions have at least two availability zones. US East has five AZs.</span>

*   <span>Redundant paths run to transit centers.</span>

*   <span>Each region has redundant transit centers. A transit center connects private links to other AWS regions, private links to AWS Direct Connect customers, and to the internet through peering and paid transit.</span>

*   <span>If one AZ fails all the other AZs keep working.</span>

*   <span>Metro-area DWDM links between AZs</span>

*   <span>82,864 fiber strands in a regions</span>

*   <span>AZs are less than 2ms apart and usually less than 1ms apart. From a latency perspective they are fairly close, within a few kilometers. Far enough apart for safety, close enough for good latency.</span>

*   <span>25Tbps peak traffic between AZs</span>

*   <span>AWS offers AZs because:</span>

    *   <span>With a single hardened datacenter the best reliability you’ll get is around </span>[<span>99.9%</span>](http://en.wikipedia.org/wiki/High_availability) <span>over a mix of applications over a large period of time. High reliability requires running in two datacenters. Traditionally datacenter diversity is from two datacenters that are very far apart because it’s not cost effective to keep datacenters close together. This means longer latencies. LA to NEW is 74ms round trip. Committing to an SSD is 1 to 2ms. You can’t wait 70+ milliseconds for a transaction to commit. Which means applications commit locally and then replicate to the second datacenter. This design in a failure case loses data during the failover. While a true failure is rare, like a building burning down, transient failures are more common, like a load balancer problem for example. So would you failover your connection was down for 3 minutes? No, because data would be lost and it would take a long time to recover that data from other sources. So you lose availability for common events.</span>

    *   <span>AZs are milliseconds apart so you can commit to both at the same time. That means if you failover a customer won’t be able to tell because the data replication was synchronous. It’s invisible. It’s hard to write code for this model so you won’t do it for everything. And for some apps a concern for multi-AZ failures might also prevent you from using multiple AZs, but for the rest of apps this is a very powerful model. It’s more costly, but it gives AWS certain advantages.</span>

### <span>Example AWS Availability Zone</span>

*   <span>An Availability Zone is always a datacenter in a completely independent building.</span>

*   <span>Amazon has 28+ datacenters. The plus means there are more datacenters in an AZ as a way of extending capacity for an AZ. More datacenters are added within an AZ to extend the capacity of an AZ. Otherwise you would be forced to split your app across AZs, even if you didn’t want to.</span>

*   <span>Some AZs have size fairly substantial datacenters.</span>

*   <span>DCs in an AZ are less than ¼ms apart.</span>

### <span>Example Datacenter</span>

*   <span>AWS datacenters are purposely not gigantic. A single datacenter is 25 - 30 megawatts, with between 50,000 - 80,000 servers</span>

*   <span>The return on datacenter largeness diminishes. The advantage of datacenter scale as you build bigger and bigger goes down. Early advantages are huge. Later advantages are small. Going from 2000 to 2500 racks is a little better. A tiny datacenter is too expensive. A really large datacenter is only marginally more expensive per rack than a medium datacenter.</span>

*   <span>Risk increases with larger datacenters. The blast radius if something goes wrong and the datacenter is destroyed, the loss is too big.</span>

*   <span>102Tbps network capacity into a datacenter.</span>

### <span>Example Rack, Server & NIC</span>

*   <span>The only thing that matters for latency is the software latency at either end of a connection. Latency between two servers when sending a message:</span>

    *   <span>Your app -> guest OS -> hypervisor -> NIC : the latency is milliseconds</span>

    *   <span>through the NIC : the latency is microseconds</span>

    *   <span>over the fiber : the latency is nanoseconds</span>

*   <span>SR-IOV (</span>[<span>Single Root I/O Virtualization</span>](http://www.redbooks.ibm.com/abstracts/redp5065.html?Open)<span>) allows a NIC to provide virtualize in hardware network cards. Each guest gets its own network card. </span>The benefit is > 2x average latency reduction and > 10x latency jitter improvement. This means outliers a down to a 1/10th of what they were. SR-IOV is being deployed now on newer instances types and will eventually be everywhere. The hard part wasn't adding SR-IOV, it was adding isolation, metering, DDoS protection, and the capacity limits that make SR-IOV useful in a cloud environment.

## <span>AWS Custom Server & Storage Designs</span>

*   **Cost of a negative situation is not high so expensive unneeded protection can be removed**. Servers are designed for what they do, not a general population of users. Amazon knows exactly in what environment the server will run in, they’ll know exactly when something goes wrong, so the servers can be designed with less engineering headroom. The cost of server failure is not that big for them. They are already on site and are very good at replacing harddisks, etc. So a lot of the carefulness in enterprise equipment is not necessary.

*   **Processors can be pushed harder**. They know the cooling requirements, they influence the mechanical design, they just design good servers, so they can get more performance out of a server. Though a partnership with Intel Amazon has processors that run faster than can be bought on the open market.

*   <span>An example is the design for their own storage rack. It weighs over a ton, 19” wide, and holds 864 disk drives. For some workloads this a wonderful game changing design that helps them get better prices in some areas.</span>

## <span>Power Infrastructure</span>

*   <span>Amazon has designed and built their own power substations. It only saves a little money, but they can build them much faster. Utility companies are not used to dealing with the rate AWS is growing at, so they had to build their own.</span>

*   <span>3 100% carbon neutral regions: US West (Oregon), AWS GovCloud (US), EU (Frankfurt)</span>

## <span>Rapid Pace of Innovation</span>

*   <span>449 new services and major features released in 2014\. 24 in 2008, 48 in 2009, 61 in 2010, 82 in 2011, 159 in 2012, 280 in 2013.</span>

*   <span>AWS is getting more reliable as the pace of innovation quickens. Their goal is to make available to customers the same tools that use to achieve this rate of innovation and high quality.</span>

## <span>Related Articles</span>

*   On [Hacker News](https://news.ycombinator.com/item?id=8875549) / On [reddit](http://www.reddit.com/r/programming/comments/2s6nf6/the_stunning_scale_of_aws_and_what_it_means_for/)

*   <span>James Hamilton’s</span> [<span>Blog</span>](http://perspectives.mvdirona.com/)<span>, other</span> [<span>talks and slides</span>](http://mvdirona.com/jrh/work/)

*   [<span>A Rare Peek Into The Massive Scale of AWS</span>](http://www.enterprisetech.com/2014/11/14/rare-peek-massive-scale-aws/) <span>and</span> [<span>on Hacker News</span>](https://news.ycombinator.com/item?id=8643248) <span>/</span> [<span>on reddit</span>](http://www.reddit.com/r/programming/comments/2n5p8c/a_rare_peek_into_the_massive_scale_of_aws/)

*   [<span>A Day in the Life of a Billion Packets</span>](https://www.youtube.com/watch?v=Zd5hsL-JNY4)

*   <span>[How and why did Amazon get into the cloud computing business?](http://www.quora.com/How-and-why-did-Amazon-get-into-the-cloud-computing-business)</span>

</div>