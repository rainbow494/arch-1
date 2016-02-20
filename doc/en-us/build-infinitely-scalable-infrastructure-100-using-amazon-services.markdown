## [Build an Infinitely Scalable Infrastructure for $100 Using Amazon Services](/blog/2007/7/30/build-an-infinitely-scalable-infrastructure-for-100-using-am.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, July 30, 2007 at 8:11AM</span></div>

<div class="body">

Can you really create an infinitely scalable infrastructure for less than $100 using Amazon's storage, grid, and queuing services platform? It appears so, at least for the right application. Amazon beams a spot light on the future battle of the roll-your-own versus the connect-the-dots approach to building next generation websites using core external services. Their argument is strong. Using Amazon's platform you can quickly build an infrastructure that would otherwise take an eternity to make, a pile of money to create, and an unbounded mass of people to implement and maintain. Yet Amazon doesn't provide SLAs, so you can you really trust them with your crown jewels? Facebook recently leap frogged Amazon's vision with an even more comprehensive set of services. The battle for the future is on.

Site: http://aws.amazon.com/

## Information Sources

*   [Slides: Building Highly Scalable Web Applications](http://www.slideshare.net/iwmw/building-highly-scalable-web-applications)*   [Podcast: Technometria: Amazon Web Services](http://www.itconversations.com/shows/detail1728.html)*   [Amazon Services Home](http://aws.amazon.com/).  

    ## Platform

    *   Amazon ECS (E-Commerce Service)*   Amazon S3 (simple storage service)*   Amazon SQS (simple queuing service)*   Amazon EC2 (grid service)*   Amazon Web Search Service*   Amazon Flexible Payments Service (Amazon FPS)*   REST and SOAP Service Interfaces  

    ## What's Inside?

    ### Why use external services?

    *   Amazon's services replace the boxes, wires, and disk drives part of the application stack.*   Amazon has spent ten years and over $1 billion developing a world-class web service that millions of customers use every day. Maybe you can leverage that experience for your site?*   Focus on the customer. 70% if Web Development isn't about providing customer value. It's about building and managing data centers. Your efforts would be better spent on your customers and not plumbing.*   Quicker to market. Scaling is hard. Let someone else worry  
    about that while you concentrate on adding user value.*   Designing for peak load is expensive. So turn fixed costs into variable costs. Say you want to handle high traffic flows from slashdot or digg, or you have high seasonal demand, having the infrastructure in place to handle those loads is a high fixed cost. You could use that money better elsewhere. It make sense to create an infrastructure where you can automatically and  
    temporarily scale resources to handle peak demand.*   High reliability and availability. A dedicated service may be more reliable than a service you could create. It say "may" because Amazon doesn't provide an SLA, so you wont get any guarantees. The idea is that Amazon is cheap enough and reliable enough that the few failures will be acceptable. Besides, SLAs usually just refund some money when things go wrong, they  
    don't really guarantee anything.*   It's a cheap CDN. Amazon's storage network could serve a relatively inexpensive content delivery network. This option is discussed in[  
    Reducing Your Website's Bandwidth Usage](http://www.codinghorror.com/blog/archives/000807.html). The idea is that just the frequent downloading of a simple [favicon.ico](http://www.hanselman.com/blog/FavIconicoCanBeABandwidthHog.aspx)  
    file can use a significant portion of your bandwidth. Using S3 for $2/month to offload 90% of your bandwidth to an external host is a good deal. However, without an SLA S3 can't be thought of as a proper CDN.  

    ### Amazon ECS (E-Commerce Service)

    *   This service exposes Amazon's product data and e-commerce functionality:  
    Detailed Product Information on all Amazon.com Products, Access to Product Images, All Customer Reviews associated with a Product, etc.*   Amazon products are aggressively priced.*   I found this service disappointing. If you want to build a store on top of Amazon it seems great, but I didn't see a way to add your own products to the store, so I don't think it's generally useful.  

    ### Amazon S3 (simple storage service)

    *   This service stores data in Amazon's storage network.*   $.15 per GPB per month storage*   $.01 for 1000 to 10000 requests.*   $.10 - $.17 per GB data transfer.*   The service is: fast, relaible, scalable, redundant, dispersed.*   You can have per object URLs. This means you can reference an image or other file directly with a URL, so it's usable in a web page.*   Typical use: CDN and backup storage.*   Storage is distributed to multiple locations so you get a level of geographical distribution.  

    ### Amazon SQS (simple queuing service)

    *   This service provides an internet scale queuing service for storing messages. Distributed actors put work on the queue and take work off the queue.*   $.10 per 1000 messages.*   $.10 - $.18 per GB data transfer.*   This service is: scalable, elastic, reliable, simple, secure.*   Typical use: a centralized work queue. You put jobs on the queue and different actors can pop work of the queue and process them when they get CPU time.*   Expected message latency, as of 2007, was 2-10 seconds. This is horrible for many applications, not bad for many others.*   Part of scalability. Have any number of producers and consumers. You don't worry about it.*   Queues are spread across multiple machines and multiple data centers.  

    ### Amazon EC2 (grid service)

    *   This service provides resizable compute capacity in the cloud. It is designed to make web-scale computing easier for developers.*   Basically you create a Xen image for your Linux distro and upload it into their "elastic compute cloud." Using an API you can then start as many instances as you like.*   Typical use: transcoding, audio work, load testing.*   Root level access to the server and full control over the machine.*   Can scale up and scale down on a minute-by-minute basis.*   For real-time processing one criticism has been slow CPUs (1.75 Ghz Xeon). This probably won't be a problem if your application is written to linearly scale.*   An EC2 instance is not persistent so you can't store a database there. You have some local storage, but it goes away when the instance goes away.*   Takes a few minutes to start and stop images, so it's not really on demand.*   You can add anything you want to an image. If you want a database you can add it in.  

    ### GigaVox Media Example Web-Scale Architecture

    *   You can start to see how Amazon's services can work together. Let's say you have a large batch of MP2s you would like transcode to MP3s. You would store the original media into S3, queue the work request into SQS, and have instances running in EC2 to take work of the queue and perform the transcoding, storing the results back into S3\. And this is exactly what GigaVox does.*   GigaVox is a podcasting company.  
    - They take original recordings and transcode them say from MP2 to MP3\. Many other transcodings are also performed.  
    - Then these chunks of media are assembled together into a delivery format based on building a show. For example, old podcasts can be reassembled each night with up to date advertisements.  
    - To do this at scale would take a lot of costly resources.*   Using Amazon's services GigaVox gets geographically redundancy and failover for relatively inexpensive CPU, bandwidth, and storage charges,  
    and bandwidth costs. You have no boxes or wires. No data center to manage. And you can grow with small fixed costs.*   Messages are time stamped on the queue. If the message waited in the queue for too long then they can start more EC2 images. You can balance costs. You could also layer in a customer based priority mechanism.*   They have each instance have its own messaging queue for command and control.*   For security reasons they upload files through ftp to instances rather than going through S3.*   All bandwidth withing the Amazon cloud is free. This is an important business consideration for making the services work together.*   Another set of instances and queues handles assembling the delivered media.*   Allows GigaVox to deliver value to their customer at a low startup cost.  

    ## Lessons Learned

    *   Build or buy is always a difficult decision. If a service doesn't work then you may lose your customers and there's nothing more you can do other send yet another urgent email to nobody in particular. This is a horrible feeling. Yet, if it does work you could be way ahead of the game. How to choose? That would be telling :-)  

    *   Build a layer of virtualization so you can switch to another provider when they become available or so you can replace it with your own service. This lessens your dependency on Amazon in the event they get tired of offering services or their performance deteriorates.  

    *   As a startup using Amazon services isn't a big risk because you are already in a risky situation. And any risk is moderated by the very low cost of starting up and money is always an issue for startups.  

    *   For many use cases buying your own dedicated servers may still be a better approach as you get more control, lower latency, and the same hardware is usable for multiple purposes.  

    *   Software as a service is a powerful and practical idea. It changes how you build software. It forces you to layer your software around interfaces. And once your software is composed of interfaces you have loosely coupled components that can be easily replaced. You also have the basis for a platform API should you ever want to provide an API you your customers. The highest level of development would to use the same API you give your customers to build your service.  

    *   Loosely coupled, message based architectures combined with service interfaces allow you to think several levels up the abstraction layer. You don't have to wallow in the muck, which frees you how to structure your application using large scale blocks of behavior.  

    *   Designing a UI for an asynchronous interactive interface poses some challenges. It may take a while to perform an operation, so how do you interact with the user to handle that?  

    *   Instinctively I doubted Amazon could deliver. But if you have the right type of problem, you really can do a lot of work cheaply using Amazon services.  

    ## See Also

    *   [Flickr](http://highscalability.com/flickr-architecture) and [YouTube](http://highscalability.com/youtube-architecture) also deal with service level APIs.  

    *   [Running Hadoop MapReduce on Amazon EC2 and Amazon S3](http://developer.amazonwebservices.com/connect/entry.jspa?externalID=873&categoryID=112)</div>