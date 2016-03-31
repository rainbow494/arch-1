## [Algolia's Fury Road to a Worldwide API](/blog/2015/7/13/algolias-fury-road-to-a-worldwide-api.html)

    

    

        ![](https://lh6.googleusercontent.com/8SVezgJy_d5WxiE5jwPKWUp3-0rzca-u-h6DU319iK4Uqe2g9IOqGMMwV9fOMUucBuCpOp6_dGn-SUd8T2-idMKIzRRQckPV2-S1SJeym0JjHwr_1sCg6zH_W_IEMbYSi_0J6Q)        

_Guest post by [Julien Lemoine](https://www.linkedin.com/in/julienlemoine), co-founder & CTO of [Algolia](http://www.algolia.com/), a developer friendly search as a service API._

    The most frequent questions we answer for developers and devops are about     [    our architecture    ](http://highscalability.com/blog/2015/3/9/the-architecture-of-algolias-distributed-search-network.html)     and how we achieve such high availability. Some of them are very skeptical about high availability with bare metal servers, while others are skeptical about how we distribute data worldwide. However, the question I prefer is “How is it possible for a startup to build an infrastructure like this”. It is true that our current architecture is impressive for a young company:    

*       Our high-end dedicated machines are hosted in 13 worldwide regions with 25 data-centers    

*       our master-master setup replicates our search engine on at least 3 different machines    

*       we process over 6 billion queries per month    

*       we receive and handle over 20 billion write operations per month    

    Just like Rome wasn't build in a day, our infrastructure wasn't as well. This series of posts will explore the 15 instrumental steps we took when building our infrastructure. I will even discuss our outages and bugs in order to you to understand how we used them to improve our architecture.    

    This first part will focus on the first three first steps we took when building the service while in beta from March 2013 to August 2013.    

#     The Cloud versus Bare metal debate    

    Before diving into the details of our architectural journey, I would like to address a choice that had a big consequences on the rest of the infrastructure. We needed to decide if we should use  a cloud-based infrastructure or bare metal machines. A hot topic that is regularly debated in technical discussions.    

    Cloud infrastructure is a great solution for most use cases, especially in the early stages. They have been instrumental in improving the high-availability of many services. Solutions having databases on multiple availability zones (AZ) or several instances running on different AZ while storing all their state in the multiple AZ database are perfect example of this. This is a standard setup used by many engineers and easily deployed in a few minutes.    

    Bare metal infrastructures require you to understand and design the small details in order to build the high availability yourself. This is a do-it-yourself approach that only makes sense for a small set of use cases. We often encounter deployments using bare metal machines in a single datacenter. This does not make sense as it is less fault tolerant than a quick deployment on a cloud provider, the datacenter is a single point of failure (SPoF).    

    Bare metal hardware remains an interesting option for businesses linked to hardware, which happens to be our case. By choosing a bare metal infrastructure, we were able to purchase higher performance hardware than that offered by cloud providers. On top of the performance gain the cost was significantly cheaper. We chose this option early on because we were fully aware we would need to build the high-availability ourself!    

#     The early days: March-August 2013    

##     Step1: March 2013    

    High availability was designed, not implemented!    

    At this time we had our first private beta running of our search as a service API. At this point in time, we could only measure our performance. We had not yet developed the high availability part of the product. We were highly confident our market would be worldwide so we launched a single machine in two different locations, Canada/East and Europe/West, with the following specifications:    

*       32G of memory    

*       Xeon E3-1245 v2 (4 cores, 8 threads, 3.4Ghz-3.8Ghz)    

*       2x Intel SSD 320 series of 120GB in Raid-0    

    Each machine was hosting a different set of users depending on their location. During our private beta, the focus was 100% on performance which is why the clock speed was a major factor in our decisions (for the same generation CPU, clock speed is directly related to speed of a search query in search engines). Since the beginning, we have done indexing on a separate process with a nice level of five. All search queries were processed directly inside nginx that we let with a nice level of zero (the lower the nice level of a process is, the more CPU time it gets). This setup allowed us to handle traffic spike efficiently by giving the search the highest allocation CPU priority. This worked very well compared to approaches used by other engines.    

    We were very surprised to have one of our first beta testers replace their previous solution with ours in production because they were so happy with the performance and relevance. As you might imagine, we were very stressed out about this. Since high availability was not implemented, we were worried about potential downtime affecting them and explained the product was not ready production! The customer told us the risk versus rewards was acceptable to them because they could rollback to their previous provider if needed. On a side note, this story helped us secure our first round of funding before the launch of the product. It ended up being our first proof of market fit. Better yet, we can call it our problem-solution fit! We can't thank that customer enough :)    

##     Step 2: June 2013    

    Implementation of high availability in our architecture    

    After three months of development and a huge amount of testing (the monkey testing approach was really fun!), we introduced the high availability support in our beta. You can read more about it in our     [    architecture post    ](http://highscalability.com/blog/2015/3/9/the-architecture-of-algolias-distributed-search-network.html)    . The idea was to have a cluster of three identical machines instead of one where each machine is a perfect replica with all of the data and able to acts as a master. This means that each one is capable of accepting write operations from API users. Each write operation triggers a consensus to make sure all machines have all the jobs and apply them in the same order.    

    We used the preliminary results from the first beta to design our new hardware setup. We discovered the following:    

*       32G of memory was not enough, indexing was using up to 10G when receiving big indexing jobs from several users, which only let 22G to cache disk IO    

*       Disk space was too low for high availability since machines needed to keep several jobs on disk in order to handle a node failure    

*       Having more memory required us to move to Xeon E5 series (E3 can only address 32G of memory). As the clock speed was important, we decided to go for the Xeon E5 1600 series that offered a very good clock speed and is able to address a lot more memory that the Xeon E3.    

    With these discoveries, our setup evolved into three machines with the following specs:    

*       64G of memory    

*       Xeon E5-1650 (6 cores, 12 threads, 3.2Ghz to 3.8Ghz)    

*       2x Intel SSD 320 series of 300GB in Raid-0    

    At this point we were able to tolerate hardware failures! However, we were still far from what cloud providers offer with multiple availability zones. All our machines were in the same datacenter with a single provider and without any knowledge of the infrastructure.    

    At the same time, we investigated whether or not the load balancing and detection failure between machines should be handled with hardware or software. We tested several approaches and found all hardware load balancers would make the use of several providers close to impossible. We ended up implementing a basic retry strategy in our API clients. Each API client was developed to be able to access three different machines. Three different DNS records represented each user:     [    USERIDID-1.algolia.io    ](http://useridid-1.algolia.io)    ,     [    USERID-2.algolia.io    ](http://userid-2.algolia.io)     and    [    USERID-3.algolia.io    ](http://userid-3.algolia.io)    . Our first implementation was to randomly select one of the records and then retry with a different one in case of failure.    

##     Step 3: August 2013    

    Official launch of the service    

    During the summer, we increased the number of API clients to 10 (JS, Ruby, Python, PHP, Objective-C, Java, C#, Node.js...). We decided to avoid using auto code generation and developed the API clients manually. While it was more work, we needed to make sure the networking code was good for things like HTTPS keep alive, using TLS correctly, having our retry strategy implemented correctly with the correct timeout, etc.    

    We officially launched the service at the end of August 2013 with our two locations (Europe/West and Canada/East). Each location contained a cluster of three identical hosts with the following specs:    

*       128G RAM    

*       E5-2687W (8 cores, 16 threads, 3.1Ghz to 3.8Ghz)    

*       2x Intel S3500 series of 300GB in Raid-0    

    Compared to the previous configuration, the main things we changed were to increase the memory size and use a better SSD. Those two changes were done based on the observation that the SSD was the bottleneck during indexing and the memory was not enough to cache all the users’ data in memory. For the CPU upgrade, it was more a question of oversizing to ensure we would have enough resources.    

    At this point, the next big item for us to focus on was implementing availability zones for our deployments. We needed to run our three machines on different network equipment and power units. Hopefully our provider was transparent about their infrastructure and where our machines were allocated. It wasn’t perfect, but we were able to implement a solution similar to those of the different cloud providers. We suspect the cloud providers do something similar to what we implemented, but have not found any detailed documentation on this topic!    

###     Next    

    Like most other startups, we started with a rough MVP to test the market. We ended up having to do some serious work developing a more mature and robust architecture. With these first few steps, we move from a MVP to a production ready API.    

    So far, we have covered 3 of the 15 steps in this blog series. In the next blog, you will learn about the first 18 months in production and all the unexpected problems we faced including the first outages!    

_Here are all three parts of the series: [Part 1](http://highscalability.com/blog/2015/7/13/algolias-fury-road-to-a-worldwide-api.html), [Part 2](http://highscalability.com/blog/2015/7/20/algolias-fury-road-to-a-worldwide-api-steps-part-2.html), [Part 3](http://highscalability.com/blog/2015/7/27/algolias-fury-road-to-a-worldwide-api-part-3.html)_

##     Related Articles    

*   [On HackerNews](https://news.ycombinator.com/item?id=9899794)

    