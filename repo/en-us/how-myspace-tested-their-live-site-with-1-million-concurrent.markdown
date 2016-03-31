## [How MySpace Tested Their Live Site with 1 Million Concurrent Users](/blog/2010/3/4/how-myspace-tested-their-live-site-with-1-million-concurrent.html)

    

    

    This is a guest post by Dan Bartow, VP of [SOASTA](http://www.soasta.com/), talking about how they pelted MySpace with 1 million concurrent users using 800 EC2 instances. I thought this was an interesting story because: that's a lot of users, it takes big cajones to test your live site like that, and not everything worked out quite as expected. I'd like to thank Dan for taking the time to write and share this article.    

![](http://farm3.static.flickr.com/2776/4405976247_0fd13b6f26_m.jpg)

    In December of 2009 MySpace launched a new wave of streaming music video offerings in New Zealand, building on the previous success of MySpace music.  These new features included the ability to watch music videos, search for artist’s videos, create lists of favorites, and more. The anticipated load increase from a feature like this on a popular site like MySpace is huge, and they wanted to test these features before making them live.     

    If you manage the infrastructure that sits behind a high traffic application you don’t want any surprises.  You want to understand your breaking points, define your capacity thresholds, and know how to react when those thresholds are exceeded.  Testing the production infrastructure with actual anticipated load levels is the only way to understand how things will behave when peak traffic arrives.     

    For MySpace, the goal was to test an additional 1 million concurrent users on their live site stressing the new video features.  The key word here is ‘concurrent’.  Not over the course of an hour or day… 1 million users concurrently active on the site. It should be noted that 1 million virtual users are only a portion of what MySpace typically has on the site during its peaks.  They wanted to supplement the live traffic with test traffic to get an idea of the overall performance impact of the new launch on the entire infrastructure.  This requires a massive amount of load generation capability, which is where cloud computing comes into play. To do this testing, MySpace worked with SOASTA to use the cloud as a load generation platform.     

    Here are the details of the load that was generated during testing.  All numbers relate to the test traffic from virtual users and do not include the metrics for live users    :

*       1 million concurrent virtual users    
*       Test cases split between searching for and watching music videos, rating videos, adding videos to favorites, and viewing artist’s channel pages    
*       Transfer rate of 16 gigabits per second    
*       6 terabytes of data transferred per hour    
*       Over 77,000 hits per second, not including live traffic    
*       800 Amazon EC2 large instances used to generate load (3200 cloud computing cores)    

    **Test Environment Architecture**     

    SOASTA CloudTest™  manages calling out to cloud providers, in this case Amazon, and provisioning the servers for testing.  The process for grabbing 800 EC2 instances took less than 20 minutes.  Calls we made to the Amazon EC2 API and requests servers in chunks of 25.  In this case, the team was requesting EC2 Large instances with the following specs to act as load generators and results collectors:     

*       7.5 GB memory     
*       4 EC2 Compute Units (2 virtual CPU cores with 2 EC2 Compute Units each)     
*       850 GB instance storage (2×420 GB plus 10 GB root partition)     
*       64-bit platform    
*       Fedora Core 8     
*       In addition, there were 2 EC2 Extra-Large instances to act as the test controller instance and the results database with the following specs:     
*       15 GB memory     
*       8 EC2 Compute Units (4 virtual cores with 2 EC2 Compute Units each)     
*       1,690 GB instance storage (4×420 GB plus 10 GB root partition)     
*       64-bit platform    
*       Fedora Core 8    
*       PostgreSQL Database     

    Once it has all of the servers that it needs for testing it begins doing health checks on them to ensure that they are responding and stable.  As it finds dead servers it discards them and requests additional servers to fill in the gaps.  Provisioning the infrastructure was relatively easy.  The diagram (figure 1.) below shows how the test cloud on EC2 was set up to push massive amounts of load into MySpace’s datacenters.     

        ![](http://farm3.static.flickr.com/2776/4405976247_0fd13b6f26.jpg?__SQUARESPACE_CACHEVERSION=1267718646170)      
    

    **figure 1.**     

    While the test is running, batches of load generators report their performance test metrics back to a single analytics service.  Each of the analytics services connect    

    to the         PostgreSQL         database to store the performance data in an aggregated repository.  This is part of the way that tests of this magnitude can scale to generate and store so much data – by limiting access to the database to only the metrics aggregators and scaling out horizontally.     

    **Challenges**     

    Because scale tends to break everything, there were a number of challenges encountered throughout the testing exercise.     

    **The test was limited to using 800 EC2 instances**    

    SOASTA is one of the largest consumers of cloud computing resources, routinely using hundreds of servers at a time across multiple cloud providers to conduct these massive load tests.  At the time of testing, the team was requesting the maximum number of EC2 instances that it could provision.  The limitation in available hardware meant that each server needed to simulate a relatively large number of users.  Each load generator was simulating between 1,300 and 1,500 users.  This level of load was about 3x what a typical CloudTest™ load generator would drive, and it put new levels of stress on the product that took some creative work by the engineering teams to solve.  Some of the tactics used to alleviate the strain on the load generators included:     

*       Staggering every virtual user’s requests so that the hits per load generator were not all firing at once    
*       Paring down the data being collected to only include what was necessary for performance analysis    

##     **A large portion of MySpace assets are served from Akamai, and the testing repeatedly maxed out the service capability of parts of the Akamai infrastructure**    

    CDN’s typically serve content to site visitors based on their geographic location from a point of presence closest to them.  If you generate all of the test traffic from, say, Amazon’s East coast availability zone, then you are likely going to be hitting only one Akamai point of presence.      

    Under load, the test was generating a significant amount of data transfer and connection traffic towards a handful of Akamai datacenters.  This equated to more load on those datacenters than what would probably be generated during typical peaks, but that would not necessarily be unrealistic given that this feature launch was happening for New Zealand traffic only.  This stress resulted in new connections being broken or refused by Akamai at certain load levels, and generating lots of errors in the test.       

    This is a common hurdle that needs to be overcome when generating load against production sites.  Large-scale production tests need to be designed to take this into account and accurately stress entire production ecosystems.  This means generating load from multiple geographic locations so that the traffic is spread out over multiple datacenters. Ultimately, understanding the capacity of geographic POPs was a valuable takeaway from the test.     

##     **Because of the impact of the additional load, MySpace had to reposition some of their servers on-the-fly to support the features being tested**    

    During testing the additional virtual user traffic was stressing some of the MySpace infrastructure pretty heavily.  MySpace’s operations team was able to grab underutilized servers from other functional clusters and use them to add capacity to the video site cluster in a matter of minutes.      

    Probably the most amazing thing about this is that MySpace was able to actually do it.  They were able to monitor capacity in real time across the whole infrastructure and elastically shrink and expand where needed.  People talk about elastic scalability all of the time and it’s a beautiful thing to see in practice.       

    **Lessons Learned**     

1.      For high traffic websites, testing in production is the only way to get an accurate picture of capacity and performance.  For large application infrastructures there are far too many ‘invisible walls’ that can show up if you only test in a lab and then try to extrapolate.     
2.      Elastic scalability is becoming an increasingly important part of application architectures.  Applications should be built so that critical business processes can be independently monitored and scaled.  Being able to add capacity relatively quickly is going to be a key architecture theme in the coming year and the big players have known this for a long time.  Facebook, Ebay, Intuit, and many other big web names have evangelized this design principle.  Keeping things loosely coupled has a whole slew of benefits that have been advertised before, but capacity and performance are quickly moving to the front of that list.     
3.      Real-time monitoring is critical.  In order to react to capacity or performance problems, you need real-time monitoring in place.  This monitoring should tie in to your key business processes and functional areas, and needs to be as real time as possible.    

## ﻿Related Articles

*   [MySpace Architecture](/blog/2009/2/12/myspace-architecture.html)
*   [How to Succeed at Capacity Planning Without Really Trying : An Interview with Flickr's John Allspaw on His New Book](/blog/2009/6/29/how-to-succeed-at-capacity-planning-without-really-trying-an.html)

    