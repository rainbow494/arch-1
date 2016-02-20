## [Algolia's Fury Road to a Worldwide API Part 3](/blog/2015/7/27/algolias-fury-road-to-a-worldwide-api-part-3.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, July 27, 2015 at 8:56AM</span></div>

<div class="body">

<span id="docs-internal-guid-21eb7309-c6e3-2b05-8b56-869278e51179"></span>

<span>![](https://lh6.googleusercontent.com/PaQgtoUcXRwy_agDKkBD1PBtZnoU4wudmeTkM46UBSX5d4TfiITy5dxV0549de06XvKELOmPY-fsWjUZmadyiyfVUtGs51qKOt7J9z0_RvCku-m_tlwlGL49udAOotuVtJtAEg)</span>

<span>The most frequent questions we answer for developers and devops are about</span> [<span>our architecture</span>](http://highscalability.com/blog/2015/3/9/the-architecture-of-algolias-distributed-search-network.html) <span>and how we achieve such high availability. Some of them are very skeptical about high availability with bare metal servers, while others are skeptical about how we distribute data worldwide. However, the question I prefer is “How is it possible for a startup to build an infrastructure like this”. It is true that our current architecture is impressive for a young company:</span>

*   <span>Our high-end dedicated machines are hosted in 13 worldwide regions with 25 data-centers</span>

*   <span>our master-master setup replicates our search engine on at least 3 different machines</span>

*   <span>we process over 6 billion queries per month</span>

*   <span>we receive and handle over 20 billion write operations per month</span>

<span>Just like Rome wasn't built in a day, our infrastructure wasn't as well. This series of posts will explore the 15 instrumental steps we took when building our infrastructure. I will even discuss our outages and bugs in order to you to understand how we used them to improve our architecture.</span>

<span>The</span> [<span>first blog post</span>](http://highscalability.com/blog/2015/7/13/algolias-fury-road-to-a-worldwide-api.html) <span>of this series focused on our early days in beta and the</span> [<span>second post</span>](http://highscalability.com/blog/2015/7/20/algolias-fury-road-to-a-worldwide-api-steps-part-2.html) <span>on the first 18 months of the service, including our first outages. In this last post, I will describe how we transformed our "startup" architecture into something new that was able to meet the expectation of big public companies.</span>

## <span>Step 11: February 2015</span>

### <span>Launch of our Synchronized Worldwide infrastructure</span>

<span>During this month, we accomplished the vision we had been working towards since April 2014, “worldwide expansion to better serve our users”.</span>

<span>This network was composed of 12 different locations: US-East (Virginia), US-West (California), Australia, Brazil, Canada, France, Germany, Hong Kong, India, Japan, Russia and Singapore. On top of all that, we also launched our “Distributed Search” feature. With this feature, in a few clicks, you are able to set the locations in our network where your data should be automatically duplicated. You get to use the same API as before and queries are automatically routed from the end-user’s browser or mobile application to the closest location.</span>

<span>This was a huge step in reducing latency for end users and improving our high availability via worldwide distribution of searches. Serving international users from one location leads to a very different quality of service because of distance and saturation of Internet links. We now provide our users a way to solve that! On top of reducing latency, this improved the high availability of our search infrastructure because it was no longer dependent on a single location. It’s worldwide!</span>

<span>Some people compare our DSN (Distributed Search Network) to a CDN (Content Delivery Network), but it is very different. We don’t store a cache of your frequent queries on each edge. We store a complete replica of all your data. We can respond to any query from the edge location itself, not just the most popular. This means when you select three POPs (US-East, Germany, Singapore), users in Europe will go to the Germany location, users in Asia will go to Singapore, and users in America will go to US-East. All POPs will respond to queries without having to communicate to another edge. This makes a huge difference in terms of user experience and high availability.</span>

<span>In order to support this change, we updated our retry logic in our API clients to target the</span> [<span>APPID-dsn.algolia.net</span>](http://appid-dsn.algolia.net) <span>hostname first, which is routed to the closest location using DNS based on geoip. If the closest host is down, the DNS record is updated to remove the host in less than one minute in order to return the next closest host. This is why we use a low TTL of 1 minute on each record. In case of failure, if the host is down and DNS has not been updated, we redirect traffic to the master region via a retry on</span> [<span>APPID-1.algolia.net</span>](http://appid-1.algolia.net)<span>,</span> [<span>APPID-2.algolia.net</span>](http://appid-2.algolia.net) <span>and</span> [<span>APPID-3.algolia.net</span>](http://appid-3.algolia.net)<span>in our official API clients. This approach is the best balance we have found between high performance and high availability, we only have a degradation of performance during one minute in case of failure, but the API remains up & running!</span>

## <span>Step 12: March 201</span>

### <span>Better High Availability per location</span>

<span>The Distributed Search Network option was a game changer for high availability but it’s only for search and international users. In order to improve the high availability of the main region, we spread our US clusters across two completely independent providers:</span>

*   <span>Two different datacenters in close locations (Ex: COPT-6 in Manassas & Equinix DC-5 in Ashburn : 24 miles between them, 1ms of latency)</span>

*   <span>Three different machines - just like before (two in the first datacenter in different availability zones and one on the second datacenter)</span>

*   <span>Two different Autonomous Systems (so two totally different network providers)</span>

<span>These changes improved our ability to react to certain problems we faced like the saturation of a peering link between on provider and AWS. Having different providers gives us the option to reroute traffic to other providers. This is a big step in terms of high availability improvements in one location.</span>

## <span>Step 13: April 2015</span>

### <span>The Random file corruption headache</span>

<span>April 2015 was a black month for our production team. We started observing random file corruptions on our production machines due to a bug in the TRIM implementation of some of our SSDs. You can read the problem in detail in</span> [<span>this blog post</span>](https://blog.algolia.com/when-solid-state-drives-are-not-that-solid/)<span>. It took us approximately one month to track the issue and properly identify it. During this time, we suspected everything, starting with our own software! This was also a big test for our architecture. Having random file corruptions on disk is not something easily handled. Nor is it easy to inform our users that they needed to reindex their data because our disks were corrupted. Luckily, we never lost any data of our customers.</span>

<span>There are two important factors in our architecture that allowed us to face this situation without losing any data:</span>

*   <span>We store three replicates of the data. The probability of a random corruption of the same data on the three machines is almost zero (and it didn’t happen).</span>

*   <span>More importantly, we did not replicate the result of indexing. Instead, we duplicated the operation received from the user and applied it on each machine. This design makes the probability of having several machines having the same corruption very low since one affected machine cannot "contaminate" the others.</span>

<span>We never considered this scenario when we designed our architecture! Even though we tried to think about all types of network and hardware failure, we never imagined the consequences of a kernel bug and even worse a firmware bug. The fact that our design was to have independent machines is the reason we were able to minimize the impact of the problem. I highly recommend this kind of independence to any system that needs high availability.</span>

## <span>Step 14: May 2015</span>

### <span>Introducing several DNS providers</span>

<span>We decided to use NSONE as a DNS provider because of their great DNS API. We were able to configure how we wanted to route queries for each of our users via their API. We also really liked how they supported edns-client-subnets because it was key to having better accuracy for geo-routing. These features all made NSONE a great provider, but we couldn’t put ourselves at risk by having only one provider in our architecture.</span>

<span>The challenge was to introduce a second DNS provider without losing all the powerful features of NSONE. This meant having two DNS providers on the same domain was not an option.</span>

<span>We decided to use our retry strategy in our API clients to introduce the second DNS provider. All API clients first try to contact</span> [<span>APPID-dsn.algolia.net</span>](http://appid-dsn.algolia.net) <span>and if there are any problems, they would retry on a different domain, TLD, and provider. We decided to use AWS Route 53 as our second provider. If there were any problems, the API client would retry randomly on</span> [<span>APPID-1.algolianet.com</span>](http://appid-1.algolianet.com)<span>,</span> [<span>APPID-2.algolianet.com</span>](http://appid-2.algolianet.com) <span>and</span> [<span>APPID-3.algolianet.com</span>](http://appid-3.algolianet.com) <span>that targets the 3 machines of the main cluster. This approach allowed us to keep all interesting geo-routing features of NSONE on algolia.net domain while introducing a second provider for a better high availability on algolianet.com domain.</span>

## <span>Step 15: July 2015</span>

### <span>Three completely independent providers per cluster</span>

<span>You can now consider that with the infrastructure we developed, we are now fully resilient to any problem... but the reality is different. Even with services hosted by a Cloud provider with multiple Availability Zones you can have outages. We see two big reasons:</span>

*   <span>Link/Router starts producing packet loss. We have seen this multiple times between US-East and US-West, including a border ISP router of a big cloud provider while they were not even aware of it.</span>

*   <span>Route leaks. This particularly impacts the big players that a large portion of the Internet relies on.</span>

<span>Our improved setup in US now allows us to build clusters spanning multiple datacenters, multiple Autonomous systems and multiple upstream providers. That said, in order to accept indexing operations, we need to have a majority of machines up, which means two machines out of three. When using two providers, If one of our providers goes down, we could potentially lose the indexing service although search would still be available. That’s why we decided to go even further with the hosting of clusters across three completely independent providers (different datacenters in locations close to each other that rely on different upstream providers and Autonomous systems). This setup allowed us to provide high availability of search and indexing via an ultra redundant infrastructure. And we provide all of that along with the same easy to use API!</span>

## <span>Building a highly available architecture takes time</span>

<span>I hope the details of our journey were inspiring and provided useful details about how we started and where we are today. If you are a startup, don’t be worried if you don’t have the perfect infrastructure in the beginning. This is to be expected! But you should think about your architecture and how to scale it early on. I would even recommend doing it before beta!</span>

Having the **architecture designed early was our secret weapon**. It allowed us to focus on execution as soon as our market fit was there. Even today, we have some features on scalability/availability that were designed very early on that are still not yet implemented. But they will be coming in the near future for sure :) 

_Here are all three parts of the series: [Part 1](http://highscalability.com/blog/2015/7/13/algolias-fury-road-to-a-worldwide-api.html), [Part 2](http://highscalability.com/blog/2015/7/20/algolias-fury-road-to-a-worldwide-api-steps-part-2.html), [Part 3](http://highscalability.com/blog/2015/7/27/algolias-fury-road-to-a-worldwide-api-part-3.html)_

_[On HackerNews](https://news.ycombinator.com/item?id=9956097)_

</div>