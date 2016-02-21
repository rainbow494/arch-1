## [Secrets to Fotolog's Scaling Success](/blog/2007/10/2/secrets-to-fotologs-scaling-success.html)

    

    

Fotolog, a social blogging site centered around photos, grew from about 300 thousand users in 2004 to over 11 million users in 2007\. Though they initially experienced the inevitable pains of rapid growth, they overcame their problems and now manage over 300 million photos and 800,000 new photos are added each day. Generating all that fabulous content are 20 million unique monthly visitors and a volunteer army of 30,000 new users each day. They did so well a very impressed suitor bought them out for a cool $90 million. That's scale meets success by anyone standards. How did they do it?

Site: http://www.fotolog.com

## Information Sources

*   [Scaling the World's Largest Photo Blogging Community](http://www.slideshare.net/frankmashraqi/fotolog-scaling-the-worlds-largest-photo-blogging-community)*   [Congrats to Fotolog on $90mm sale to Hi-Media](http://www.beyondvc.com/2007/08/congrats-to-fot.html)*   [Fotolog overtaking Flickr?](http://www.kottke.org/07/01/fotolog-overtaking-flickr)*   [Fotolog Hits 11 Million Members and 300 Million Photos Posted](http://www.stockphototalk.com/the_stock_photo_industry_/2007/09/fotolog-hits-11.html)*   [Site of the Week: Fotolog.com](http://www.pcmag.com/article2/0,1895,2150030,00.asp) by PC Magazine*   [CEO John Borthwick's Blog](http://www.borthwick.com/weblog/category/fotolog/).*   [DBA Frank Mash's Blog](http://mysqldatabaseadministration.blogspot.com)*   [Fotolog, lessons learnt](http://www.borthwick.com/weblog/2008/01/09/fotolog-lessons-learnt/) by John Borthwick  

    ## The Platform

    *   Java*   PHP*   Sun*   Solaris 10*   MySQL*   Apache*   Hibernate*   Memcached*   [3PAR](http://www.3par.com/index.php) (a simple, efficient and scalable tiered-storage array for utility computing)*   [IBRIX](http://www.ibrix.com/) (a single namespace parallel file system, a scalable volume manager, high availability feature)*   StrongMail*   CDN: Akamai/Panther  

    ## The Stats

    *   Started in 2002\. In 2004 they had around 300k or 400k members, 3 employees, no scalable infrastructure, and no revenue model.*   Due to the rapid growth the site had frequent technical problems and 2005 they had to limit new free members to 1,000 a day.*   In 2007 they had over 11 million users and were sold for $90 million to Hi-Media.*   Members are from over 200 countries with a majority in South America. Over 20% of page views are from Europe. They rejected a US centric strategy, developing a global and engaged audience.*   Generates over 3.5 billion page views and receives over 20 million unique visitors each month and has earned a top 20 Alexa ranking.*   Manages over 300 Million photos and over 500,000 photos are uploaded each day.*   Over 30,000 new members are added each day and attracts more than 4.6 million daily users. Expanded with no marketing or member incentives.*   Over 500 user-generated communities.*   20% of member visit the site daily and spend an average of 24 minutes.*   32 MySQL servers and a 30 memcached server cluster.  

    ## The Architecture

    *   Site originally written in PHP.  
    - Their new "Fotolog memberpage" feature is written in Java with significant performance improvement. Page is cleaner with an improved response time.  
    - They are now serving the site on less than half the boxes they were using.  
    - Daily registrations are up over 35% given the improved performance and a requirement to register to post a guest book message.  
    - The new code base allows them to innovate much more on the member experience.  

    *   They have surpassed Flickr in popularity being a firmly Web 1.0 application.  
    - There are no tags, no APIs, no JavaScript widgets, no Ajax.  
    - They have a Spanish language option which extends the site to a broad user base.  
    - They use very little text. It's mostly visual so it usable by a broad range of users.  
    - Their interface is customizable and many people like to express their individual identities.  
    - Their unique visitors are 1MM less than Yahoo's, yet the total minutes on the site are twice that of Yahoo and pages are 3x.  

    *   Revenue model:  
    - Gold camera member for about $5/month means you can upload 6 photos a day instead of 1, have 200 comments per photo instead of 20, a custom title image for your profile, a mini-thumbnail of your most recent photo displayed next to your name in guest books, plus the possibility of having your photo featured on the front page..  
    - Adsense. Revenue lift from Google is trending up approximately 15% given additional contextual data from guest books.  
    - Will move to a peer-to-peer advertising among their members.  
    - Members will have the ability to buy and sell real and virtual items using a micro-payment service.  

    *   They have a one-post-per-day rule where users can only post one photo a day. Rather than inhibit growth this rules ensures quality and generates exceptional usage by increasing the chance of a photo being seen and by attracting positive comments. Where as people usually run out of things to say on a blog, people can always find a picture to take, upload, and talk about.*   Only photos less than 2,000 kb in size can be uploaded. These are automatically resized to a 500x500 format. Pages look cleaner and load faster.*   Model is browsing over searching. Opportunistic serendipitous treasure hunting is encouraged.*   Friends are added automatically without needing permission. This generates an audience for your photos.*   Supports a browse by groups feature, which have categories like "Colors" and "Emotions."*   The site is intentionally simple.  
    - They have resisted the temptation to add feature after feature. Instead their vision is to offer a handful of features, similar to Craig's list, the focus being on content and the conversations.  
    - Pages need to be social.  
    - Pages need to include not only your images, but also images from across the network, providing a visual navigation that today drives much of the time their members spend on the site, a self formed, organic distribution system, letting members see and be seen.  
    - Complementing this social network of images are comments and guest book entries — making the experience one where media intersects with communications, day in day out, millions of images collide with billions of conversations.*   Photobucket vs Fotolog  
    - Photobucket stores image-based media, then distributes it to your page on social networking sites such as Myspace, Bebo, Piczo, Friendster, etc.  
    - Fotolog is a destination.  
    - The first generation of social-networking sites stressed self-publishing over connections (from Geocities, to Tripod to Blogger). The next generation focused mostly on connections (sixdegrees, and friendster are the classic examples here — tools to gather friends and connections, as social capital accrues in theory to the people with the most connections). The third and current generation of sites blends media with connections — each with a different emphasis.  

    *   Backup: Sun 6540 disk array*   Their 32 SQL servers are divided into four clusters  
    - user, GB (guest book), PH (photos), FF (friends and favorites lists)  
    - Uses non-persistent connections.  
    - Connection pooling on the Java side.  
    - InnoDB  
    - Partitioning is handled by the application layer.*   Each cluster:  
    - Is fronted by a set of application servers.  
    - Divided into a set of shards.  
    - Each shard has MySQL write-only master-master configuration feeding a few read-only slaves.  
    - Application servers send their read requests to the slaves and their write requests to the masters.  
    - Data are assigned to shared based on some sort of cluster specific partioning key. Naive partitioning algorithms can lead to very uneven shard loads, you want a more balanced load on each shard.*   MySQL is used to store image metadata only. This seems pretty standard. Almost nobody seems to store important blobs in the database because it slows down database operations.*   Photo storage uses 3PAR and IBRIX. A CDN is used for hot content.*   The virtual storage system, though expensive, has worked very well.*   As more selects are used lock contention for auto-incremented keys grows.*   Through database optimizations they've been able to grow from 4 million members to 11 million members on the same 32 database servers. This is also do to the efficiency of MySQL running on Solaris 10, and increasing the memcache cluster, porting to Java, and increasing RAM.*   Happy with memcached.  
    - Created a distributed cluster of 50 memcached servers with a total cache size of approximately 150 gigabytes, supporting around 4 billion page views/month. Peak load times dropped from 10 seconds to 2 seconds.  
    - Quote from [CTO](http://mashraqi.com/labels/memcached.html):

    > _I have a new memcached user to add to your list: we here at Fotolog, the world's largest photo blogging community, now use it and we love it. I just rolled our first code to use it into production today and it has been a lifesaver. I can't wait to start using it in places where we had been relying on Berkeley databases to offload some database work. We are not some wimpy million page a day site, either. Fotolog is a billion+ pages/month site (35 to 40 million views/day is pretty typical for us). We had recently overcome some significant DB-related performance issues which allowed our site traffic to explode, and it started to bog down again under the heavy traffic load (getting back up towards 10 seconds for a page to load sometimes during the peak periods). The servers were churning away each recreating a list every time when it could easily be shared in the same form for at least 5 or 10 minutes. So we introduced memcache, creating a distributed 30-server cluster with 4 gigs available in total and made a very minor code mod to use memcache, and our peak period load times dropped back down to the 2 second or so range. It has allowed for continued growth and incredible efficiency. I can't say when I've ever been so pleased with something that worked so simply."_

    ## Lessons Learned

    *   Popularity is driven by a base of active users, not a rich set of cool features.*   The web is global and its tail is very long. By courting users outside the US with language and culturally specific design you can compete with the big boys. Some the hardest competition for Google, Yahoo, etc comes from local startups with an ear to what the locals want.*   If you want to get a lot of buzz then do what ever alpha geeks want you to do. If you want a lot of happy users do what they want you to do.*   Constraints in web sites can, like in poetry, make something unexpectedly better. The rule that users are only allowed to post one photo per day creates an environment where people comment more on each others photos which creates a more engaged community. Who knew?*   Protect your website with limits. Limit the size of pictures, comments, etc so your resource usage doesn't grow outrageously.*   Have a vision. Have a strong sense of what your site is supposed to be and why, then use that vision to decide what you should build and how you should build it. Their vision of social site built around daily photographs led to a very different site than one where your goal is to store all your photos.*   Revenue generation features can be added without destroying the integrity of your site. I really like how they give people a reasonable set of features for free and then charge for the resources they need to have more. Those features also serve to extend and reinforce the social vision of their site. It will be interesting to see how their new monetization strategies play out.*   Don't be afraid to scale up and out. By adding more cache, more RAM, more CPUs, and more efficient CPUs you handle dramatically more load with the same number of machines. And that's a good thing from a datacenter space and power POV.*   Making MySQL perform:  
    - Find the source of the problem.  
    - Mature systems are mostly disk bound.  
    - The query cache may be hurting you.  
    - Add RAM to help dodge the bullet.  
    - Stripe your disks.  
    - Restructure tables for optimal performance.  
    - Use libumem.so to find memory leaks.*   Things to remember:  
    - Know the problem  
    - Know your application  
    - Know your storage engine  
    - Know your requirements  
    - Know your budget  
    - Use all this information to decide what parts of your system really require the investment of time, money, and testing to be highly available.  

    ## Related Articles

    *   [Flickr Architecture](http://highscalability.com/flickr-architecture)*   [An Unorthodox Approach to Database Design : The Coming of the Shard](http://highscalability.com/unorthodox-approach-database-design-coming-shard)    