## [Pinterest Architecture Update - 18 Million Visitors, 10x Growth,12 Employees, 410 TB of Data](/blog/2012/5/21/pinterest-architecture-update-18-million-visitors-10x-growth.html)

    

    

![](http://farm8.staticflickr.com/7180/6886606039_bc5c70dbf9_m.jpg)

There has been an update on Pinterest: [Pinterest growth driven by Amazon cloud scalability](http://news.techworld.com/storage/3352613/pinterest-growth-driven-by-amazon-cloud-scalability/) since our last post: [A Short on the Pinterest Stack for Handling 3+ Million Users](http://highscalability.com/blog/2012/2/16/a-short-on-the-pinterest-stack-for-handling-3-million-users.html).

With Pinterest we see a story very similar to that of [Instagram](http://highscalability.com/blog/2012/4/16/instagram-architecture-update-whats-new-with-instagram.html). Huge growth, lots of users, lots of data, with remarkably few employees, all on the cloud.

While it's true that both Pinterest and Instagram are not making great advances in [science and technology](http://www.theatlantic.com/business/archive/2012/05/the-golden-age-of-silicon-valley-is-over-and-were-dancing-on-its-grave/257401/), that is more indicator of the easy power of today's commodity environments rather than a sign of Silicon Valley's lack of innovation. The numbers are so huge and the valuations are so high we naturally want some sort of fundamental technological revolution to underlie their growth. The revolution is more subtle. It really is just that easy to attain such growth these days, if you can execute on the right idea. Get used to it. This is the new normal.

Here's what Pinterest looks like today: 

*   80 million objects stored in S3 with 410 terabytes of user data, 10x what they had in August. EC2 instances have grown by 3x.      Around $39K fo S3 and $30K for EC2.    
*   12 employees as of last December. Using the cloud a site can grow dramatically while maintaining a very small team. _Looks like [31 employees](http://pinterest.com/about/team/) as of now_.
*   Pay for what you use saves money. Most traffic happens in the afternoons and evenings, so they reduce the number of instances at night by 40%. At peak traffic  $52 an hour is spent on EC2 and at night, during off peak, the spend is as little as $15 an hour.
*   150 EC2 instances in the web tier
*   90 instances for in-memory caching, which removes database load
*   35 instances used for internal purposes
*   70 master databases with a parallel set of backup databases in different regions around the world for redundancy
*   Written in Python and Django 
*   Sharding is used, a database is split when it reaches 50% of capacity, allows easy growth and gives sufficient IO capacity
*   ELB is used to load balance across instances. The ELB API makes it easy to move instances in and out of production.
*   One of the fastest growing sites in history. Cites AWS for making it possible to handle 18 million visitors in March, a 50% increase from the previous month, with very little IT infrastructure.
*   The cloud supports easy and low cost experimenation. New services can be tested without buying new servers, no big up front costs.
*   Hadoop-based Elastic Map Reduce is used for data analysis and costs only a few hundred dollars a month.

## Related Articles

*   [On Hacker News](http://news.ycombinator.com/item?id=4003863)
*   [On GigaOM](http://gigaom.com/cloud/innovation-isnt-dead-it-just-moved-to-the-cloud/)
*   [Startups Are Creating A New System Of The World For IT](http://highscalability.com/blog/2012/5/7/startups-are-creating-a-new-system-of-the-world-for-it.html)
*   [What is Amazon’s Secret for Success and Why is EC2 a Runaway Train?](http://www.cloudscaling.com/blog/cloud-computing/what-is-amazons-secret-for-success-and-why-is-ec2-a-runaway-train/)

    