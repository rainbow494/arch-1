## [Is NoSQL a Premature Optimization that's Worse than Death? Or the Lady Gaga of the Database World?](/blog/2011/7/25/is-nosql-a-premature-optimization-thats-worse-than-death-or.html)

    

    

![](http://farm7.static.flickr.com/6007/5971685438_28ff951283_o.jpg)

Michael Stonebraker sure knows how to stir up a storm. Unlike for others, that doesn't make him a troll in my mind, he's [way too accomplished](http://en.wikipedia.org/wiki/Michael_Stonebraker) in the field to be that, but he does have a bit of Barnum & Bailey in him, which serves to get the discussion flowing, and that's a good thing. A lot of previously hidden wisdom and passion unlocks, which we'll try to capture here.

This disturbance in the force is over OldSQL vs NoSQL vs [NewSQL](http://blogs.the451group.com/information_management/2011/04/06/what-we-talk-about-when-we-talk-about-newsql/). Warning, these are not crisp categories, there's leakage all over the place, watch your step:

*   **OldSQL** (Oracle, MySQL, etc) refers to what some want to term as legacy relational database like MySQL, that don't scale out horizontally with aplomb.
*   **NoSQL** (CouchDB, Redis, Cassandra, HBase, MongoDB, Riak, Neo4j, etc) refers to, well, a collection of technologies that aren't OldSQL, these often are designed to scale out horizontally, aren't on ACID, and use schemaless non-relational datamodels.
*   **NewSQL**  (Xeround, Clustrix, NimbusDB, GenieDB, ScaleBase, VoltDB) are databases that preserve SQL, the relational model, ACID, schemas, and are scalable, though not necessarily horizontally (which I don't quite understand). Sharding should be transparent. The general pitch is once you have ACIDy SQL goodness and elasticity, all on commodity hardware, then there's no reason to use NoSQL. 

OK, got it? Then you might be the only one...

The disturbance first started with this article by Derrick Harris, which gets a lot of mileage out of a few quotes by Stonebraker. The short of it is: 

*   [Facebook trapped in MySQL ‘fate worse than death’](http://gigaom.com/cloud/facebook-trapped-in-mysql-fate-worse-than-death/) by Derrick Harris. 
    *   [On Hacker News](http://news.ycombinator.com/item?id=2740432). [On Reddit](http://www.reddit.com/r/programming/comments/ijeot/facebook_trapped_in_mysql_fate_worse_than_death/). [On Slashdot](http://developers.slashdot.org/story/11/07/09/1256241/Facebook-Trapped-In-MySQL-a-Fate-Worse-Than-Death). [On Quora](http://www.quora.com/If-Facebook-is-trapped-by-MySQL-in-a-fate-worse-than-death-what-would-have-been-a-better-option-for-data-persistence-for-the-site-back-in-the-day/answer/Evan-Priestley).  
    *   Facebook is operating a huge, complex MySQL implementation equivalent to ‘a fate worse than death,’ and the only way out is ‘bite the bullet and rewrite everything.’” 
    *   It takes a large amount of work and skill required to make MySQL fit its purposes. Life would be easier if Facebook was built initially on something designed for its purposes.

That prompted a doozy of a response post by Facebook Database Engineer Domas Mituzas, who feels MySQL suits their purposes just fine, given Facebook actually knows what their purposes are:

*   [Stonebraker trapped in Stonebraker ‘fate worse than death’](http://dom.as/2011/07/08/stonebraker-trapped/) by Domas Mituzas
    *   [On Hacker News](http://news.ycombinator.com/item?id=2745460)
    *   Building the web that lasts is completely different task from what academia people imagine building the web is. 
    *   Disks are way more cost efficient, and if used properly can be used to facilitate way more long-term products, not just real time data. 
    *   We balance the workload so that I/O subsystem provides as efficient as possible delivery of the long tail.
    *   The average write transaction at FB is at around 5ms timing, so argument about transactional cost is not that important, as RPC times add up to that quite a bit, and multi-cluster dbms wouldn't reduce the RPC costs.
    *   There’s some operational overhead in handling sharding and availability of MySQL deployments, at large scale it becomes somewhat constant cost, whereas operational efficiency gains are linear.

Then the stream split in this post by Bob Warfield, which riffs on the idea of NoSQL being premature optimization:

*   [NoSQL is a Premature Optimization](http://smoothspan.wordpress.com/2011/07/22/nosql-is-a-premature-optimization/) and [Redux](http://smoothspan.wordpress.com/2011/07/23/biggest-post-ever-redux-nosql-as-a-more-flexible-solution/) by Bob Warfield. 
    *   [On Reddit](http://www.reddit.com/r/programming/comments/ixc2n/nosql_is_a_premature_optimization/)
    *   Starting with NoSQL because you think you might someday have enough traffic and scale to warrant it is a premature optimization, and as such, should be avoided by smaller and even medium sized organizations.  
    *   NoSQL technologies require more investment than Relational to get going with. There is no particular advantage to NoSQL until you reach scales that require it.  In fact it is the opposite, given Point 1. If you are fortunate enough to need the scaling, you will have the time to migrate to NoSQL and it isn’t that expensive or painful to do so when the time comes.

Jeremy Zawodny did not care for this take, and a memo to some of his commenters, if anyone has the chops to have an opinion on all of this, it's Jeremy. The short of it:

*   [NoSQL is What?](http://blog.zawodny.com/2011/07/23/nosql-is-what/) by Jeremy Zawodny. 
    *   [On Hacker News](http://news.ycombinator.com/item?id=2798813)
    *   NoSQL exists because a lot of people find it useful. Features like more domain friendly modeling, built-in sharding, replica sets, a datastructure capabilities are other important features independent of scale. Scaling is just one aspect to consider.
    *   Choose the best tool for the job, not all jobs. SQL isn't cheaper and easier for many requirements. Gross generalizations don't apply. It’s closed-minded to argue that everyone should just start with SQL because it’s easy and worry about the real issues later.

## What does Lady Gaga have to do with all this?

In a parallel subject continuum, I think this comment by [Slappy198s](https://www.youtube.com/user/Slappy198) in [Lady Gaga's solo piano performance of "The Edge Of Glory" on Howard Stern](https://www.youtube.com/watch?v=F_GMgkcc2KM), nicely summarizes the zeitgeist:

> You know, there's a difference between not liking someone's music and not recognizing their talent. If﻿ you can't recognize the fact that Lady GaGa is, in fact, extremely talented in many ways, then you may want to try to look at her with less of a bias. There's plenty of artists I can't stand, but still respect their talent.

Even if you don't like Lada Gaga's schtick, that is a great performance. I get the feeling a lot SQL people don't recognize the talent of NoSQL, whereas NoSQL people are generally use the best tool for the job types who have no problem with you using SQL if that works for you. Which leads to...

## SQL is not the Default Option So it Can't Be Premature Optimization

Jeremy does a good job saying why NoSQL isn't a case of premature optimization. It's not just about scale, it's about solving a problem in a way that is sensible for your job.

There's a deeper fundamental problem I have with the characterization of NoSQL as premature optimization, and it's the same one I have when this phrase is used by programmers. Usually it's just code for I don't like your design choices; I would do it another way so whatever you are doing is wrong. This is a common tactic in in the rough and tumble world of design reviews. It's dismissive. The problem is the person making the assertion probably doesn't have the same experiences as the person doing the implementation and they probably haven't thought through their problem as deeply, so it's often arrogance to say you know what is premature optimization for another's problem.

The arrogance here is seeing a SQL product as the default choice with any deviation requiring a justification. This is not the case. There is no default choice. You don't have to [make excuses](http://www.slideshare.net/toddhoffious/what-should-ido-11) for choosing something different. You just have solve your particular problem. That's it.

[cogman10](http://www.reddit.com/user/cogman10) has a good sense of it:

> Picking Tech A over Tech B is NOT a premature optimization. Would the author claim that "Using InnoDB is a premature optimization because MySQL is better supported!" It is called planning, you do that whenever you write a new application.
> 
> Use the database that best matches your data. If some non-relational database is a perfect match for the data you want to store, by all means use it. Don't give two shits about people like the author that think SQL is the one and only query language. (hell, I wish that SQL would die in flames, but it is heavily built into current business models. Not because it is the best, but because it is common.)
> 
> Insisting on the wrong tech is not premature optimization. It is stupidity. 

[Errr](http://blog.zawodny.com/2011/07/23/nosql-is-what/#comment-2915) also makes some good points:

> NoSQL isn’t just something that you replace your SQL database with when it reaches a certain scale. If that was the only case where NoSQL was useful then it would be just an optimization, but in a lot of cases it’s easier, cheaper and more efficient to implement it as an NoSQL database right away.
> 
> You’re hardly an expert if you’re going to devote more time, effort and money to fit something into a SQL database when all that can be achieved much easier using a NoSQL database.  

## Is Facebook Screwed?

I'll bet on Domas Mituzas knowing what he's doing, so it seems highly unlikely that Facebook is screwed. He's looking from the long view, the perspective of someone who provides a working service for 700+ million people, accessing uncountable bytes of old and new data, that will ideally last forever. That's a completely different perspective from someone coming from a place of building relatively simple and specialized transactional systems.  

Facebook can still do what they need doing, so there's no reason to switch, but it's reasonable to ask if [Facebook](http://royal.pingdom.com/2010/06/18/the-software-behind-facebook/) would be in a better place if they started today with a different technology? Would they prefer to use their new HBase stack, for example? It may not be as clear cut as you think. Take a moment to browse [MySQL at Facebook](http://www.facebook.com/MySQLatFacebook), Facebook's own page on how they use MySQL. What's clear is they are MySQL experts and they deal with other stone cold experts like Percona. They know MySQL backwards and forwards and can make it do what they want. Would you change something that's working to use a new technology that you don't know? 

At Facebook it's not a matter of MySQL or else either. With a policy of using the best technology for the job and deep experience in other stacks, Facebook could switch stacks if they wanted. Not every product at Facebook uses MySQL. Take [Facebook's New Real-Time Messaging System which uses HBase](http://highscalability.com/blog/2010/11/16/facebooks-new-real-time-messaging-system-hbase-to-store-135.html), for example.

Don't believe the switch would be too hard and that they are locked in. Facebook uses APIs that would allow incrementally moving data over to another backend datastore over time. That they aren't doing this means they are satisfied enough with their highly tuned memcached + highly tuned MySQL + highly automated operations approach to stand pat.

As Mituzas hints at, when operating at this scale, all the programs created to manage the system completely dwarf all the other code, regardless of the starting point of the technology, and that's what Facebook has mastered, so the reports of Facebook's impending infrastructure death may be premature.

## **Related Articles**

*   [Facebook on HighScalability](http://highscalability.com/display/Search?searchQuery=facebook&moduleId=4876569)
*   [Facebook on InfoQ](http://www.infoq.com/facebook/)
*   [MySQL at Facebook](http://www.facebook.com/MySQLatFacebook) - Facebook's own MySQL page.
*   [MySQL Performance Blog](http://www.mysqlperformanceblog.com/)
*   [Lessons Learned from Migrating 2+ Billion Documents at Craigslist](http://www.10gen.com/presentation/mongosf2011/craigslist) by Jeremy Zawodny, Craigslist
*   [The NewSQL Movement](http://www.readwriteweb.com/cloud/2011/04/the-newsql-movement.php) by Klint Finley
*   [Pierce Wetter with a good comment](http://blog.zawodny.com/2011/07/23/nosql-is-what/#comment-2964) on why NoSQL is preoptimization.
*   [Replacing Datacenter Oracle with Global Apache Cassandra on AWS](http://www.slideshare.net/adrianco/migrating-netflix-from-oracle-to-global-cassandra#) by Adrian Cockcroft
*   [Rethinking Databases for the Cloud](https://groups.google.com/d/topic/cloud-computing/yGwhY_2DwTk/discussion) on the Cloud Computing news group.
*   [What we talk about when we talk about NewSQL](http://blogs.the451group.com/information_management/2011/04/06/what-we-talk-about-when-we-talk-about-newsql/) by Matthew Aslett
*   [VoltDB Decapitates Six SQL Urban Myths And Delivers Internet Scale OLTP In The Process](http://highscalability.com/blog/2010/6/28/voltdb-decapitates-six-sql-urban-myths-and-delivers-internet.html)
*   [Is VoltDB really as scalable as they claim?](http://www.mysqlperformanceblog.com/2011/02/28/is-voltdb-really-as-scalable-as-they-claim/) by Baron Schwartz.
*   [Exclusive Guest Column from Dr. Michael Stonebraker - High Velocity Transactional Systems](http://www.dbta.com/Articles/Editorial/Trends-and-Applications/Exclusive-Guest-Column-from-Mike-Stonebraker---High-Velocity-Transactional-Systems-76776.aspx)

    