## [It's the Fraking IOPS - 1 SSD is 44,000 IOPS, Hard Drive is 180](/blog/2011/6/22/its-the-fraking-iops-1-ssd-is-44000-iops-hard-drive-is-180.html)

    

    

![](http://farm6.static.flickr.com/5061/5860539924_bb80dd7c5a_o.jpg)

Planning your next buildout and thinking SSDs are still far in the future? Still too expensive, too low density. Hard disks are cheap, familiar, and store lots of stuff. In this short and entertaining video Wikia's [Artur Bergman](http://twitter.com/#!/crucially) wants to change your mind about SSDs. SSDs are for today, get with the math already.

Here's Artur's logic:

*   Wikia is all SSD in production. The new Wikia file servers have a theoretical read rate of ~10GB/sec sequential, 6GB/sec random and 1.2 million IOPs. If you can't do math or love the past, you love spinning rust. If you are awesome you love SSDs.
*   SSDs are cheaper than drives using the most relevant metric: $/GB/IOPS. 1 SSD is 44,000 IOPS and one hard drive is 180 IOPS. Need 1 SSD instead of 50 hard drives.
*   With 8 million files there's a 9 minute fsck. Full backup in 12 minutes (X-25M based).
*   4 GB/sec random read average latency 1 msec.
*   2.2 GB/sec random write average latency 1 msec.
*   50TBs of SSDs in one machine for $80,000\. With the densities most products can skip sharding completely.
*   Joins are slow because random access disk IO slow. Not true with SSDs. Joins will perform well.
*   Best way to save power because you need fewer CPUs. 
*   Recommends starting small, with Intel 320s. Don't need fancy high end cards. 40K IOPS goes a long ways. $1000 for 600 GB.

 Here's the [video](http://www.livestream.com/oreillyconfs/video?clipId=pla_3beec3a2-54f5-4a19-8aaf-35a839b6ecaa):

<iframe width="560" height="340" src="http://cdn.livestream.com/embed/oreillyconfs?layout=4&amp;clip=pla_3beec3a2-54f5-4a19-8aaf-35a839b6ecaa&amp;autoplay=false" style="border:0;outline:0" frameborder="0" scrolling="no"></iframe>

Well worth watching. The simplicity of not having to fight IO can be a real win. Some people have claimed SSDs aren't reliable, others claim they are. And if you need a lot CPU processing you'll those machines anyway so centralizing storage on fast SSDs may not be that big a win. The point here is that's it probably high time to consider SSD based architectures.

## Related Articles         

*   [Can flash SSDs be trusted?](http://storagemojo.com/2011/06/20/can-flash-ssds-be-trusted/) by Robin Harris. TL;DR: Yes.

    