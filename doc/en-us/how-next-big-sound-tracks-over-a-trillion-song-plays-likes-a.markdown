## [How Next Big Sound Tracks Over a Trillion Song Plays, Likes, and More Using a Version Control System for Hadoop Data](/blog/2014/1/28/how-next-big-sound-tracks-over-a-trillion-song-plays-likes-a.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, January 28, 2014 at 8:56AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7459/12179769185_bee3ffd6b8_m.jpg)

_<span>This is a guest post by [Eric Czech](http://www.linkedin.com/profile/view?id=26317730)</span>, Chief Architect at Next Big Sound, talks about some unique approaches taken to solving scalability challenges in music analytics._

Tracking online activity is hardly a new idea, but doing it for the entire music industry isn't easy. Half a billion music video streams, track downloads, and artist page likes occur each day and measuring all of this activity across platforms such as Spotify, iTunes, YouTube, Facebook, and more, poses some interesting scalability challenges. Next Big Sound collects this type of data from over a hundred sources, standardizes everything, and offers that information to record labels, band managers, and artists through a web-based analytics platform.

While many of our applications use open-source systems like Hadoop, HBase, Cassandra, Mongo, RabbitMQ, and MySQL, our usage is fairly standard, but there is one aspect of what we do that is pretty unique. We collect or receive information from 100+ sources and we struggled early on to find a way to deal with how data from those sources changed over time, and we ultimately decided that we needed a data storage solution that could represent those changes.  Basically, we needed to be able to "version" or "branch" the data from those sources in much the same way that we use revision control (via Git) to control the code that creates it.  We did this by adding a logical layer to a Cloudera distribution and after integrating that layer within Apache Pig, HBase, Hive, and HDFS, we now have a basic version control framework for large amounts of data in Hadoop clusters.

As a sort of "[Moneyball for Music,](http://www.forbes.com/sites/zackomalleygreenburg/2013/02/13/moneyball-for-music-the-rise-of-next-big-sound/)" Next Big Sound has grown from a single server LAMP site tracking plays on MySpace (it was cool when we started) for a handful of artists to building industry-wide [popularity charts for Billboard](http://www.digitaltrends.com/social-media/billboard-social-networking-music-chart-debuts/) and ingesting records of [every song streamed on Spotify](http://www.spotifyartists.com/spotify-next-big-sound-artist-analytics/). The growth rate of data has been close to exponential and early adoption of distributed systems has been crucial in keeping up. With over 100 sources tracked coming from both public and proprietary providers, dealing with the heterogenous nature of music analytics has required some novel solutions that go beyond the features that come for free with modern distributed databases.

Next Big Sound has also transitioned between full cloud providers (Slicehost), hybrid providers (Rackspace), and colocation ([Zcolo](http://www.zayo.com/services/colocation)) all while running with a small engineering staff using nothing but open source systems. There was no shortage of lessons learned in this process and we hope others can take something away from our successes and failures.

## <span>Stats</span>

*   <span>40 node Hadoop cluster (150TB capacity), ~60 OpenStack virtual machines</span>

*   <span>10TB of unreplicated, compressed metric data (6TB raw, 4TB indexed)</span>

*   <span>10 engineers, 22 people total</span>

*   <span>5 years development</span>

*   <span>300k timeseries queries per day</span>

*   <span>400G new data per day, at peak</span>

*   <span>Over 1 trillion events recorded for 1M+ artists, including YouTube music video views ([Google I/O talk by Kris Schroder](http://commondatastorage.googleapis.com/io-2013/presentations/1122%20-%20Find%20the%20next%20big%20thing%20with%20the%20YouTube%20Analytics%20API.pdf)<a>), retweets and mentions of artists on Twitter (via</a> [GNIP](http://gnip.com/sources/twitter/)), iTunes purchases, and online radio streams (via [Partnership with Spotify](http://www.spotifyartists.com/spotify-next-big-sound-artist-analytics/))</span>

## <span>Platform</span>

*   <span>**Hosting**: Colocation via [ZColo](http://www.zcolo.com/)</span>

*   <span>**Operating System**: Ubuntu 12.04 LTS for VMs and physical servers</span>

*   <span>**Virtualization**: OpenStack (2x Dell R720 compute nodes, 96GB RAM, 2x Intel 8-core CPU, 15K SAS drives)</span>

*   <span>**Servers**: mainly Dell R420, 32GB RAM, 4x 1TB 7.2K SATA data drives, 2x Intel 4-core CPU</span>

*   <span>**Deployment**: Jenkins</span>

*   <span>**Hadoop**: [Cloudera (CDH 4.3.0)](http://www.cloudera.com/content/cloudera/en/products-and-services/cdh.html)</span>

*   <span>**Configuration**: Chef</span>

*   <span>**Monitoring**: Nagios, Ganglia, Statsd + Graphite, [Zenoss](http://www.zenoss.com/), [Cube](http://square.github.io/cube/), [Lipstick](http://techblog.netflix.com/2013/06/introducing-lipstick-on-apache-pig.html)<a></a></span>

*   <span>**Databases**: HBase, MySQL, MongoDB, Cassandra (dropped recently in favor of HBase)</span>

*   <span>**Languages**: PigLatin + Java for data collection/integration, Python + R + SQL for data analysis, PHP ([Codeigniter](http://ellislab.com/codeigniter) + [Slim](http://www.slimframework.com/)), JavaScript ([AngularJS](http://angularjs.org/) + [Backbone.js](http://backbonejs.org/) + [D3](http://d3js.org/))</span>

*   <span>**Processing**: Impala, Pig, Hive, [Oozie](http://oozie.apache.org/), [RStudio](http://www.rstudio.com/ide/docs/server/configuration)</span>

*   <span>**Networking**: Juniper (10Gig, redundant core layer w/ auto failover, 1 Gig access switches on racks)</span>

## <span>Storage Architecture</span>

Storing timeseries data is relatively simple with distributed systems like Cassandra and HBase, but managing how that data evolves over time is much less so. At Next Big Sound, aggregating data from 100+ sources involves a somewhat traditional Hadoop [ETL](http://en.wikipedia.org/wiki/Extract,_transform,_load) pipeline where raw data is processed via MapReduce applications, Pig, or Hive and results are written to HBase for later retrieval via [Finagle](http://twitter.github.io/finagle/)/[Thrift](http://thrift.apache.org/) services; but with a twist. All data stored within Hadoop/HBase is maintained by a special version control system that supports changes in ETL results over time, allowing for changes in the code that defines the processing pipeline to align with the data itself.

Our "versioning" approach for managing data within Hadoop is an extension to techniques like that used in the [Linked.in data cycle](http://data.linkedin.com/blog/2009/06/building-a-terabyte-scale-data-cycle-at-linkedin-with-hadoop-and-project-voldemort), where results from Hadoop are recomputed, in full, on a recurring basis and atomically swapped out with old result sets in [Voldemort](http://www.project-voldemort.com/voldemort/) in a revertible, versioned way. The difference with our system is that versioning doesn't just occur at the global level, it occurs on a configurable number of deeper levels. This means, for example, that if we're recording the number of retweets by country of an artist on Twitter and we find that our logic for geocoding tweet locations was wrong for a few days, we can simply create new versions of data for just those days rather than rebuilding the entire dataset. Different values will now be associated with each of these new versions and access to each version can be restricted to certain users; developers might see only the newest versions while normal users will see the old version until the new data is confirmed as accurate. "Branching" data like this has been critical for keeping up with changes in data sources and customer requests as well as supporting efficient, incremental data pipelines.

For some extra detail on this system, this diagram portrays some of the key differences described above.

[<span class="full-image-block ssNonEditable"><span>![](https://dl.dropboxusercontent.com/u/65158725/hadoop_arch.png)</span></span>](https://dl.dropboxusercontent.com/u/65158725/hadoop_arch.png)

For even more details, check out [<span>Paper: HBlocks: A Hadoop Subsystem for Iterative Data Engineering</span>](https://dl.dropboxusercontent.com/u/65158725/hblocks.pdf).

The Hadoop infrastructure aside, there are plenty of other challenges we face as well. Mapping the relationships of entities within the music industry across social networks and content distribution sites, building web applications for sorting/searching through millions of datasets, and managing the collection of information over millions of API calls and web crawls all require specialized solutions. We do all of this using only open source tools and a coarse overview of how these systems relate to one another is shown below.

[<span class="full-image-block ssNonEditable"><span>![](https://dl.dropboxusercontent.com/u/65158725/nbs_arch.png)</span></span>](https://dl.dropboxusercontent.com/u/65158725/nbs_arch.png)

<a name="product_challenges"></a>

## <a name="product_challenges"></a>

<a name="product_challenges"></a>

*   <span>**Data Presentation**: The construction of our metric dashboard has always been an ongoing project guided in large part by our customers. Striking the right balance between flexibility and learning curve is a moving target with so many different datasets, and maintaining a coherent JavaScript/PHP codebase to manage it all only gets harder with each new customer and feature. Here are some highlights on how we've dealt with this so far:</span>

    1.  Started as simple Codeigniter app, tried to incorporate Backbone as much as possible, now shifting towards Angular (aggressively)
    2.  Memcache for large static objects (e.g. country to state mappings)
    3.  Local storage for metric data caching and history (i.e. when you reload a page, this is how we know what you were looking at before)
    4.  Graphing all done with D3, previously used [Rickshaw](http://code.shutterstock.com/rickshaw/)

    Also, we don't do anything fancy for feature flags, but we use our basic implementation of them incessantly. They've been one crucial (though sometimes messy) constant in a codebase that's consistently being rewritten and there are many things we would have been unable to do without them.

*   <span>**FIND**: We've invested heavily in building products that give our users the ability to search through our data for interesting artists or songs based on a number of criteria (we call our premier version of this the "FIND" product). As something akin to a stock screener for music, this product lets users sort results after filtering by criteria like "Rap artists within the 30th - 40th percentile of YouTube video views that have never previously appeared on a popularity chart of some kind". The bulk of the infrastructure for this resides in MongoDB where heavily indexed collections are fed by MapReduce jobs and provide nearly instantaneous search capabilities over millions of entities.</span>

    Building this type of product on MongoDB has worked well but indexing limits have been an issue. We're currently exploring systems better suited to this kind of use case, specifically ElasticSearch.

*   <span>**Internal Services**: All metric data used by our products and APIs is served from an internal Finagle service that reads from HBase and MySQL. This service is separated into tiers (all running the same code) where a more critical, low-latency tier is used directly by our products and a second tier capable of much greater throughput, but with a much higher 90th percentile latency, is used by programmatic clients. The latter of the two tends to be much more bursty and unpredictable so using separate tiers like this helps to keep response times as low as possible for customers. This is also a convenient split because it means we can build smaller, virtual machines for the critical tier and then just colocate the other array of Finagle servers on our Hadoop/HBase machines.</span>

*   <span>**Next Big Sound API**: We've gone through a lot of iterations on the primary API we expose externally as well as use internally to power our products. Here are some of the best practices we've found to be the most influential:</span>

    1.  Don't build an API that just exposes methods, build an API that models the entities in your system and let HTTP verbs (eg GET, PUT, POST, HEAD, PATCH, DELETE) imply the behaviors of those entities. This makes the structure of the API much easier to infer and experiment with.
    2.  For methods requiring entity _relationships_, use something like a "fields" parameter for the main entity and let the existence of fields in that parameter imply what relationships actually need to be looked up. For us, this means that our API exposes an "artist" method with a "fields" parameter that would only return return the artist's name if the fields are set as "id,name", but could also return the data about the artists YouTube channel and any videos on it if the fields are set as "id,name,profiles,videos". Fetching the relationships between entities can be expensive so this is a good way to save database queries without having to write a bunch of ugly, combination methods like "getArtistProfiles" or "getArtistVideos".
    3.  Using an externally exposed API to build your own products is always a good idea, but one more subtle advantage of this we've seen is with the on-boarding of new web developers. We used to put a good bit of PHP code between our JS code and the API calls but are now trying to limit interactions to be strictly between JavaScript and the API. This means web devs can focus on the browser code they know so well and it plays much more nicely with their favorite frameworks like Backbone and Angular.
*   <span>**Alerts and Benchmarks**: There are always a lot of things going on in the world of music and one way we try to dig up significant events in all the noise is by benchmarking data across whole platforms (e.g. establishing overall trends in the number of Facebook likes happening every day) and by alerting our customers when the artists they care about see significant spikes in activity. We had some early scalability issues with this, but we've solved most of those by committing to using only Pig/Hadoop for it with results stored in MongoDB or MySQL. The remaining issues center around how to set thresholds for what is "significant," and our biggest take away so far has been that online activity tends to trend and spike globally, so baselines have to take into account as much data as possible without focusing solely on single entities (or artists in our case). Deviations from these more wholistic baselines are a good indicator of _real_ changes in behavior.</span>

*   <span>**Billboard Charts**: We license two charts to Billboard magazine, one for overall popularity of artists online (the [Social 50 Chart](http://www.billboard.com/charts/social-50)) and one basically attempting to predict which artists are most likely to make that list in the future (the [Next Big Sound Chart](http://www.billboard.com/charts/next-big-sound-25)). Calculating these charts doesn't introduce any dramatic scaling challenges since it's just a reverse sort by some computed score across all artists, but producing a polished, de-duplicated, production-worthy list takes some consideration. We have a lot of problems with duplicate or near-duplicate artists within our system as well as the associations of those artists to their online profiles (e.g. Is Justin Bieber's twitter username "justinbieber", "bieber", or "bieberofficial"?). Solving problems like this usually takes some combination of automation and human interaction, but when it's very important not to have false-positives in filtering routines (i.e. removing even a single legitimate artist is a big problem), manual curation is necessary. We've found though that augmenting this curation with systems that remember actions taken and then have the ability to replay those actions is pretty effective and easy to implement.</span>

*   <span>**Predictive Billboard Score**: One of the more interesting analytical results we've ever produced is a [patented algorithm](http://making.nextbigsound.com/post/68287169332/predicting-next-years-breakout-artists) for calculating the likelihood with which an artist will "breakout" in the next year. This process involves the application of a [stochastic gradient boosting](http://astro.temple.edu/~msobel/courses_files/StochasticBoosting(gradient).pdf) technique to predict this likelihood based on the "virality" of different social media numbers. The math aside, this is difficult to do because many of the tools we use for it don't have Hadoop-friendly implementations and we've found that [Mahout](http://mahout.apache.org/) just doesn't work beyond basic applications. Our architecture for a process like this then involves input data sets built and written to MongoDB or Impala by MapReduce jobs, pulled into R via [R-Mongo](http://cran.r-project.org/web/packages/RMongo/index.html) and [R-Impala](http://cran.r-project.org/web/packages/RImpala/index.html), and then processed on a single giant server using some of R's parallel processing libraries like [multicore](http://cran.r-project.org/web/packages/multicore/index.html). Doing most of the heavy lifting with Hadoop and leaving the rest to a single server has some obvious limitations and it's unclear exactly how we'll eventually address them. [RHadoop](https://github.com/RevolutionAnalytics/RHadoop/wiki) might be our best hope.</span>

## <span>Hosting</span>

*   <span>Rolling your own networking solutions sucks. If you're going to do it as a small team, make sure you've got someone dedicated to the task that has done it before and if you don't, find someone. This has pretty easily been our biggest pain point with colocation (and the cause of some pretty scary outages).</span>

*   <span>Moving between hosting providers is always tricky but doesn't have to be risky if you budget for the extra money you'll inevitably spend with machines running in both environments, doing more or less the same thing. Aside from a few unavoidable exceptions, we always ended up duplicating our architecture, in full and usually with some enhancements, within our new provider before shutting any old resources down. Sharing systems between the providers never seems to go well and usually the money saved isn't worth the lack of sleep and uptime.</span>

*   <span>With ~90% of our capacity dedicated to Hadoop/HBase and a really consistent workload, it's hard to beat the price point that came with owning your own servers. Our workloads aren't bursty on a daily basis due to user traffic since that traffic is small compared to all the internal number crunching going on. We do have to increase capacity regularly but doing it as a step function is fine since those increases usually coincide with the acquisition of large customers or data partners. This is why we saved ~$20k/month by moving on to our own hardware.</span>

## <span>Lessons Learned</span>

*   <span>If you're aggregating data from a lot of sources and running even modest transformations on it, you're going to make mistakes. Most of the time, these mistakes will probably be obvious and you can fix them before they make it to production, but the rest of the time, you'll need something in place to handle them once they're there. Here's the sort of scenario we went through way too many times before realizing this:</span>

    1.  Collect terabyte-sized dataset for followers of Twitter artists, load it into the database in a day or two.
    2.  Let customers know the data is ready, high-five ourselves for being awesome.
    3.  (a month later) Wait, why do 20% of all followers live in bumblefuck, Kansas?
    4.  Oh, the code that converts location names to coordinates translates "US" to the coordinates for the middle of the country.
    5.  Ok, well since customers are still using the correct part of the dataset and we can't delete the whole thing, lets just reprocess it, write it to a new table, updated all code everywhere to read from both tables, only take records from the old table if none exist in the new one, and delete the old table after the reprocessing is done (easy right?).
    6.  A hundred lines of hacky spaghetti code (that never go away) and a few days later, job complete.

    There might be a smarter way to do things in some cases like this, but when you run into enough of them it becomes pretty clear you need a good way to update production data that can't just be completely removed and rebuilt. This is why we went through the trouble of building a system for it.

*   <span>Most of our data is built and analyzed using Pig. It is incredibly powerful, virtually all of our engineers know how to use it, and it has functioned very well as the backbone of our storage system. Figuring out what the hell it's doing half the time though is still a work in progress and we've found Lipstick, from Netflix, very helpful for that. We've also found that, in lieu of great visibility, keeping the length of development iterations down with Pig is crucial. Putting time in to intelligently build sample input datasets to longer-running scripts that spawn 20+ Hadoop jobs is a must before trying to test them.</span>

*   <span>We used Cassandra for many years beginning with version 0.4 and despite a terrifying experience early on, it was really awesome by the time we moved away from it. That move really didn't have much to do with Cassandra but was really just a consequence of wanting to use Cloudera's platform as we rebuilt our storage architecture. The lesson we learned though after using it and HBase extensively is that arguing about which to use is probably just a waste of time for most people. They both worked reliably and performed well once we understood how to tune them, and focusing on our data model (eg key compression schemes, capping row sizes, using variable length integers, query access patterns) made a much bigger difference than anything.</span>

The [Next Big Sound Blog](http://blog.nextbigsound.com/) also has some more interesting posts on data mining in the music industry and if you're really into that sort of thing, we're always [hiring](https://www.nextbigsound.com/about#join)!

</div>