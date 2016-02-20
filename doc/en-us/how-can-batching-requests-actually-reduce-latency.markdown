## [How Can Batching Requests Actually Reduce Latency?](/blog/2013/12/4/how-can-batching-requests-actually-reduce-latency.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, December 4, 2013 at 8:56AM</span></div>

<div class="body">

Jeremy Edberg gave a talk on [Scaling Reddit from 1 Million to 1 Billion–Pitfalls and Lessons]( http://www.infoq.com/presentations/scaling-reddit) and [one of the issues](http://highscalability.com/blog/2013/8/26/reddit-lessons-learned-from-mistakes-made-scaling-to-1-billi.html) they had was that they:

> Did not account for increased latency after moving to EC2\. In the datacenter they had submillisecond access between machines so it was possible to make a 1000 calls to memache for one page load. Not so on EC2\. Memcache access times increased 10x to a millisecond which made their old approach unusable. Fix was to batch calls to memcache so a large number of gets are in one request.

Dave Pacheco had an [interesting question](https://news.ycombinator.com/item?id=6224990) about batching requests and its impact on latency:

> <span>I was confused about the memcached problem after moving to the cloud. </span>I understand why network latency may have gone from submillisecond to milliseconds, but how could you improve latency by batching requests? Shouldn't that improve efficiency, not latency, at the possible expense of latency (since some requests will wait on the client as they get batched)? 

Jeremy cleared it up by saying:

> The latency didn't get better, but what happened is that instead of having to make a lot of calls to memcache it was just one (well, just a few), so while that one took longer, the total time was much less.

But [Dave Rosenthal](https://news.ycombinator.com/item?id=6225221) created a great graphic showing how batching can in fact decrease total system latency:

<span class="full-image-block ssNonEditable"><span>![](http://farm3.staticflickr.com/2861/10953478314_e14d289f4e.jpg?__SQUARESPACE_CACHEVERSION=1384908201915)</span></span>

</div>