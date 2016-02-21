## [Scalability as a Service](/blog/2014/12/22/scalability-as-a-service.html)

    

    

![](http://farm3.static.flickr.com/2661/4188985841_8a7dd5671e.jpg)

_This is a guest post by [Thierry Schellenbach](https://www.linkedin.com/in/thierryschellenbach), CEO [GetStream.io](https://getstream.io/) and author of the open source Stream-Framework, which enables you to build scalable newsfeeds using Cassandra or Redis._

    We first wrote about our     [    newsfeed architecture    ](http://highscalability.com/blog/2013/10/28/design-decisions-for-scaling-your-high-traffic-feeds.html)     on High Scalability in October 2013\. Since then our open source     [    Stream-Framework    ](https://github.com/tschellenbach/Stream-Framework)     grew to be the most used package for building scalable newsfeeds. We’re very grateful to the High Scalability community for all the support.    

In this article I want to highlight the **current trend in our industry of moving  towards externally hosted components**. We’re going to compare the hosted solutions for search, newsfeeds and realtime functionality to their open source alternative. This move towards hosted components means you can add scalable components to your app at a fraction of the effort it took just a few years ago.

###     1.) Search servers    

    ![](https://lh5.googleusercontent.com/yH-um1iRjdvzOgf8nrbkszeo823A_21_koAQGjDThT9R3IsNraLwqpNcUVzf22KDA1-1xvzj8hafTP1EQRJQcKfX4hMrGanrTUNDM3ZIZecRHfOBZOomJhPQuPqwP1TS)        ![](https://lh5.googleusercontent.com/sEW_NXrEWoXVGck2IrfZfkzFKtaING1B-dxvud7RNQ90mSRLPee21ZlQi6X40yq0m-znbla8y9KmXDGQDGuM9H0xG4eV02FEY5zMo5sbXBenLt_neH1epwLKJnjOUd1V)    

    Let’s start with a bit of history about the search market. A long time ago most developers integrated Lucene directly into their app. In 2007 SOLR created an API around the core Lucene search experience. Three years later ElasticSearch was released and enabled you to setup a scalable search cluster.     [    ElasticSearch    ](http://www.elasticsearch.org/)     is a brilliant open search server and I highly recommend you give it a try. A recent entrant to the search market is     [    Algolia    ](https://www.algolia.com/)    , a YC backed startup. Unlike ElasticSearch’s open source solution they offer their proprietary search technology via the hosted model.    

    It is extremely straightforward to setup ElasticSearch. However you’re still adding a new component to your stack. It will take several days to setup monitoring, tweak relevance, add a cloud hosting configuration and setup backups. Afterwards you’ll need to deal with upgrades, instance downtime and incidents.    

    Algolia simply offers a hosted API and takes care of the monitoring and maintenance for you. Furthermore when they make improvements you will immediately benefit from this. You can literally add a scalable and highly performant search component to your app in a few hours, which is amazing.    

    In addition they also offer a     [    distributed search network    ](https://www.algolia.com/dsn)    , which replicates the search index across the world. This approach ensures your users always get the lowest possible latency on their search request.    

    Algolia’s hosted model saves you several days spend on setup and maintenance. Furthermore they optimize performance and global latency to an extent that is simply not feasible when you’re maintaining your own cluster. Another benefit is the excellent set of analytics which they include in the dashboard.    

    The hosted model also benefits Algolia. They can quickly iterate  and instantly push updates to their customers . All this without having to maintain a matrix of compatibility between client and server versions. (Note: There are also many companies providing     [    hosted    ](https://www.google.com/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#safe=off&q=hosted%20elasticsearch)     ElasticSearch solutions.)    

###     2.) Newsfeed/ Activity streams    

    ![](https://lh5.googleusercontent.com/_CtjXWUZ0ce9Q0UKX7MJ-_sWxZOm0bgtisKMr2fEsPZ7VnXj5PN41JBdlM9J2GQD0W3nq1Eh5q_w9v6aPllPE1jtbvvPwk7SSKJwau3W3DGHIjft-XgYc435BeZJpAc3)    

    The second example is about newsfeed development. We know this market as the back of our hand as we’ve authored both the open source Stream Framework and the hosted getstream.io service. Traditionally it takes a few weeks to several months to add a scalable newsfeed to your app. Companies often develop an initial version based on Redis, which eventually becomes expensive to maintain. A famous example of this is     [    Instagram’s switch from Redis to Cassandra    ](http://planetcassandra.org/blog/interview/facebooks-instagram-making-the-switch-to-cassandra-from-redis-a-75-insta-savings/)    . Depending on your experience, the size of your user base and the complexity of the features, it can easily take months to build a solution that works well.    

    Our open source     [    Stream Framework    ](https://github.com/tschellenbach/Stream-Framework)     helps you bring this development time down to a few days or weeks. However setting up and maintaining services such as Cassandra, Redis, Celery and RabbitMQ is still time consuming and adds considerably complexity to your stack, especially components such as Cassandra and Celery have a steep learning curve.    

    The hosted GetStream.io allows you to add scalable newsfeeds to your application in just a few hours. The online     [    getting started    ](https://getstream.io/get_started/#intro)     gets you up to speed in a few clicks. Furthermore you immediately benefit from improvements to the technology and have zero maintenance burden.    

    One advantage of setting up your own hosting is a low latency. For hosted solutions to work well they need to support multiple geographical regions. GetStream.io has recently launched     [    multi region    ](http://blog.getstream.io/post/105515674623/multi-region-support)     support. Over the past years it’s become fairly feasible to do this using tools such as AWS, Cloudformation and Puppet.    

###     3.) Realtime    

    ![](https://lh3.googleusercontent.com/8BZeekEChOfMKOmiFBq2y3kp2XoxdeHcTUfapiWkFylQU1V4aChsCf5JChEpmJsVYFs639HdDk8pCcoN7cEHwbCN1GwuPl3ozEdCQkYDf0kD5EKyQ5z0VLtYO0fidXXj)        ![](https://lh5.googleusercontent.com/r2u-Z2sG215a1aELy8Uq8w8JRNgtxh0qiNf76B5S1ngmM9RcpKkpSHeYGwoOBtjFyvxz_VWMhvptllDGcfw77zOhP2Y2H_Oc6SN66uQKZQwdK_Um0ZckzHmRwUHMwqYN)    

    A third example is the realtime component of your app. A great open source library for this is     [    Faye    ](http://faye.jcoglan.com/)    . It allows you to setup your realtime functionality in a few minutes. However it will take quite some tweaking till you can handle many concurrent realtime connections. Services like     [    PubNub    ](http://www.pubnub.com/)     allow you to get up and running in a few minutes.    

    PubNub distributes the realtime events to 14 data centers. This setup allows your users to always have the lowest latency when listening to realtime changes. It’s an optimization which you will never get around to when setting up your own Faye stack.    

###     Why now    

    There are two trends powering this shift towards hosted components.    

First, developers and companies are becoming **increasingly comfortable with a micro services architecture**. From running and maintaining a service yourself, the step to outsourcing it is very small.

Secondly **cloud providers make it easy to support a multi region setup**. For most components a multi region setup is pretty much a requirement for the hosted model to work. Companies such as Algolia and PubNub take it a step further by offering a full CDN like offering to ensure low latency.

    There are several reasons why some companies are still hesitant to adopt hosted solutions though:    

A.) For one you **can’t customize** the hosted solutions. This is not a major concern as most users of mature projects like ElasticSearch and Faye will never fork the codebase anyhow.

B.) **Security** is another common concern. Often the hosted component providers actually do quite well in this regard. However, there are certain industries such as healthcare or banking where storing data on an external service is simply not an option.

C.) Furthermore there are also large **trust issues** with using external components. Downtime can often cascade and take your application down. On the other hand a specialized, dedicated team working on one component of your app is in a good position to do this well. However I would always check the background of the team and the company to see if you’re comfortable with their experience.

D.) **Vendor lock** is another major concern. In the case of getstream.io we support exports to S3\. If needed you can always fall back to our open source Stream-Framework. When you’re using Algolia from an abstraction layer like Haystack you can always switch to another search engine if you’re no longer happy with Algolia. This is however not the case for all hosted components. Before picking a solution you should make sure you have an exit strategy.

###     Conclusion    

    There is a shift in the industry towards hosted components. This change in the environment is enabling young startups such as Algolia, GetStream.io and PubNub to offer scalable hosted components for your application.    

    There are two consequences of this trend. For one it means you can add scalable components to your app in a fraction of the effort it took just a few years ago. You can focus on what makes your app unique and you can keep the maintenance burden low. Furthermore you’ll often get advanced features such as detailed monitoring and a low global latency which are not feasible to support in a self hosted environment.    

    I hope you enjoyed reading this post and we would love to get your feedback on     [    getstream.io    ](https://getstream.io/)     and the open source     [    Stream-Framework    ](https://github.com/tschellenbach/Stream-Framework)    .    

## Related Articles

*       [On Hacker News](https://news.ycombinator.com/item?id=8784162)     

    