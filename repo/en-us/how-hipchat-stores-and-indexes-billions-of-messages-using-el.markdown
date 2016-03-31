## [How HipChat Stores and Indexes Billions of Messages Using ElasticSearch and Redis](/blog/2014/1/6/how-hipchat-stores-and-indexes-billions-of-messages-using-el.html)

    

    

![](http://farm4.staticflickr.com/3820/11648388196_4961154929_m.jpg)

_    This article is from an interview with     [    Zuhaib Siddique    ](http://www.linkedin.com/in/zuhaib)    , a production engineer at     [    HipChat    ](https://www.hipchat.com/)    , makers of                  group chat and IM for teams.    _

    HipChat started in an unusual space, one you might not think would have much promise, enterprise group messaging, but as we are learning there is gold in them there     [    enterprise hills    ](http://www.developereconomics.com/still-building-consumer-apps-enterprise-pays-4x/)    . Which is why Atlassian, makers of well thought of tools like JIRA and Confluence,     [    acquired HipChat in 2012    ](http://blog.hipchat.com/2012/03/07/weve-been-acquired-by-atlassian/)    .    

    And in a tale not often heard, the resources and connections of a larger parent have actually helped HipChat enter an     [    exponential growth cycle    ](https://s3.amazonaws.com/uploads.hipchat.com/10804/368466/10joz8sztfw4dfc/HipChat1B.jpg)    . Having reached the 1.2 billion message storage mark they are now doubling the number of messages sent, stored, and indexed every few months.    

    That kind of growth puts a lot of pressure on a once adequate infrastructure. HipChat exhibited a common scaling pattern. Start simple, experience traffic spikes, and then think what do we do now? Using bigger computers is usually the first and best answer. And they did that. That gave them some breathing room to figure out what to do next. On AWS, after a certain inflection point, you start going Cloud Native, that is, scaling horizontally. And that’s what they did.    

But there's a twist to the story. Security concerns have driven the development of an on-premises version of HipChat in addition to its cloud/SaaS version. We'll talk more about this interesting development in a [post later this week](http://highscalability.com/blog/2014/1/8/under-snowdens-light-software-architecture-choices-become-mu.html).

    While HipChat isn’t Google scale, there is good stuff to learn from HipChat about how they index and search billions of messages in a timely manner, which is the key difference between something like IRC and HipChat. Indexing and storing messages under load while not losing messages is the challenge.    

    This is the road that HipChat took, what can you learn? Let’s see…    

##     Stats    

*       60 messages per second.    

*       1.2 Billion documents stored    

*       4TB of EBS Raid    

*       8 ElasticSearch servers on AWS    

*       26 front end proxy serves. Double that in backend app servers.    

*       18 people    

*       .5 terabytes of search data.    

##     Platform    

*   **Hosting**: AWS EC2 East with 75 Instance currently all Ubuntu 12.04 LTS

*   **Database**: CouchDB currently for Chat History, transitioning to ElasticSearch.  MySQL-RDS for everything else

*   **Caching**: Redis

*   **Search**: ElasticSearch

*   **Queue/Workers server**: Gearman (queue) and [Curler](https://github.com/powdahound/curler), (worker)

*   **Language**: Twisted Python (XMPP Server) and PHP (Web front end)

*   **System Configure**: Open Source Chef + Fabric

*   **Code Deployment**: Capistrano

*   **Monitoring**: Sensu and monit pumping alerts to Pagerduty

*   **Graphing**: statsd + Graphite

##     Product     

*   Traffic is very bursty. On weekends and holidays it will be quiet. During peak load is hundreds of requests per second. Chat messages don't actually make up the majority of traffic ; it's presence information (away, idle, available), people connecting/disconnecting, etc. So 60 messages per second may seem low, but it is just an average.

*       HipChat wants to be your notification center, where you go to collaborate with your team and get all the information coming from your tools and other systems. Helps keeps everyone in the loop. Especially with remote offices.    

*       The big reason to use HipChat over say IRC, is that HipChat stores and indexes every conversation so you can search for them later. Search is emphasized so you can stay inside HipChat. The win for teams of this feature is that you can go back at any time and remember what happened and what you agreed to do. It will also route messages to multiple devices owned by the same user, as well as temporary message caching/retry if your device is unreachable when a message is sent.    

*       Growth is from more customers, but also greater engagement as more customers use the product at each site. Also seeing growth from their API integration.    

*       Storage and searching of messages is the major scalability bottleneck in their system.    

*       HipChat uses XMPP so any XMPP client can connect into the system, which is a huge win for adoption. And they’ve built their own native clients (Windows, Linux, Mac, iOS, Android) with extensions like PDF viewing, custom emoticons, automatic user registration, etc.    

*       Not long ago bringing tools like Wikis into a corporate culture was almost impossible. Now enterprise level tools seem to accepted. Why now?    

    *       Text based communication is now common and accepted. We have texting, IM, and Skype, so using chatting tools is now natural.    

    *       Rise of distributed work teams. Teams are more and more distributed. We can’t just sit down together and get a lecture. There’s a need to document everything which means organizing communication is perceived as a great asset.    

    *       Enhanced features. Features like inline images, animated gifs, etc make it fun, which appeals to wider groups of people.    

*   HipChat has [an API](https://www.hipchat.com/docs/api) which makes it possible to write tools analogous to [IRC bots](http://en.wikipedia.org/wiki/Internet_Relay_Chat_bot). Example use is for Bitbucket commits. At 10:08 developer X commits some code to fix a bug. It is sent out via HipChat with links directly to the code commit and the commit log. All automated. Bitbucket commit hits a web hook and uses one of the addons to post the information. Addons help with writing your own bots. Go to your Bitbucket account. Say I have my API token and I want to post to this API whenever a commit happens. Works similarly for GitHub.

*       Started with Adobe Air on the client side, it leaked memory and would take down machines. So moved to native apps. A pain, but a good pain. They have users across all platforms in all parts of a company. You need to be where users are at. Want users to have a great experience on all OSs. Users are everyone, not just techies.    

##     XMPP Server Architecture    

*       HipChat is based on XMPP, a message is anything inside an     [    XMPP stanza    ](http://xmpp.org/rfcs/rfc3920.html)    , which could be anything from a line of text or long section of log output. They didn’t want to talk about their XMPP architecture, so there aren’t a lot of details.    

*       Instead of using a 3rd party XMPP server, they’ve built their own using using Twisted Python and XMPP libraries. This allows allows the creation of a scalable backend, user management, and easy addition of features without fighting someone elses code base.    

*       RDS on AWS is used for user authentication and other areas where transactions and SQL would be useful. It’s a stable, proven technology. For the on-premises product they use MariaDB.    

*       Redis for caching. Information like which users are in which rooms, presence information, who is online, etc, so it doesn’t matter which XMPP server you connect to, the XMPP server itself is not a limit.    

    *       Pain point is Redis doesn’t have clustering (yet). For high availability a hot/cold model is used so a slave is ready to go. Failover takes about 7 minutes from a master to a slave. Slave promotion is by hand, it’s not automated.    

*       Increased loads exposed a weakness in the proxy server and how many clients they could handle.    

    *       This is a real problem as not losing messages is a high priority. Customers indicate that not dropping messages is more important than low latency. Users would rather get messages late than not at all.    

    *       With 6 XMPP servers the system worked well. As the connection counts increased they started seeing unacceptable latency. Connections don’t just come from clients, but also bots supporting their programmatic interfaces.    

    *       As a first pass they split out front-end servers and app servers. Proxies handle connections and the backend apps processes stanzas. The number of frontend servers is driven by the number of active listening clients and not the number of messages sent. It's a challenge to keep so many connections open while providing timely service.    

    *       After fixing the datastore issues the plan is to look into how to optimize the connection management. Twisted works well, but they have a lot of connections, so have to figure out how to handle that better.    

##     Storage Architecture    

*       At 1 billion messages sent on HipChat while experiencing growth, they pushed the limits of their CouchDB and Lucene solution for storing and searching messages.    

    *       Thought Redis would be the failure point. Thought Couch/Lucene would be good enough. Didn’t do proper capacity planning and looking at message growth rate. Growing faster than they thought and shouldn’t have focussed so much on Redis and focused on data storage instead.    

    *       At that time they believed in scaling by adding more horsepower, moving up to larger and larger Amazon instances. They got to the point where with their growth this approach would only work for another 2 months. So they had to do something different.    

    *       Couch/Lucene was not updated in over year and it couldn’t do faceting. Another reason for something different.    

*       At about half a billion messages on Amazon was a tipping point. With a dedicated server and 200 Gig of RAM their previous architecture might have still worked, but not in a cloud with limited resources.    

*       They wanted to stay on Amazon.    

    *       Like its flexibility. Just spin up a new instance and chug along.    

    *       Amazon’s flakiness makes you develop better. Don’t put all your eggs in one basket, if a node goes down you have deal with it or traffic will be lost for some users.    

    *       Uses a dynamic model. Can lose an instance quickly and bring up new instances. Cloud native type stuff. Kill a node at any time. Kill a Redis master. Recover in 5 minutes. Split across all 4 US-East availability zones currently, but not yet multi-region.    

    *       EBS only let’s you have 1TB of data. Didn’t know about this limit when they ran into it. With Couch they ran into problems with EBS disk size limitations. HipChat’s data was .5 terabytes. To do compaction Couch had to copy data into a compaction file which doubles the space. On a 2 TB RAID hitting limits during compaction on the weekends. Didn’t want a RAID solution.    

*       Amazon DynamoDB wasn’t an option because they are launching a HipChat Server, a hosted service behind the firewall.    

    *       HipChat Server drives decisions on the tech stack. Private versions are a host your own solution. Certain customers can’t use the cloud/SaaS solution, like banks and financial institutions. NSA has spooked international customers. Hired two engineers to create an installable version of the product.    

    *       Redis Cluster can be self-hosted and it works on AWS as can ElasticSearch. Instead of RDS they use MariaDB in the on-premises version.    

    *       Can’t consider a full SaaS solution as that would be a lockin.    

*       Now transitioning to ElasticSearch.    

    *       Moved to ElasticSearch as their storage and search backend because it can eat all the data they can feed it, it is highly available, it scales transparently by simply adding more nodes, it is multi-tenant, it can transparently with sharding and replication handle a node loss, and it’s built on top of Lucene.    

    *       Didn’t really need a MapReduce functionality. Looked into BigCouch and Riak Search (didn’t look well baked). But ES performance on a GET was pretty good. Liked the idea of removing a component, one less thing to troubleshoot. ES HA has made them feel really confident in the solidity of their system.    

    *       Lucene compatible is a big win as all their queries are already Lucene compatible, so it was a natural migration path.    

    *       Customer data is quite varied, ranging from chat logs to images, so response type are all over the place. They need to be able to quickly direct query data from over 1.2 Billion docs.    

    *       In a move that is becoming more and more common, HipChat is using ElasticSearch as their key-value store too, reducing the number of database systems they need and thus reducing overall complexity. Performance and response times were so good they thought why not use it? Between 10ms and 100ms response times. It was beating Couch in certain areas without any caching in front of it. Why have multiple tools?    

    *       With ES a node can go down and nobody notices, you will get an alert about high CPU usage while it is rebalancing, but it keeps chugging along.    

    *       Have 8 ES nodes to handle growth in traffic.    

    *       As with all Java based products JVM tuning can be tricky.    

        *       To use ES must capacity plan for how much heap space you’ll be using    

        *       Test out caching. ES can cache filter results, it is very fast, but you need a lot of heap space. With 22 gigs of heap on 8 boxes, memory was exhausted with caching turned on. So turn off cache unless planned for.    

        *       Had problems with caching because it would hit an out of memory error and then fail. The cluster self healed in minutes. Only a handful of users noticed a problem.    

*       Automated failover on Amazon is problematic because of the network unreliability issues. In a cluster it can cause an election to occur errantly.    

    *       Ran into this problem with ElasticSearch. Originally had 6 ES node running as master electable. A node would run out of memory or hit a GC pause and on top of that a network loss. Then others could no longer see the master, hold an election, and declare itself a master. Flaw in their election architecture that they don’t need a quorum.  So there’s a brain split. Two masters. Caused a lot of problems.    

    *       Solution is to run ElasticSearch masters on dedicated nodes. That’s all they do is be the master. Since then there have been no problems. Masters handle how shard allocation is done, who is primary, and maps where replica shards are located.  Makes rebalancing much easier because the masters can handle all the rebalancing with excellent performance. Can query from any node and will do the internal routing itself.    

*       Uses a month index. Every month is a separate index. Each primary index has 8 shards then two replicas on that. If one node is lost the system still functions.    

*       Not moving RDS into ES. Stuff they need SQL for is staying in RDS/MariaDB, typicall User management data.    

*       A lot of cache in Redis in a master/slave setup until Redis Cluster is released. There’s a Redis stat server, who is in a room, who is offline. Redis history caches the last 75 messages to prevent constantly hitting the database when loading conversation for the first time. Also status for internal status or quick data, like the number of people logged in.     

##     General     

*       Gearman used for async jobs like iOS push and delivering email.    

*       AWS West is used for disaster recovery. Everything is replicated to AWS West.    

*       Chef is used for all configuration. ElasticSearch has a nice cookbook for Chef. Easy to get started. Like Chef because you can start writing Ruby code instead of using a Puppet style DSL. It also has a nice active community.    

*       Acquisition experience.  They now have access to core company assets and talent, but Atlassian doesn’t mess with what works, we bought you for a reason. Can ask internally, for example, how to scale ElasticSearch and when others in Atlassian need help they can pitch in. Good experience overall.    

*       Team structure is flat. Still a small team. Currently have about 18 people. Two people in devops. Developers for platforms, iOS, Android, a few on the server side, and a web development engineer (in France).    

*       Capistrano is used to deploy to all boxes.    

*       Sensu for monitoring apps. Forgot to monitor a ElasticSearch node for heap space and then hit OOM problems without any notifications. Currently at 75% of heap which is the sweet spot they want.    

*       Bamboo is used for continuous integration.    

*       Client releases not formal yet, developer driven. Have a staging area for testing.    

*       Group flags. Can control which groups get a feature to test features to slowly release features and control load on boxes.    

*       Feature flag. Great for protection during the ElasticSearch rollout, for example. If they notice a bug they can turn off a feature and roll back to Couch. Users don’t notice a difference. During the transition phase between Couch and ElasticSearch they have applications replicate to both stores.    

*       New API version will use Oauth so developers can deploy on their own servers using the HipChat API. Having customers use their own servers is a more scalable model.    

##     The Future     

*       Will hit two billion messages in a few months and it’s estimated ElasticSearch can handle about two billion messages. Not sure how to handle the expected increases in load. Expect to go to Amazon West to get more datacenter availability and possibly put more users in different datacenters.    

*       Looking to use AWS’s auto scale capabilities.    

*       Moving into voice, private 1-1 video, audio chat, basic conferencing.    

*       Might use RabbitMQ for messaging in the future.    

*       Greater integration with Confluence (a wiki). Use HipChat to talk then use Confluence page to capture details.    

##     Lessons    

*   **There’s money in them there Enterprise apps**. Selling into an Enterprise can be torture. Long sales cycles means it can be hit or miss. But if you hit it’s a lucrative relationship. So you may want to consider the Enterprise market. Times are changing. Enterprise’s may still be stodgy and slow, but they are also adopting new tools and ways of doing things, and there’s an opportunity there.

*   **Privacy is becoming more important and it impacts your stack choices when selling into the Enterprise**. HipChat is making an on premise version of their product to satisfy customers who don’t trust the public networks or clouds with their data. To a programmer the cloud makes perfect sense as a platform. To an Enterprise the cloud can be evil. What this means is you must make flexible tech stack choices. If you rely 100% on AWS on services it will make it nearly impossible to move your system to another data center. This may not matter for Netflix, but it matters if you want to sell into the Enterprise market.

*   **Scale up to get some breathing space**. While you are waiting to figure out what you are going to next in your architecture, for very little money you can scale-up and give yourselves a few more months of breathing space. 

*   **Pick what can’t fail**. HipChat made never losing user chat history a priority, so their architecture reflects this priority to the point of saving chat to disk and then later reloading when the downed systems recover.

*       **Go native**        . Your customers are on lots of different platforms and a native app would give the best experience. For a startup that’s a lot of resources, too many. So at some point it makes sense to sell to a company with more resources so you can build a better product.    

*       **Feature and group flags make a better release practice**        . If you can select which groups see a feature and if you can turn off features in production and test, then you don’t have to be so afraid of releasing new builds.    

*       **Select technologies you feel really confident with**        . ElasticSearch’s ability to expand horizontally to deal with growth helped HipChat feel very confident in their solution. That users would be happy. And that’s a good feeling.    

*       **Become part of the process flow and you become more valuable and hard to remove**        . HipChat as a natural meeting point between people and tools is also a natural point to write bots that implement all sorts of useful work flow hacks. This makes HipChat a platform play in the enterprise. It enables features that wouldn’t otherwise be buildable. And if you can do that you are hard to get rid of.    

*       **AWS needs a separate node presence bus**        . It’s ridiculous that in a cloud, which is a thing in itself, that machine presence information isn’t available from an objective third party source. If you look at rack it will have often have a separate presence bus so slots can know if other slots are available. This way you don’t have to guess. In the cloud software is using primitive TCP based connection techniques and heartbeats to guess when another node is down, which can lead to split brain problems and data loss on failovers. It’s time to evolve and take huge step towards reliability.    

*       **Product decision drive stack decisions**        . HipChat server drives decisions on the tech stack. Redis Cluster can be self-hosted. Amazon DynamoDB wasn’t an option because they are launching a hosted service behind the firewall.    

*   **Throwing horsepower at a problem will only get you so far**. You need to capacity plan, even in the cloud. Unless your architecture is completely Cloud Native from the start, any architecture will have load inflection points at which their architecture will no longer be able to handle the load. Look at growth rate. Project out. What will break? What will you do about it? And don’t make the same mistake again. How will HipChat deal with 4 billion messages? That’s not clear.

*       **Know the limits of your system**        . EBS has a 1 TB storage limit, which is a lot, but if your storage is approaching that limit there needs to be a plan. Similarly, if your database, like Couch, uses double the disk space during a compaction phase that will impact your limits.    

*   **The world will surprise you**. Six months ago HipChat thought Redis was going to be the weakest link, but it’s still be going strong and it was Couch and EBS that were the weakest link.

##     Related Articles    

*   [On Reddit](http://www.reddit.com/r/programming/comments/1ujzel/scaling_hipchat_with_elasticsearch_and_redis/) / [On Hacker News](https://news.ycombinator.com/item?id=7015179)
*   [    Why are you still building consumer apps? Enterprise pays 4x more!    ](http://www.developereconomics.com/still-building-consumer-apps-enterprise-pays-4x/)
*   [    How HipChat scales to 1 Billion Messages    ](http://blog.hipchat.com/2013/10/16/how-hipchat-scales-to-1-billion-messages/)
*       Reddit on     [    How HipChat scales to 1 billion messages - Zuhaib Siddique, engineer at HipChat, explains how it uses CouchDB, ElasticSearch, and Redis to handle the site’s load    ](http://www.reddit.com/r/programming/comments/1svjjo/how_hipchat_scales_to_1_billion_messages_zuhaib)    .    
*   [    HipChat search now powered by Elasticsearch    ](http://blog.hipchat.com/2013/08/12/hipchat-search-now-powered-by-elasticsearch/)
*   [    HipChat ElasticSearch Meetup Video + Slides    ](http://zuhaiblog.com/2013/12/04/hipchat-elasticsearch-meetup-video-slides/)
*       [SF ElasticSearch Meetup - How HipChat Scaled to 1B Messages](http://www.youtube.com/watch?v=K2P7l98Uj9c)    

    