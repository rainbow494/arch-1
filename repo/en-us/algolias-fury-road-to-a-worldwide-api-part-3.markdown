## [Algolia's Fury Road to a Worldwide API Part 3](/blog/2015/7/27/algolias-fury-road-to-a-worldwide-api-part-3.html)

    

    

        

    ![](https://lh6.googleusercontent.com/PaQgtoUcXRwy_agDKkBD1PBtZnoU4wudmeTkM46UBSX5d4TfiITy5dxV0549de06XvKELOmPY-fsWjUZmadyiyfVUtGs51qKOt7J9z0_RvCku-m_tlwlGL49udAOotuVtJtAEg)    

    The most frequent questions we answer for developers and devops are about     [    our architecture    ](http://highscalability.com/blog/2015/3/9/the-architecture-of-algolias-distributed-search-network.html)     and how we achieve such high availability. Some of them are very skeptical about high availability with bare metal servers, while others are skeptical about how we distribute data worldwide. However, the question I prefer is “How is it possible for a startup to build an infrastructure like this”. It is true that our current architecture is impressive for a young company:    

*       Our high-end dedicated machines are hosted in 13 worldwide regions with 25 data-centers    

*       our master-master setup replicates our search engine on at least 3 different machines    

*       we process over 6 billion queries per month    

*       we receive and handle over 20 billion write operations per month    

    Just like Rome wasn't built in a day, our infrastructure wasn't as well. This series of posts will explore the 15 instrumental steps we took when building our infrastructure. I will even discuss our outages and bugs in order to you to understand how we used them to improve our architecture.    

    The     [    first blog post    ](http://highscalability.com/blog/2015/7/13/algolias-fury-road-to-a-worldwide-api.html)     of this series focused on our early days in beta and the     [    second post    ](http://highscalability.com/blog/2015/7/20/algolias-fury-road-to-a-worldwide-api-steps-part-2.html)     on the first 18 months of the service, including our first outages. In this last post, I will describe how we transformed our "startup" architecture into something new that was able to meet the expectation of big public companies.    

##     Step 11: February 2015    

###     Launch of our Synchronized Worldwide infrastructure    

    During this month, we accomplished the vision we had been working towards since April 2014, “worldwide expansion to better serve our users”.    

    This network was composed of 12 different locations: US-East (Virginia), US-West (California), Australia, Brazil, Canada, France, Germany, Hong Kong, India, Japan, Russia and Singapore. On top of all that, we also launched our “Distributed Search” feature. With this feature, in a few clicks, you are able to set the locations in our network where your data should be automatically duplicated. You get to use the same API as before and queries are automatically routed from the end-user’s browser or mobile application to the closest location.    

    This was a huge step in reducing latency for end users and improving our high availability via worldwide distribution of searches. Serving international users from one location leads to a very different quality of service because of distance and saturation of Internet links. We now provide our users a way to solve that! On top of reducing latency, this improved the high availability of our search infrastructure because it was no longer dependent on a single location. It’s worldwide!    

    Some people compare our DSN (Distributed Search Network) to a CDN (Content Delivery Network), but it is very different. We don’t store a cache of your frequent queries on each edge. We store a complete replica of all your data. We can respond to any query from the edge location itself, not just the most popular. This means when you select three POPs (US-East, Germany, Singapore), users in Europe will go to the Germany location, users in Asia will go to Singapore, and users in America will go to US-East. All POPs will respond to queries without having to communicate to another edge. This makes a huge difference in terms of user experience and high availability.    

    In order to support this change, we updated our retry logic in our API clients to target the     [    APPID-dsn.algolia.net    ](http://appid-dsn.algolia.net)     hostname first, which is routed to the closest location using DNS based on geoip. If the closest host is down, the DNS record is updated to remove the host in less than one minute in order to return the next closest host. This is why we use a low TTL of 1 minute on each record. In case of failure, if the host is down and DNS has not been updated, we redirect traffic to the master region via a retry on     [    APPID-1.algolia.net    ](http://appid-1.algolia.net)    ,     [    APPID-2.algolia.net    ](http://appid-2.algolia.net)     and     [    APPID-3.algolia.net    ](http://appid-3.algolia.net)    in our official API clients. This approach is the best balance we have found between high performance and high availability, we only have a degradation of performance during one minute in case of failure, but the API remains up & running!    

##     Step 12: March 201    

###     Better High Availability per location    

    The Distributed Search Network option was a game changer for high availability but it’s only for search and international users. In order to improve the high availability of the main region, we spread our US clusters across two completely independent providers:    

*       Two different datacenters in close locations (Ex: COPT-6 in Manassas & Equinix DC-5 in Ashburn : 24 miles between them, 1ms of latency)    

*       Three different machines - just like before (two in the first datacenter in different availability zones and one on the second datacenter)    

*       Two different Autonomous Systems (so two totally different network providers)    

    These changes improved our ability to react to certain problems we faced like the saturation of a peering link between on provider and AWS. Having different providers gives us the option to reroute traffic to other providers. This is a big step in terms of high availability improvements in one location.    

##     Step 13: April 2015    

###     The Random file corruption headache    

    April 2015 was a black month for our production team. We started observing random file corruptions on our production machines due to a bug in the TRIM implementation of some of our SSDs. You can read the problem in detail in     [    this blog post    ](https://blog.algolia.com/when-solid-state-drives-are-not-that-solid/)    . It took us approximately one month to track the issue and properly identify it. During this time, we suspected everything, starting with our own software! This was also a big test for our architecture. Having random file corruptions on disk is not something easily handled. Nor is it easy to inform our users that they needed to reindex their data because our disks were corrupted. Luckily, we never lost any data of our customers.    

    There are two important factors in our architecture that allowed us to face this situation without losing any data:    

*       We store three replicates of the data. The probability of a random corruption of the same data on the three machines is almost zero (and it didn’t happen).    

*       More importantly, we did not replicate the result of indexing. Instead, we duplicated the operation received from the user and applied it on each machine. This design makes the probability of having several machines having the same corruption very low since one affected machine cannot "contaminate" the others.    

    We never considered this scenario when we designed our architecture! Even though we tried to think about all types of network and hardware failure, we never imagined the consequences of a kernel bug and even worse a firmware bug. The fact that our design was to have independent machines is the reason we were able to minimize the impact of the problem. I highly recommend this kind of independence to any system that needs high availability.    

##     Step 14: May 2015    

###     Introducing several DNS providers    

    We decided to use NSONE as a DNS provider because of their great DNS API. We were able to configure how we wanted to route queries for each of our users via their API. We also really liked how they supported edns-client-subnets because it was key to having better accuracy for geo-routing. These features all made NSONE a great provider, but we couldn’t put ourselves at risk by having only one provider in our architecture.    

    The challenge was to introduce a second DNS provider without losing all the powerful features of NSONE. This meant having two DNS providers on the same domain was not an option.    

    We decided to use our retry strategy in our API clients to introduce the second DNS provider. All API clients first try to contact     [    APPID-dsn.algolia.net    ](http://appid-dsn.algolia.net)     and if there are any problems, they would retry on a different domain, TLD, and provider. We decided to use AWS Route 53 as our second provider. If there were any problems, the API client would retry randomly on     [    APPID-1.algolianet.com    ](http://appid-1.algolianet.com)    ,     [    APPID-2.algolianet.com    ](http://appid-2.algolianet.com)     and     [    APPID-3.algolianet.com    ](http://appid-3.algolianet.com)     that targets the 3 machines of the main cluster. This approach allowed us to keep all interesting geo-routing features of NSONE on algolia.net domain while introducing a second provider for a better high availability on algolianet.com domain.    

##     Step 15: July 2015    

###     Three completely independent providers per cluster    

    You can now consider that with the infrastructure we developed, we are now fully resilient to any problem... but the reality is different. Even with services hosted by a Cloud provider with multiple Availability Zones you can have outages. We see two big reasons:    

*       Link/Router starts producing packet loss. We have seen this multiple times between US-East and US-West, including a border ISP router of a big cloud provider while they were not even aware of it.    

*       Route leaks. This particularly impacts the big players that a large portion of the Internet relies on.    

    Our improved setup in US now allows us to build clusters spanning multiple datacenters, multiple Autonomous systems and multiple upstream providers. That said, in order to accept indexing operations, we need to have a majority of machines up, which means two machines out of three. When using two providers, If one of our providers goes down, we could potentially lose the indexing service although search would still be available. That’s why we decided to go even further with the hosting of clusters across three completely independent providers (different datacenters in locations close to each other that rely on different upstream providers and Autonomous systems). This setup allowed us to provide high availability of search and indexing via an ultra redundant infrastructure. And we provide all of that along with the same easy to use API!    

##     Building a highly available architecture takes time    

    I hope the details of our journey were inspiring and provided useful details about how we started and where we are today. If you are a startup, don’t be worried if you don’t have the perfect infrastructure in the beginning. This is to be expected! But you should think about your architecture and how to scale it early on. I would even recommend doing it before beta!    

Having the **architecture designed early was our secret weapon**. It allowed us to focus on execution as soon as our market fit was there. Even today, we have some features on scalability/availability that were designed very early on that are still not yet implemented. But they will be coming in the near future for sure :) 

_Here are all three parts of the series: [Part 1](http://highscalability.com/blog/2015/7/13/algolias-fury-road-to-a-worldwide-api.html), [Part 2](http://highscalability.com/blog/2015/7/20/algolias-fury-road-to-a-worldwide-api-steps-part-2.html), [Part 3](http://highscalability.com/blog/2015/7/27/algolias-fury-road-to-a-worldwide-api-part-3.html)_

_[On HackerNews](https://news.ycombinator.com/item?id=9956097)_

    