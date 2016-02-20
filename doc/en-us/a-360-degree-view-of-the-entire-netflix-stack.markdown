## [A 360 Degree View of the Entire Netflix Stack](/blog/2015/11/9/a-360-degree-view-of-the-entire-netflix-stack.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, November 9, 2015 at 8:56AM</span></div>

<div class="body">

![](https://c1.staticflickr.com/1/636/22710586190_e5d99f00d8_o.jpg)

<div class="container">

<div class="row">

<div class="col-md-pull-3 col-md-9">

<div class="clearfix postprofiletop">_This is a guest [repost](http://www.scalescale.com/the-stack-behind-netflix-scaling/) by [Chris Ueland](http://twitter.com/ChrisUeland), creator of [Scale Scale](http://scalescale.com/), with a creative high level view of the Netflix stack._  

<div class="info">

> As we research and dig deeper into scaling, we keep running into Netflix. They are very public with their stories. This post is a round up that we put together with Bryan’s help. We collected info from all over the internet. If you’d like to reach out with more info, we’ll append this post. Otherwise, please enjoy!
> 
> –Chris / ScaleScale / MaxCDN

</div>

</div>

<div class="grid">

<div class="left half"><span style="font-size: small;"><span class="full-image-block ssNonEditable"><span>![](https://c2.staticflickr.com/6/5685/22539311029_449f86e17a_z.jpg?__SQUARESPACE_CACHEVERSION=1447204302683)</span></span>  
</span></div>

</div>

## A look at what we think is interesting about how Netflix Scales

Netflix was founded in 1997 by Marc Randolph and Reed Hastings in Scotts Valley, California and started with 30 employees with 925 working on pay-per-rent.Netflix, now the world’s leading Internet television network, has more than 69 million subscribers in 50 countries enjoying more than ten billion hours of TV shows and movies per month. They are very transparent and publish a lot of information online. We’ve collected it and are sharing the things we think are most interesting:

<span style="font-size: small;"><span class="full-image-block ssNonEditable"><span>![](https://c2.staticflickr.com/6/5755/22480496217_e03746d942_o.png?__SQUARESPACE_CACHEVERSION=1447083309082)</span></span></span>  

## Scaling Culture

NetFlix had a famous presentation about culture. The concepts are about re-thinking HR. A lot of their scaling of people is focused on the principles form this presentation. Here are some sample slides and the presentation. This gives some important context to the culture to understand how they scale their software stack and why it works.

![](http://image.slidesharecdn.com/culture9-090801103430-phpapp02/95/culture-34-638.jpg?cb=1414231623)

![](http://image.slidesharecdn.com/culture9-090801103430-phpapp02/95/culture-81-638.jpg?cb=1414231623)

The Full presentation is [here](http://www.scalescale.com/the-stack-behind-netflix-scaling/#).

## Supporting Many titles with Amazon

Netflix’s infrastructure is on <span class="no-wrap"> Amazon EC2</span> with master copies of digital films from movie studios being stored on <span class="no-wrap">Amazon S3</span>. Each film is encoded into over 50 different versions based on video resolution and audio quality using machines on the cloud. Over 1 petabyte of data is stored on <span class="no-wrap">Amazon</span>. These data are sent to content delivery networks to feed the content to local ISPs.

Netflix uses a number of open-source software at the backend, including Java, MySQL, Gluster, Apache Tomcat, Hive, Chukwa, Cassandra, and Hadoop.

<div class="annotated"><span class="full-image-block ssNonEditable"><span>[![](https://c1.staticflickr.com/1/720/22910079121_024fdbf805_z.jpg?__SQUARESPACE_CACHEVERSION=1447083510278)](http://2zmzkp23rtmx20a8qi485hmn.wpengine.netdna-cdn.com/wp-content/uploads/2015/11/netflix-architecture.png)</span></span></div>

## Supporting Many Devices

The huge amount of codec and bitrate combinations on Netflix means “having to encode the same title 120 different times before it can be delivered to all streaming platforms”.

Although Netflix uses adaptive bitrate streaming technology to adjust the video and audio quality to match the customer’s download speed, they also provide users the ability to choose the quality of video on its website.

You can watch instantly from any Internet-connected device that offers a Netflix app, such as a computer, gaming console, DVD or Blu-ray player, HDTV, set-top box, home theater system, phone or tablet.

They support every title in the following Codecs with different bit rates to make them work on device and connection.

*   Video – [VC-1](https://en.wikipedia.org/wiki/VC-1), [H.264 (AVC)](https://en.wikipedia.org/wiki/H.264), VC-1, [H.263](https://en.wikipedia.org/wiki/H.263), [H.265 (HEVC)](https://en.wikipedia.org/wiki/High_Efficiency_Video_Coding)
*   Audio – [WMA](https://en.wikipedia.org/wiki/Windows_Media_Audio), [Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital), [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus), [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) and [Ogg Vorbis](https://en.wikipedia.org/wiki/Ogg_Vorbis)

### Netflix Open Connect CDN

The Netflix Open Connect CDN is provided for larger ISPs that have over 100,000 subscribers. A specially built low power high storage density appliance caches Netflix content within the ISPs’ data centers to reduce internet transit costs. This appliance runs the <span class="no-wrap"> FreeBSD</span> operating system, <span class="no-wrap"> nginx</span> and the<span class="no-wrap"> Bird Internet routing daemon</span>.

![](http://pbs.twimg.com/media/BxhuMI3IIAAljvI.jpg)  

NetFlix Paris Open Connect – Photo Credit: @dtemkin twitter

Watch the Open Connect video [here](https://www.youtube.com/watch?v=mBCXdaukvcc).

## Scaling Algorithms

In 2009, Netflix did a contest called the [Netflix prize](https://en.wikipedia.org/wiki/Netflix_Prize). They opened up a bunch of anonymized data and allowed teams to try and derive better algorithms. They got a 10.06% uplift of their existing algorithm from the winning team. Netflix was going to run another Netflix Prize but ultimately didn’t because of privacy concerns from the FTC.

The Netflix recommendation system consists of many algorithms. The two core algorithms used in their production system are Restricted Boltzmann Machines (RBM) and a form of Matrix Factorization called SVD++. These two algorithms are combined using a linear blend to produce a single higher accuracy estimate.

Restricted Boltzmann Machines are neural networks that have been modified to work in collaborative filtering. Each user has one RBM with the input node for each representing a movie the user has rated.

SVD++ is an asymmetric form of SVD (Singular Value Decomposition) that makes use of implicit information like RBMs. It was developed by the winning team in the Netflix Prize contest.

On their Engineering blog, the Netflix team covers [Learning a Personalized Homepage](http://techblog.netflix.com/2015/04/learning-personalized-homepage.html)

## Open Source Projects

[https://netflix.github.io/](https://netflix.github.io/). Netflix has a great engineering blog and they recently did a post called [The Evolution of Open Source at Netflix](http://techblog.netflix.com/2015/10/evolution-of-open-source-at-netflix.html).

<div id="ossprojects">

## Big Data

*   [Genie](https://github.com/Netflix/genie) - A powerful, REST-based abstraction to our various data processing frameworks, notably Hadoop.
*   [Inviso](https://github.com/Netflix/inviso) - provides detailed insights into the performance of our Hadoop jobs and clusters.
*   [Lipstick](https://github.com/Netflix/lipstick) - Shows the workflow of Pig jobs in a clear, visual fashion.
*   [Aegisthus](https://github.com/Netflix/aegisthus) - Enables the bulk abstraction of data out of Cassandra for downstream analytic processing.

## Build and Delivery Tools

*   [Nebula](https://nebula-plugins.github.io/) - Effort at Netflix to share its internal build infrastructure.
*   [Aminator](https://github.com/Netflix/aminator) - A tool for creating EBS AMIs.
*   [Asgard](https://github.com/Netflix/asgard) - Web interface for application deployments and cloud management in Amazon Web Services (AWS).

## Common Runtime Services & Libraries

*   [Eureka](https://github.com/Netflix/eureka) - Service discovery for the Netflix cloud platform.
*   [Archaius](https://github.com/Netflix/archaius) - Distributed configuration.
*   [Ribbon](https://github.com/Netflix/ribbon) - Resilent and intelligent inter-process and service communication.
*   [Hystrix](https://github.com/Netflix/hystrix) - Provides reliability beyond single service calls. Isolates latency and fault tolerance at runtime.
*   **[Karyon](https://github.com/Netflix/karyon)** and **[Governator](https://github.com/Netflix/governator) - **JVM container services.
*   [Prana](https://github.com/Netflix/prana)<span style="font-weight: bold;"> sidecar</span> <span style="font-weight: bold;">-</span><span style="font-weight: bold;"> </span>Prana provides proxy capabilities within an instance.
*   [Zuul](https://github.com/Netflix/zuul) - Provides dyamically scriptable proxying at the edge of the cloud deployment.
*   [Fenzo](https://github.com/Netflix/Fenzo) - Provides advanced scheduling and resource management for cloud native frameworks.

## Data Persistence

*   **[EVCache](https://github.com/Netflix/EVCache)** and **[Dynomite](https://github.com/Netflix/dynomite) ****-**For using Memcached and Redis at scale.
*   **[Astyanax](https://github.com/Netflix/astyanax)** and **[Dyno](https://github.com/Netflix/dyno) ****-**Client libraries to better consume datastores in the Cloud.

## Insight, Reliability and Performance

*   [Atlas](https://github.com/Netflix/atlas) - Time-series telemetry platform
*   [Edda](https://github.com/Netflix/edda) - Service to track changes in your cloud
*   [Spectator](https://github.com/Netflix/spectator) - Easy integration of Java application code with Atlas
*   [Vector](https://github.com/Netflix/vector) - Exposes high-resolution host-level metrics with minimal overhead.
*   [Ice](https://github.com/Netflix/ice) - Exposes ongoing cost and and cloud utilization trends.
*   [Simian Army](https://github.com/Netflix/SimianArmy) - Tests Netflix instances for random failures.

## Security<span style="font-size: 12px;"> </span>

*   [Security Monkey](https://github.com/Netflix/security_monkey) - Helps monitor and secure large AWS-based environments.
*   [Scumblr](https://github.com/Netflix/scumblr) - Leverages Internet-wide targeted searches to surface specific security issues for investigation.
*   [MSL](https://github.com/Netflix/MSL) - An extensible and flexible secure messaging protocol that addresses a number of secure communications use cases and requirements.
*   [Falcor](https://netflix.github.io/falcor/) - Represent remote data sources as a single domain model via a virtual JSON graph.
*   [Restify](https://github.com/restify/node-restify) - node.js REST framework specifically meant for web service APIs
*   [RxJS](https://github.com/ReactiveX/RxJS) - A reactive programming library for JavaScript

</div>

</div>

## References

1.  [On HackerNews](https://news.ycombinator.com/item?id=10534219)
2.  [https://en.wikipedia.org/wiki/Netflix ](https://en.wikipedia.org/wiki/Netflix)
3.  [http://gizmodo.com/this-box-can-hold-an-entire-netflix-1592590450 ](http://gizmodo.com/this-box-can-hold-an-entire-netflix-1592590450)
4.  [http://edition.cnn.com/2014/07/21/showbiz/gallery/netflix-history/ ](http://edition.cnn.com/2014/07/21/showbiz/gallery/netflix-history/)
5.  [http://techblog.netflix.com/2015/01/netflixs-viewing-data-how-we-know-where.html ](http://techblog.netflix.com/2015/01/netflixs-viewing-data-how-we-know-where.html)
6.  [https://gigaom.com/2013/03/28/3-shades-of-latency-how-netflix-built-a-data-architecture-around-timeliness/ ](https://gigaom.com/2013/03/28/3-shades-of-latency-how-netflix-built-a-data-architecture-around-timeliness/)
7.  [https://gigaom.com/2015/01/27/netflix-is-revamping-its-data-architecture-for-streaming-movies/ ](https://gigaom.com/2015/01/27/netflix-is-revamping-its-data-architecture-for-streaming-movies/)
8.  [http://stackshare.io/netflix/netflix ](http://stackshare.io/netflix/netflix)
9.  [https://www.quora.com/How-does-the-Netflix-movie-recommendation-algorithm-work ](https://www.quora.com/How-does-the-Netflix-movie-recommendation-algorithm-work)
10.  [https://netflix.github.io/ ](https://netflix.github.io/)

</div>

</div>

</div>