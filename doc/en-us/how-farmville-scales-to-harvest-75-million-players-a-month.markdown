## [How FarmVille Scales to Harvest 75 Million Players a Month](/blog/2010/2/8/how-farmville-scales-to-harvest-75-million-players-a-month.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, February 8, 2010 at 8:04AM</span></div>

<div class="body">

_Several readers had follow-up questions in response to this article. Luke's responses can be found in [How FarmVille Scales - The Follow-up](http://highscalability.com/blog/2010/3/10/how-farmville-scales-the-follow-up.html)._

![](http://farm5.static.flickr.com/4049/4335031559_fa690c1f81_m.jpg) If real farming was as comforting as it is in [Zynga's mega-hit Farmville](http://www.farmville.com/) then my family would have probably never left those harsh North Dakota winters. None of the scary bedtime stories my Grandma used to tell about farming are true in FarmVille. [Farmers](http://w.good.is/post/What-Does-Farmville-Mean-for-Farmers/) make money, plants grow, and animals never visit the [red barn](http://www.youtube.com/watch?v=eUJz3137EQ8). I guess it's just that keep-your-shoes-clean back-to-the-land charm that has helped make FarmVille the "largest game in the world" in such an astonishingly short time.

How did FarmVille scale a web application to handle 75 million players a month? Fortunately FarmVille's Luke Rajlich has agreed to let us in on a few their challenges and secrets. Here's what Luke has to say...

The format of the interview was that I sent Luke a few general questions and he replied with this response:

> FarmVille has a unique set of scaling challenges which are unique to the application. The game has had to scale fast and far. The game had 1 M daily players after 4 days and 10M after 60 days. At the time of launch, the largest social game was 5M daily players. Currently, FarmVille has 28M daily players and 75M monthly players 9 months after launch. That makes the monthly player base of FarmVille larger than the entire population of France. There are two fundamental characteristics that make FarmVille a unique scaling challenge: it is the largest game in the world and it is the largest application on a web platform. Both of these aspects present a unique set of scaling challenges that FarmVille has had to overcome. In terms of technology investment, FarmVille primarily utilizes open source components and is at its core built off the LAMP stack.
> 
> In order to make FarmVille scale as a game, we have to accommodate the workload requirements of a game. A user's state contains a large amount of data which has subtle and complex relationships. For example, in a farm, objects cannot collide with each other, so if a user places a house on their Farm, the backend needs to check that no other object in that user's farm occupies an overlapping space. Unlike most major site like Google or Facebook, which are read heavy, FarmVille has an extremely heavy write workload. The ratio of data reads to writes 3:1, which is an incredibly high write rate. A majority of the requests hitting the backend for FarmVille in some way modifies the state of the user playing the game. To make this scalable, we have worked to make our application interact primarily with cache components. Additionally, the release of new content and features tends to cause usage spikes since we are effectively extending the game. The load spikes can be as large as 50% the day of a new feature's release. We have to be able to accommodate this spikey traffic.
> 
> The other piece is making FarmVille scale as the largest application on a web platform and is as large as some of the largest websites in the world. Since the game is run inside of the Facebook platform, we are very sensitive to latency and performance variance of the platform. As a result, we've done a lot of work to mitigate that latency variance: we heavily cache Facebook data and gracefully ratchet back usage of the platform when we see performance degrade. FarmVille has deployed an entire cluster of caching servers for the Facebook platform. The amount of traffic between FarmVille and the Facebook platform is enormous: at peak, roughly 3 Gigabits/sec of traffic go between FarmVille and Facebook while our caching cluster serves another 1.5 Gigabits/sec to the application. Additionally, since performance can be variable, the application has the ability to dynamically turn off any calls back to the platform. We have a dial that we can tweak that turns off incrementally more calls back to the platform. We have additionally worked to make all calls back to the platform avoid blocking the loading of the application itself. The idea here is that, if all else fails, players can continue to at least play the game.
> 
> For any web application, high latency kills your app and highly variable latency eventually kills your app. To address the high latency, FarmVille has worked to put a lot of caching in front of high latency components. Highly variable latency is another challenge as it requires a rethinking of how the application relies on pieces of its architecture which normally have an acceptable latency. Just about every component is susceptible to this variable latency, some more than others. Because of FarmVille's nature, where the workload is very write and transaction heavy, variability in latency has a magnified effect on user experience compared with a traditional web application. The way FarmVille has handled these scenarios is through thinking about every single component as a degradable service. Memcache, Database, REST Apis, etc. are all treated as degradable services. The way in which services degrade are to rate limit errors to that service and to implement service usage throttles. The key ideas are to isolate troubled and highly latent services from causing latency and performance issues elsewhere through use of error and timeout throttling, and if needed, disable functionality in the application using on/off switches and functionality based throttles.
> 
> To help manage and monitor FarmVille's web farm, we utilize a number of open source monitoring and management tools. We use nagios for alerting, munin for monitoring, and puppet for configuration. We heavily utilize internal stats systems to track performance of the services the application uses, such as Facebook, DB, and Memcache. Additionally, when we see performance degradation, we profile a request's IO events on a sampled basis.

## Lessons Learned

There's not quite as much detail as I would like about some things, but there are still a number of interesting points that I think people can learn from:

1.  **Interactive games are write heavy**. Typical web apps read more than they write so many common architectures may not be sufficient. Read heavy apps can often get by with a caching layer in front of a single database. Write heavy apps will need to partition so writes are spread out and/or use an in-memory architecture.
2.  **Design every component as a degradable service**. Isolate components so increased latencies in one area won't ruin another. Throttle usage to help alleviate problems. Turn off features when necessary.
3.  **Cache Facebook data**. When you are deeply dependent on an external component consider caching that component's data to improve latency.
4.  **Plan ahead for new realease related usage spikes**.
5.  **Sample**. When analyzing large streamsof data, looking for problems for example, not every piece of data needs to processed. Sampling data can yield the same resuls for much less work.

I'd like to thank Zynga and Luke Rajlich for making the time for this interview. If anyone else has an architecture that they would like to feature please let me know and we'll set it up.

## Related Articles

1.  [Building Big Social Games](http://www.slideshare.net/amittmahajan/building-big-social-games) - Talks about the game play mechanics behind FarmVille.
2.  [How BuddyPoke Scales on Facebook Using Google App Engine](http://highscalability.com/blog/2010/1/22/how-buddypoke-scales-on-facebook-using-google-app-engine.html)
3.  [Strategy: Sample to Reduce Data Set](http://highscalability.com/blog/2008/4/29/strategy-sample-to-reduce-data-set.html)
4.  HighScalability posts on [caching](http://highscalability.com/blog/category/caching) and [memcached](http://highscalability.com/blog/category/memcached).
5.  HighScalability posts on [sharding](http://highscalability.com/blog/category/sharding).
6.  HighScalability posts on [Memory Grids](http://highscalability.com/blog/category/memory-grid).
7.  [How to Succeed at Capacity Planning Without Really Trying : An Interview with Flickr's John Allspaw on His New Book](../../blog/2009/6/29/how-to-succeed-at-capacity-planning-without-really-trying-an.html)
8.  [Scaling FarmVille](http://perspectives.mvdirona.com/2010/02/13/ScalingFarmVille.aspx) by James Hamilton

</div>