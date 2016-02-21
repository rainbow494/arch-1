## [YouTube Strategy: Adding Jitter isn't a Bug](/blog/2012/4/17/youtube-strategy-adding-jitter-isnt-a-bug.html)

    

    

![](http://farm8.staticflickr.com/7138/7081913937_f8910dffd9_m.jpg) The adding [    jitter    ](http://en.wikipedia.org/wiki/Jitter)     strategy was one of the most commented on techniques from     [    7 Years Of YouTube Scalability Lessons In 30 Minutes    ](http://highscalability.com/blog/2012/3/26/7-years-of-youtube-scalability-lessons-in-30-minutes.html)     on     [    HackerNews    ](http://news.ycombinator.com/item?id=3757456)    . Probably because it’s one of the emergent phenomena that you really can’t predict and is shocking when you see it in real life. Here’s the technique:    

###     Add Entropy Back into Your System    

*       If your system doesn’t jitter then you get     [    thundering herds    ](http://highscalability.com/blog/2008/3/14/problem-mobbing-the-least-used-resource-error.html)    . Distributed applications are really weather systems. Debugging them is as deterministic as predicting the weather. Jitter introduces more randomness because surprisingly, things tend to stack up.    
*       For example, cache expirations. For a popular video they cache things as best they can. The most popular video they might cache for 24 hours. If everything expires at one time then every machine will calculate the expiration at the same time. This creates a thundering herd.    
*       By jittering you are saying  randomly expire between 18-30 hours. That prevents things from stacking up. They use this all over the place. Systems have a tendency to self synchronize as operations line up and try to destroy themselves. Fascinating to watch. You get slow disk system on one machine and everybody is waiting on a request so all of a sudden all these other requests on all these other machines are completely synchronized. This happens when you have many machines and you have many events. Each one actually removes entropy from the system so you have to add some back in.    

Comments from HackerNews really help to fill out the topic with more detail:  

**There were some other examples of the thundering herd problem**:

*       Adblock Plus had a     [    thundering herd problem    ](https://adblockplus.org/blog/downloading-a-file-regularly-how-hard-can-it-be)    : Ad blocking lists check for updates every 5 days. Eventually, many users' update schedules would converge on Mondays because office computers did not run over the weekend. Updates that had been scheduled for Saturday or Sunday would spill over to Monday.         From     [    cpeterso    ](http://news.ycombinator.com/item?id=3759196)    .    
*       Facebook uses jitter a lot in their cache infrastructure.         From     [    alexgartrell    ](http://news.ycombinator.com/item?id=3758514)    .    
*       The     [    Multicast DNS    ](http://datatracker.ietf.org/doc/draft-cheshire-dnsext-multicastdns/)     extension makes extensive use of randomness to reduce network collisions.          From     [    makmanalp    ](http://news.ycombinator.com/item?id=3758634)    .    
*       Jitter is used in all sorts of applications, from cron jobs to configuration management to memcache key expiration. Any time you have a lot of nodes that all need to do an operation at a specific time you rely on jitter to keep resources from bottlenecking. Probably used anywhere the systems or network has a miniscule amount of resource headroom (like cluster nodes that run at 93% utilization).         From     [    peterwwillis    ](http://news.ycombinator.com/item?id=3758455)    .    
*       I frequently find myself adding a sleep for PID mod some appropriate constant to the beginning of big distributed batch jobs in order to keep shared resources (NFS, database, etc) from getting hammered all at once.         From     [    minimax    ](http://news.ycombinator.com/item?id=3758382)    .    

**There are a number of ways of adding jitter**:

*       Use     [    exponentially distributed delays    ](http://en.wikipedia.org/wiki/Poisson_process)    : Then reoccurring events form poisson processes which are really easy to reason about when composed eg if you have N servers firing off events at the same exponentially distributed rate the times are distributed the same as if you had one server firing events at N x the rate and distributing jobs uniformly at random to the other servers.         From     [    jamii    ](http://news.ycombinator.com/item?id=3758609)    .      
*       Use     [    distinct prime numbers    ](http://www.stdlib.net/~colmmacc/2009/11/26/period-pain-3/)     for periodicities in the first place.         From     [    colmmacc    ](http://news.ycombinator.com/item?id=3758618)    .    

**Not everyone adds jitter**:

*       Jeff Dean at Google says they prefer to have a known hit every once in awhile, so they will have all the cron jobs go off at the same time versus introducing jitter. It perhaps has to do with their emphasis on reducing variability to control the size of the long tail distribution.    
*       The Linux kernel tries to schedule timer events for the same deadline time. That allows the processor to sleep longer because the kernel doesn't need to wake up as often just to handle 1 or 2 timer events.         From     [    cpeterso    ](http://news.ycombinator.com/item?id=3759222)    .    

**An interesting paper**:

*   [    The Synchronization of Periodic Routing Messages    ](http://ee.lbl.gov/papers/sync_94.pdf)     by Sally Floyd and Van Jacobson.         From     [    breidh    ](http://news.ycombinator.com/item?id=3758552)    .    

    