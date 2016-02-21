## [How League of Legends Scaled Chat to 70 million Players - It takes Lots of minions.](/blog/2014/10/13/how-league-of-legends-scaled-chat-to-70-million-players-it-t.html)

    

    

![](https://farm4.staticflickr.com/3931/15319028040_2c841d841e_m.jpg)

        

    How would you build a chat service that needed to handle 7.5 million concurrent players, 27 million daily players, 11K messages per second, and 1 billion events per server, per day?    

    What could generate so much traffic? A game of course.     [    League of Legends    ](http://leagueoflegends.com)    . League of Legends is a team based game, a multiplayer online battle arena (    [    MOBA    ](http://en.wikipedia.org/wiki/Multiplayer_online_battle_arena)    ), where two teams of five battle against each other to control a map and achieve objectives.    

    For teams to succeed communication is crucial. I learned that from     [    Michal Ptaszek    ](https://twitter.com/michalptaszek)    , in an interesting talk on     [    Scaling League of Legends Chat to 70 million Players    ](https://www.youtube.com/watch?v=_jsMpmWaq7I)     (    [    slides    ](http://www.slideshare.net/michalptaszek/strange-loop-presentation)    ) at the     [    Strange Loop 2014    ](https://thestrangeloop.com/)     conference. Michal gave a good example of why multiplayer team games require good communication between players. Imagine a basketball game without the ability to call plays. It wouldn’t work. So that means chat is crucial. Chat is not a Wouldn’t It Be Nice feature.    

    Michal structures the talk in an interesting way, using as a template the expression: Make it work. Make it right. Make it fast.    

    Making it work meant starting with XMPP as a base for chat. WhatsApp followed the     [    same strategy    ](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html)    . Out of the box you get something that works and scales well...until the user count really jumps. To make it right and fast, like WhatsApp, League of Legends found themselves customizing the Erlang VM. Adding lots of monitoring capabilities and performance optimizations to remove the bottlenecks that kill performance at scale.    

    Perhaps the most interesting part of their chat architecture is the use of Riak’s     [    CRDTs    ](http://blog.joeljacobson.com/riak-2-0-data-types/)     (    convergent      replicated data types) to achieve their goal of a shared nothing fueled massively linear horizontal scalability. CRDTs are still esoteric, so you may not have heard of them yet, but they are the next cool thing if you can make them work for you. It’s a different way of thinking about handling writes.    

    Let’s learn how League of Legends built their chat system to handle 70 millions players...    

##     Stats    

*       67 million unique players every month (not counting other services using chat)    

*       27 million daily players    

*       7.5 million concurrent players    

*       1 billion events routed per server, per day, only using 20-30 percent of available CPU and RAM.    

*       11K messages per second    

*       A few hundred chat servers are deployed around the world. Managed by 3 people.    

*       99% uptime.    

##     Platform    

*   [    Ejabberd    ](https://www.ejabberd.im/)     (Erlang based) XMPP server    

*       Riak    

*       Load Balancer    

*       Graphite    

*       Zabbix    

*       Nagios    

*       Jenkins    

*       Confluence    

##     Chat    

*       Can be one-on-one or a group chat.    

*       Chat acts as a presence service and it also maintains a friends list. It knows if a player is online or offline. Are they playing? How long they have been playing? What champion they are playing?    

*       REST APIs provide chat as a backend service for other LoL services. The store talks to chat to verify friendship, for example. Leagues use the chat social graph to group new players together so they can play more often and compete with each other.    

*       Chat must run with a low and stable latency. When chat doesn’t work the game experience is degraded.    

*       Selected     [    XMPP    ](http://xmpp.org/)     as the protocol. Provides messaging, presence information, and maintains contact lists.    

*       For performance reasons and to implement new features they had to diverge from the core XMPP protocol.    

*       Chose Ejabberd as their server. It worked well in the beginning. Erlang has many benefits. It’s built with concurrency, distribution and scalability in mind. It supports hot code reloading so bugs can be patched without stopping a service.    

*       Goal was     [    share nothing architecture    ](http://en.wikipedia.org/wiki/Shared_nothing_architecture)     to achieve massive linear horizontal scalability. Also allows for better error isolation and traceability. Not there yet, but making progress towards this goal.    

*       With hundreds of chat server and only a few people to manage them it’s important the servers be fault tolerant. That not every failure requires human intervention.    

*       Let it crash. Don’t try to slowly recover from a major failure. Instead, restart from a known state. For example, if there’s a large backlog of pending queries to the database the database is restarted. All the new queries are processed in real-time while the queued up queries will be rescheduled for processing.    

*       Each physical server runs Ejabberd and Riak. Riak is used as the database. Servers can be added horizontally as necessary. Ejabberd runs in a cluster. Risk servers also run in their own cluster.    

*       Riak servers use multi datacenter replication to export persistent data to a secondary Riak cluster. Costly ETL queries, like social graph queries, are run on the secondary cluster without interrupting the primary cluster. Backups are also run off the secondary cluster.    

*       Over time, with the necessity to focus on scalability, performance, and fault tolerance, most of Ejabberd was rewritten.    

    *       Rewrote to match their requirements. For example, in LoL friendship is bidirectional only, XMPP allows asymmetrical friendship. XMPP friendship creation required about 16 messages between client and servers, which was a hit on the database.  The new protocol is three messages.    

    *       Removed unnecessary and unwanted code.    

    *       Optimised the protocol itself.    

    *       Wrote a lot of tests to make sure nothing was broken.    

*       Profile code to remove clear bottlenecks.    

*       Avoid shared mutable state so it can scale linearly on a single server as well as in a clustered environment.    

    *       Multi User Chat (MUC). Every chat server can handle hundreds of thousands of connections. For every user connection there’s a session process. Every time a user wanted to update their presence or send a message to a room that event had to go to a single process called MUC router. It would then relay messages to the relevant group chats. This was a clear bottleneck. The solution was to parallelize the routing. Now lookup for a group chat room happens in the user session. Able to use all available cores.    

    *       Every Ejabberd server contains a copy of the session table, which contains a mapping between user IDs and sessions. Sending a message requires looking where the user’s session is in the cluster. Messages were written to the session table. By checking if the session exists, checking presence priority, and some other checks the number of distributed writes was reduced by 96%. Huge win. More users could login much faster and presence updates could occur much more frequently.    

*       Added functionality get more visibility about code that was upgraded into production. Made is so code could be updated on multiple servers at once in a transactional context.    

*       To optimize servers debug functionality was added into the Erlang VM. Needed the ability to see how memory was being used in sessions so they could better optimize memory usage.    

*       Database scalability was designed in from the start. Started with MySQL but hit multiple performance, reliability, and scalability issues. For example, it wasn’t possible to update the schema fast enough to track changes made in code.    

    *       So they chose Riak. Riak is distributed, fault tolerant, key-value store. Truly masterless so there is no single point of failure. Even two servers dying doesn’t degrade service or lose data.    

    *       Had to spent a lot of work in the chat server to implement eventual consistency. Implemented a Ejabberd     [    CRDT library    ](http://blog.joeljacobson.com/riak-2-0-data-types/)     (    convergent     replicated data types). Takes care of all the write conflicts. Tries to converge objects to a stable state.    

    *       How does CRDT work? Instead of appending a new player directly to a friends lists an operational log is maintained for the objects. In the log are entries like “Add Player 1” and “Add Player 2”. Next time the object read the log is consulted and any conflicts are resolved. Then the logs are applied in any order to the object because order doesn’t matter. This way the friend’s list is in a consistent state. The idea is the value is updated in place, instead a long log of operations is built for the objects and the operations are applied whenever the object is read.    

    *       Riak was a huge success. Allowed to scale linearly. Also gave schema flexibility because the objects could be changed on the fly.    

    *       It was a huge mindshift and a lot of work. It changed how they tested services and built tools around it.    

##     Monitoring    

*       Built over 500 real-time counters that are collected every minute and posted into a monitoring system (Graphite, Zabbix, Nagios).    

*       Counters have thresholds which generate alerts when crossed. Problems can be addressed long before players notice any service problems.    

*       For example, a recent client update entered an infinite loop of  broadcasting its own presence. Looking at Graphite it was immediately clear chat servers were hammered with presence updates that began with the new client release.    

##     Implementation Feature Toggles (feature flags)    

*       Have the ability to turn new feature on and off on the fly without having to restart the service.    

*       When a new feature is developed it is surround it by on/off toggles. If a feature causes problems it can be disabled.    

*       Partial deployments. New code can be enabled only for certain users or a percentage of users can have the new code activated. This allows testing potentially dangerous features at far less than full load. If the new feature works it can be turned on for everyone.    

##     Code Reloading on the Fly    

*       One of the great features of Erlang is the ability to hot load new code on the fly.    

*       In one case third party clients (like pidgin) were not well tested and it turned out they were sending different kinds of events than the official client. The fixes could be deployed and integrated into the chat servers without having to restart the whole chat. This means lower downtime for players.    

##     Logging    

*       Log all abnormal conditions like errors and warnings.    

*       Servers also report healthy state which gives the ability to look at the logs and determine if a server is OK because it’s logging users, accepting new connections,  modifying friends lists.    

*       Built-in the ability to enter debug mode for selected user sessions. If there’s a suspicious user or an experimental user (QA testing on production servers), even though there are 100,000 sessions on a chat server only a particular session need be logged. Logging includes XML traffic, events, and metrics. This saves huge on log storage.    

*       With a combination of feature toggles, partial deployment, and selective logging it’s possible to deploy a feature to production servers so it can be tested only by a few people. The relevant logs can be collected and analyzed without noise from all users.    

##     Load Testing Code    

*       Every night an automatic verification system deploys a build of all changes to a load test environment and runs a battery of load tests.    

*       Server health is monitored during the tests. Metrics are pulled and analyzed. A Confluence page is generated with all the metrics and test results. An email summary is sent with a summary of the test results.    

*       Changes can be compared to the previous build so it’s possible to track of how code changes impact tests, finding problems like it was a disaster or changes like memory consumption was improved by X percent.    

##     The Future    

*       Migrating data off of MySQL.    

*       Would like to make chat available outside of the game so players can enjoy their friendships without having to log into the game.    

*       Would like to use the social graph to make the experience better. Analyze player connections and understand how that impacts enjoyment in the game.    

*       Plan to migrate in-game chat to the out-of-game chat servers.    

##     Lessons Learned    

*   **Things will fail**. You don’t have full control. Even if your code is bug free what happens if an ISP’s router dies and 100,000 players are lost at the same time? Be prepared. Make sure your system can handle losing half of your players at once. Or losing 5 out of 20 chat servers without degrading performance.

*       **Scale surfaces bugs**        . Even if a bug only happens once in a billion times that means at the scale of League of Legends the bug will occur once a day. Even unlikely events will happen over time.    

*       **Key to success is understanding what the system is actually doing**        . Know if your system is in a healthy state or about to crash.    

*       **Have a strategy**        .  LoL has a strategy of horizontal scaling their chat service. To support their strategy they did something different. They bought into not just NoSQL with Riak, but they changed their approach to leverage CRDTs to make horizontal scaling as seamless and powerful as possible.    

*       **Make it work**        . Start somewhere and evolve. Ejabberd got them off the ground. Would it have been easier to make a from scratch? Maybe, but they were able to evolve a system to match their requirements as they learned what those requirements were.    

*       **Make it visible**        . Add tracing, logging, alerting, monitoring, graphs, and all that good stuff.    

*       **Make it DevOps**        . LoL added transactions to software updates, feature flags, hot updates, automated load testing, highly configurable log levels, etc. to make the system easier to manage.    

*       **Reduce chatty protocols**        . Tailor functionality to what your system needs. If your system only supported bidirectional friendships, for example, then you don’t a more general and costly protocol.    

*       **Avoid shared mutable state**        . A common tactic, but it’s always interesting to see how shared state causes more and more problems at each step up in scale.    

*       **Leverage your social graph**        . A chat service naturally provides a social graph. That information can be used to both improve user experience and implement novel new features.    

##     Related Articles    

*   [On Reddit](http://www.reddit.com/r/programming/comments/2j95dv/how_league_of_legends_scaled_chat_to_70_million/)

*       [New Facebook Chat Feature Scales To 70 Million Users Using Erlang](http://highscalability.com/blog/2008/5/14/new-facebook-chat-feature-scales-to-70-million-users-using-e.html)         (2008)    

*   [    How HipChat Stores And Indexes Billions Of Messages Using ElasticSearch And Redis    ](http://highscalability.com/blog/2014/1/6/how-hipchat-stores-and-indexes-billions-of-messages-using-el.html)     (uses XMPP)    

*   [    The WhatsApp Architecture Facebook Bought For $19 Billion    ](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html)     (uses XMPP)    

*       [Myth: Eric Brewer On Why Banks Are BASE Not ACID - Availability Is Revenue](http://highscalability.com/blog/2013/5/1/myth-eric-brewer-on-why-banks-are-base-not-acid-availability.html)    

*   [Inside League of Legends, E-Sports’s Main Event](http://www.nytimes.com/2014/10/12/technology/riot-games-league-of-legends-main-attraction-esports.html)

    