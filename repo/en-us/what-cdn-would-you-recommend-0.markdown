## [What CDN would you recommend?](/blog/2009/4/21/what-cdn-would-you-recommend.html)

    

    **Update 10:** [The Value of CDNs](http://www.afnog.org/afnog2008/conference/talks/Google-AFNOG-presentation-public.pdf) by Mike Axelrod of Google. Google implements a distributed content cache from within [large ISPs](http://www.lacnic.net/documentos/lacnicxi/presentaciones/Google-LACNIC-final-short.pdf). This allows them to serve content from the edge of the network and save bandwidth on the ISPs backbone.  
**Update 9:** [Just Jump: Start using Clouds and CDNs](http://www.rockstarapps.com/wordpress/?p=138#). Bob Buffone gives a really nice and practical tutorial of how to use CloudFront as your CDN.  

**Update 8:** [Akamai’s Services Become Affordable for Anyone! Blazing Web Site Performance by Distribution Cloud](http://www.thebitsource.com/2009/02/07/akamai%E2%80%99s-services-become-affordable-for-anyone-blazing-fast-web-site-performance-with-distribution-cloud/#). _Distribution Cloud starts at $150 per month for access to the best content distribution network in the world and the leader of Content Distribution Networks._  
**Update 7:** [Where Amazon’s Data Centers Are Located](http://www.datacenterknowledge.com/archives/2008/11/18/where-amazons-data-centers-are-located/), [Expanding the Cloud: Amazon CloudFront](http://www.allthingsdistributed.com/2008/11/amazon_cloudfront.html). [Why Amazon's CDN Offering Is No Threat To Akamai, Limelight or CDN Pricing](http://blog.streamingmedia.com/the_business_of_online_vi/2008/11/amazons-new-cdn-offering-does-not-challenge-akamai-and-limelight.html). Amazon has launched their CDN with "“low latency, high data transfer speeds, and no commitments.” The perfect relationship for many. The majority of the locations are in North America, but some are in Europe and Asia.  
**Update 6:** [Amazon Launching New Content Delivery Network: No Threat To Major CDNs, Yet](http://blog.streamingmedia.com/the_business_of_online_vi/2008/09/amazon-to-launc.html). All the Amazon will kill all other CDNs is a bit overblown. As usual Dan Rayburn sets us straight: _The offering won't support streaming, live broadcasting, or provide many of the other products and services that video content owners need...the real story here is that Amazon is going to offer a high performance method of distributing content with low latency and high data transfer rates._  
**Update 5:** [When It Comes To Content Delivery Networks, What Is The "Edge"?](http://blog.streamingmedia.com/the_business_of_online_vi/2008/09/when-it-comes-t.html). Dan Rayburn is on edge about the misuse of the term _edge_: closest location to the user does not guarantee quality, often content is not delivered from the closest location, all content is not replicated at every "edge" location. Lots of other essential information.  
_Update 4: David Cancel runs a great test to see if you should be [Using Amazon S3 as a CDN?](http://davidcancel.com/2008/05/29/using-amazon-s3-as-a-cdn/). Conclusion: "CacheFly performed the best but only slightly better than EdgeCast. The S3 option was the worst with the Nginx/DIY option performing just over 100 ms faster." Also take look at [Part 2 - Cacheability?](http://davidcancel.com/2008/06/04/using-amazon-s3-as-cdn-part-2-cacheability/)_  
_Update 3: Mr. Rayburn takes [A Detailed Look At Akamai's Application Delivery Product](http://blog.streamingmedia.com/the_business_of_online_vi/2008/04/an-detailed-loo.html) . They create a "bi-nodal overlay network" where users and servers are always within 5 to 10 milliseconds of each other. Your data center hosted app can't compete. The problem is that people (that is, me) can understand the data center model. I don't yet understand how applications as a CDN will work._  
_Update 2: Dan Rayburn starts an interesting series of articles on [Highlights Of My Day In Cambridge With Akamai](http://blog.streamingmedia.com/the_business_of_online_vi/2008/04/recap-of-my-day.html). Akamai is moving strong into the application distribution business. That would make an interesting cloud alternative.._  
_Update: [Streamingmedia](http://blog.streamingmedia.com/the_business_of_online_vi/2008/04/digital-fountai.html) links to new CDN [DF Splash](http://www.dfsplash.com) that specializes in instant-on TV-quality video streaming._  

A question was raised on the forum asking for a CDN recommendation. As usual there are no definitive answers, but here are three useful articles that may help your deliberations.  
*   First, Tony Chang shows how to drive down response times using edge acceleration strategies.  
    *   Then Pingdom gives a nice overview and introduction to CDNs.  
    *   And last but not least, Dan Rayburn from StreamingMedia.com gives a master class in how much you should pay for your CDN, what you should be getting for your money, and how to find the right provider for your needs.  

    Lots and lots of good stuff to learn, even if you didn't roll out of bed this morning pondering the deeper mysteries of content delivery networks and the Canadian dollar.  

    ### [Edge Acceleration Strategies: Akamai](http://www.winnersdontlose.com/2008/02/application-acceleration-strat.html) by Tony Chang

    The edge network is the "network physically closest to the end user and the 'origin' is where the application(s) is hosted." Tony talks about how you use CDNs to manage the user experience through meeting millisecond+ level SLAs using edge acceleration services. He does this in an interesting way. He follows a request through its life cycle and shows how to turn your caterpillar into a butterfly at each stage:  

    *   An edge DNS means a name server closest to the end user will serve the DNS request.  
    *   Static content is easily cached on the edge.  
    *   Dynamic content is moving to the edge using what Akamai calls Web Application Accelerators.  
    *   And something I've never heard of is to use your CDN to improve routing performance by up to 33%. The service bypasses BGP using its own more optimized route tables to decrease latency.  

    ### Pingdom's [A look at Content Delivery Networks, or “how to serve lots of content really fast”](http://royal.pingdom.com/?p=249)

    CDNs are the hidden powerhouse of the internet. The unsung mitochondria powering bits forward. Cost, convenience and performance are the reasons people turn to CDNs. A CDN does what you can't, it put lots of servers in lots of different places. Panther Express, for example, puts 800 servers in 22 different geographical locations. Since CDNs sell delivery capacity capacity planning is one of their big challenges. And Pingdom would like you to recognize the importance of monitoring for detecting and quickly reacting to problems :-) The future of CDNs lies in larger caches for HD video, better locality, and more integration with hosting providers.  

    ### Video on Content [Delivery Network Pricing, Costs for Outsourced Video Delivery](http://www.scribemedia.org/2007/12/12/cdn-pricing-video/) by Dan Rayburn

    Also [CDN Pricing Data: Average Cost Per GB Declines In Q4 Due To Startups](http://blog.streamingmedia.com/the_business_of_online_vi/2007/11/cdn-pricing-dat.html).  

    It's evident Dan really knows his stuff. His articles and presentations are highly educational for anyone interested in the complex and confusing CDN world. Dan sees hundreds of real-life customer-CDN vendor contracts a year and he reports on real prices averaged over all the contracts he has seen. One of the hardest things as a consumer is knowing what a good price is for your basket of goods and Dan gives you the edge, so to speak.  

    What I learned:  
    *   You decide who is a CDN.There's no central agency setting a standard. Dan's minimal definition is a service delivering live video in the US and Europe.  
    *   CDN market has gone from 10 to 30 vendors. VCs are pumping hundreds of millions into the space.  
    *   CDN providers provide a wide variety of services: application caching, static caching, streaming video, progressive video, etc. Dan concentrates only on video delivery.  
    *   You can't say vendor A is better than vendor B. It depends on your needs.  
    *   When comparing vendors you need to do an "apples to apples" comparison. He really likes that phrase. You can't compare vendors, only like products between vendors.  
    *   Video serving is complex because there are few standards in the market. There are multiple platforms, multiple encoding standards, etc.  
    *   Customer's don't buy on price alone. Delivery of bits over a network is a commodity. Buy on SLA, customer service, product, format, support, geographic reach, and performance.  
    *   There appears to be no way to compare vendors on the performance of their network. There are too many variables in play to make an accurate comparison.He's quite adament about this. Performance could mean: SLA, customer service, upload content, buffering, etc. No way to measure performance network performance across networks. Static image performance is very different than streaming performance. People all over the globe are accessing your content so what is the "performance" of that?  
    *   A trend this year is demand for P2P pricing and services.  
    *   To price your video delivery you need to answer 4 questions: 1) How many hours? 2) How many users? 3) How long will they watch? 4) What encoding and what bit rate?  
    *   Price varies on product bundle. Vendors need to specialize so they can move themselves out of the commodity market. If you would pay 8 cents a gig for delivered video your price will be different if you add application and static caching.  
    *   Contracts are at 12 months. Maybe 2 years when bundling services.  
    *   P2P is not necessarily cheaper so compare. Pricing is very confusing.  
    *   You can sometimes get a lower price by using the vendor's player.  
    *   Flash streaming is more expensive because of licensing fees. The number varies because each vendor cuts their own licensing deals. Could be 20% more, or it could be double, depends on volume.  
    *   When signing a vendor think if you need global reach or is regional reach sufficient? Use a regional service provider if you need a CDN just once in a while. It's matter of picking based on your needs. You can often get a less expensive deal and get a quarterly commit versus a montly commit.  
    *   Storage costs have really fallen. High of $10/gig and low of 10 cents per gig.  
    *   Most CDNs will pull from your origin storage and cache, which reduces your storage cost.  
    *   CDNs don't want to get paid with promises of ad sharing.  
    *   Pick a CDN vendor that will take the time to educate you. They should ask you about your business first, they shouldn't talk about themselves first. He mentions this point a few times and it makes a lot of sense.  
    *   Consider a dual vendor strategy where you pick one vendor for video and another for application.  
    *   Quality in the industry is very high. People rarely complain about the network. Customers want better support and reporting. Poor reporting is the #1 complaint. Run away if a vendor wants to charge for reporting.  

    Lots of good stuff.  

    ## Related Articles

    *   [Highscalability CDN Tag Cloud](http://highscalability.com/tags/cdn)  
    *   [Edge Acceleration Strategies: Akamai](http://www.winnersdontlose.com/2008/02/application-acceleration-strat.html) by Tony Chang  
    *   [A look at Content Delivery Networks, or “how to serve lots of content really fast”](http://royal.pingdom.com/?p=249)  
    *   [Content Delivery Network Pricing, Costs for Outsourced Video Delivery](http://www.scribemedia.org/2007/12/12/cdn-pricing-video/)  
    *   [CDN Pricing Data: Average Cost Per GB Declines In Q4 Due To Startups](http://blog.streamingmedia.com/the_business_of_online_vi/2007/11/cdn-pricing-dat.html)  
    *   [A Taxonomy and Survey of Content Delivery Networks](http://www.gridbus.org/cdn/reports/CDN-Taxonomy.pdf)  
    *   [Content Delivery Networks (CDN) Research Directory](http://www.cs.mu.oz.au/~apathan/CDNs.html)    