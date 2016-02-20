## [YouTube Strategy: Adding Jitter isn't a Bug](/blog/2012/4/17/youtube-strategy-adding-jitter-isnt-a-bug.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, April 17, 2012 at 9:15AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7138/7081913937_f8910dffd9_m.jpg) The adding [<span>jitter</span>](http://en.wikipedia.org/wiki/Jitter) <span>strategy was one of the most commented on techniques from</span> [<span>7 Years Of YouTube Scalability Lessons In 30 Minutes</span>](http://highscalability.com/blog/2012/3/26/7-years-of-youtube-scalability-lessons-in-30-minutes.html) <span>on</span> [<span>HackerNews</span>](http://news.ycombinator.com/item?id=3757456)<span>. Probably because it’s one of the emergent phenomena that you really can’t predict and is shocking when you see it in real life. Here’s the technique:</span>

### <span>Add Entropy Back into Your System</span>

*   <span>If your system doesn’t jitter then you get</span> [<span>thundering herds</span>](http://highscalability.com/blog/2008/3/14/problem-mobbing-the-least-used-resource-error.html)<span>. Distributed applications are really weather systems. Debugging them is as deterministic as predicting the weather. Jitter introduces more randomness because surprisingly, things tend to stack up.</span>
*   <span>For example, cache expirations. For a popular video they cache things as best they can. The most popular video they might cache for 24 hours. If everything expires at one time then every machine will calculate the expiration at the same time. This creates a thundering herd.</span>
*   <span>By jittering you are saying  randomly expire between 18-30 hours. That prevents things from stacking up. They use this all over the place. Systems have a tendency to self synchronize as operations line up and try to destroy themselves. Fascinating to watch. You get slow disk system on one machine and everybody is waiting on a request so all of a sudden all these other requests on all these other machines are completely synchronized. This happens when you have many machines and you have many events. Each one actually removes entropy from the system so you have to add some back in.</span>

Comments from HackerNews really help to fill out the topic with more detail:  

**There were some other examples of the thundering herd problem**:

*   <span>Adblock Plus had a</span> [<span>thundering herd problem</span>](https://adblockplus.org/blog/downloading-a-file-regularly-how-hard-can-it-be)<span>: Ad blocking lists check for updates every 5 days. Eventually, many users' update schedules would converge on Mondays because office computers did not run over the weekend. Updates that had been scheduled for Saturday or Sunday would spill over to Monday.</span> <span>From</span> [<span>cpeterso</span>](http://news.ycombinator.com/item?id=3759196)<span>.</span>
*   <span>Facebook uses jitter a lot in their cache infrastructure.</span> <span>From</span> [<span>alexgartrell</span>](http://news.ycombinator.com/item?id=3758514)<span>.</span>
*   <span>The</span> [<span>Multicast DNS</span>](http://datatracker.ietf.org/doc/draft-cheshire-dnsext-multicastdns/) <span>extension makes extensive use of randomness to reduce network collisions.  </span><span>From</span> [<span>makmanalp</span>](http://news.ycombinator.com/item?id=3758634)<span>.</span>
*   <span>Jitter is used in all sorts of applications, from cron jobs to configuration management to memcache key expiration. Any time you have a lot of nodes that all need to do an operation at a specific time you rely on jitter to keep resources from bottlenecking. Probably used anywhere the systems or network has a miniscule amount of resource headroom (like cluster nodes that run at 93% utilization).</span> <span>From</span> [<span>peterwwillis</span>](http://news.ycombinator.com/item?id=3758455)<span>.</span>
*   <span>I frequently find myself adding a sleep for PID mod some appropriate constant to the beginning of big distributed batch jobs in order to keep shared resources (NFS, database, etc) from getting hammered all at once.</span> <span>From</span> [<span>minimax</span>](http://news.ycombinator.com/item?id=3758382)<span>.</span>

**There are a number of ways of adding jitter**:

*   <span>Use</span> [<span>exponentially distributed delays</span>](http://en.wikipedia.org/wiki/Poisson_process)<span>: Then reoccurring events form poisson processes which are really easy to reason about when composed eg if you have N servers firing off events at the same exponentially distributed rate the times are distributed the same as if you had one server firing events at N x the rate and distributing jobs uniformly at random to the other servers.</span> <span>From</span> [<span>jamii</span>](http://news.ycombinator.com/item?id=3758609)<span>.  </span>
*   <span>Use</span> [<span>distinct prime numbers</span>](http://www.stdlib.net/~colmmacc/2009/11/26/period-pain-3/) <span>for periodicities in the first place.</span> <span>From</span> [<span>colmmacc</span>](http://news.ycombinator.com/item?id=3758618)<span>.</span>

**Not everyone adds jitter**:

*   <span>Jeff Dean at Google says they prefer to have a known hit every once in awhile, so they will have all the cron jobs go off at the same time versus introducing jitter. It perhaps has to do with their emphasis on reducing variability to control the size of the long tail distribution.</span>
*   <span>The Linux kernel tries to schedule timer events for the same deadline time. That allows the processor to sleep longer because the kernel doesn't need to wake up as often just to handle 1 or 2 timer events.</span> <span>From</span> [<span>cpeterso</span>](http://news.ycombinator.com/item?id=3759222)<span>.</span>

**An interesting paper**:

*   [<span>The Synchronization of Periodic Routing Messages</span>](http://ee.lbl.gov/papers/sync_94.pdf) <span>by Sally Floyd and Van Jacobson.</span> <span>From</span> [<span>breidh</span>](http://news.ycombinator.com/item?id=3758552)<span>.</span>

</div>