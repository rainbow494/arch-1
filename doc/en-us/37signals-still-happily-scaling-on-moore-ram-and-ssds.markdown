## [37signals Still Happily Scaling on Moore RAM and SSDs](/blog/2012/1/30/37signals-still-happily-scaling-on-moore-ram-and-ssds.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, January 30, 2012 at 8:28AM</span></div>

<div class="body">

![](http://s3.amazonaws.com/37assets/svn/755-840gb-of-ram-1.png)

There are so many architectural ideas swirling in the bit wind these days. Two of the biggest battles are cloud vs. bare metal and RAM vs. disk vs. SSD. 37signals has published two solid articles that are counter hype cycle in their message:

> Technologists who grew up when RAM cost $1,000 per megabyte can have a hard time dealing with the luxury of RAM being virtually free.
> 
> The progress of technology is throwing an ever greater number of optimizations into the “premature evil” bucket never to be seen again.

37signals made quite a stir with their money shot of the [864GB of RAM they bought for a mere $12K](http://37signals.com/svn/posts/3090-basecamp-nexts-caching-hardware) as part of their caching layer for Basecamp. That's a lot of memory for not a lot of money. There's nothing like actually seeing it in the flesh to bring the point home. Does that make [Memory Based Architectures](http://highscalability.com/are-cloud-based-memory-architectures-next-big-thing) a little more appealing?

37signals then followed up with another provocative article: [Three years later, Mr. Moore is still letting us punt on database sharding](http://37signals.com/svn/posts/3089-three-years-later-mr-moore-is-still-letting-us-punt-on-database-sharding). The gist is scaling up is working for them. RAM is getting cheaper and FusionIO is getting faster, so they've been able to avoid architecture complexifications like sharding. Does that make SSD based architectures a little more appealing?

StackExchange is in [much the same position](http://highscalability.com/blog/2011/10/24/stackexchange-architecture-updates-running-smoothly-amazon-4.html), with a different stack, but with sympatico core ideas and comparable results. The learning: In your transaction oriented features, if you aren't Googleish in your requirements, then scale-up using bare metal, RAM, and SSD may be the way to go. The tug you feel towards the cloud and horizontal scaling may just be a strong consensus wind a blowin'.

Some of the key takeways are: 

*   SSD technology is accelerating which makes it unlikely they will need to shard Basecamp in the future.
*   Memory is still expensive in the cloud and on a VPS. RAM is at a premium. So to move to a RAM architecture you need to go bare metal. A generous commenter  priced elastic cache at: 20k / month for ~ 800 GB of Memory (12 * Quadruple Extra Large nodes / 68 GB).
*   37signals uses FusionIO for their databases, but since the RAM was to be installed in 3 servers it was cheaper to go with RAM.
*   BaseCamp has a relatively predictable capacity planning problem, so bare metal is far more cost efficient than the cloud. If you are Netflix with wild swings in your usage patterns, then the tradeoffs may be different.
*   SSD has higher densities and lower cost than RAM, but is far slower than RAM. RAM accelerates read and write loads whereas SSD accelerates [reads better than writes](http://guyharrison.squarespace.com/blog/2010/10/21/accelerating-oracle-database-performance-with-ssd.html).
*   37signals handles failure by having redundancy in all systems. All databases have replicas. Excess capacity is maintained as are spare servers.  There's no geographic redundancy as of yet.
*   Schema changes, a notorious bottleneck in most relational databases, is less of a problem with SSD. They still cache tables in RAM as much as possible.

## Related Articles

*   Google Groups Thread: [In-memory database (inspired by 864GB of RAM for $12,000)](https://groups.google.com/forum/#!topic/nodejs/aqVYp5ABX_4)
*   [Three years later, Mr. Moore is still letting us punt on database sharding ](http://37signals.com/svn/posts/3089-three-years-later-mr-moore-is-still-letting-us-punt-on-database-sharding)

</div>