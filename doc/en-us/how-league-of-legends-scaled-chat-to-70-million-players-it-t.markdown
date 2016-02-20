## [How League of Legends Scaled Chat to 70 million Players - It takes Lots of minions.](/blog/2014/10/13/how-league-of-legends-scaled-chat-to-70-million-players-it-t.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, October 13, 2014 at 8:56AM</span></div>

<div class="body">

![](https://farm4.staticflickr.com/3931/15319028040_2c841d841e_m.jpg)

<span id="docs-internal-guid-48bcc2e8-07a8-53ac-2ab0-3406066c2d8e"></span>

<span>How would you build a chat service that needed to handle 7.5 million concurrent players, 27 million daily players, 11K messages per second, and 1 billion events per server, per day?</span>

<span>What could generate so much traffic? A game of course.</span> [<span>League of Legends</span>](http://leagueoflegends.com)<span>. League of Legends is a team based game, a multiplayer online battle arena (</span>[<span>MOBA</span>](http://en.wikipedia.org/wiki/Multiplayer_online_battle_arena)<span>), where two teams of five battle against each other to control a map and achieve objectives.</span>

<span>For teams to succeed communication is crucial. I learned that from</span> [<span>Michal Ptaszek</span>](https://twitter.com/michalptaszek)<span>, in an interesting talk on</span> [<span>Scaling League of Legends Chat to 70 million Players</span>](https://www.youtube.com/watch?v=_jsMpmWaq7I) <span>(</span>[<span>slides</span>](http://www.slideshare.net/michalptaszek/strange-loop-presentation)<span>) at the</span> [<span>Strange Loop 2014</span>](https://thestrangeloop.com/) <span>conference. Michal gave a good example of why multiplayer team games require good communication between players. Imagine a basketball game without the ability to call plays. It wouldn’t work. So that means chat is crucial. Chat is not a Wouldn’t It Be Nice feature.</span>

<span>Michal structures the talk in an interesting way, using as a template the expression: Make it work. Make it right. Make it fast.</span>

<span>Making it work meant starting with XMPP as a base for chat. WhatsApp followed the</span> [<span>same strategy</span>](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html)<span>. Out of the box you get something that works and scales well...until the user count really jumps. To make it right and fast, like WhatsApp, League of Legends found themselves customizing the Erlang VM. Adding lots of monitoring capabilities and performance optimizations to remove the bottlenecks that kill performance at scale.</span>

<span>Perhaps the most interesting part of their chat architecture is the use of Riak’s</span> [<span>CRDTs</span>](http://blog.joeljacobson.com/riak-2-0-data-types/) <span>(<span>convergent </span> replicated data types) to achieve their goal of a shared nothing fueled massively linear horizontal scalability. CRDTs are still esoteric, so you may not have heard of them yet, but they are the next cool thing if you can make them work for you. It’s a different way of thinking about handling writes.</span>

<span>Let’s learn how League of Legends built their chat system to handle 70 millions players...</span>

## <span>Stats</span>

*   <span>67 million unique players every month (not counting other services using chat)</span>

*   <span>27 million daily players</span>

*   <span>7.5 million concurrent players</span>

*   <span>1 billion events routed per server, per day, only using 20-30 percent of available CPU and RAM.</span>

*   <span>11K messages per second</span>

*   <span>A few hundred chat servers are deployed around the world. Managed by 3 people.</span>

*   <span>99% uptime.</span>

## <span>Platform</span>

*   [<span>Ejabberd</span>](https://www.ejabberd.im/) <span>(Erlang based) XMPP server</span>

*   <span>Riak</span>

*   <span>Load Balancer</span>

*   <span>Graphite</span>

*   <span>Zabbix</span>

*   <span>Nagios</span>

*   <span>Jenkins</span>

*   <span>Confluence</span>

## <span>Chat</span>

*   <span>Can be one-on-one or a group chat.</span>

*   <span>Chat acts as a presence service and it also maintains a friends list. It knows if a player is online or offline. Are they playing? How long they have been playing? What champion they are playing?</span>

*   <span>REST APIs provide chat as a backend service for other LoL services. The store talks to chat to verify friendship, for example. Leagues use the chat social graph to group new players together so they can play more often and compete with each other.</span>

*   <span>Chat must run with a low and stable latency. When chat doesn’t work the game experience is degraded.</span>

*   <span>Selected</span> [<span>XMPP</span>](http://xmpp.org/) <span>as the protocol. Provides messaging, presence information, and maintains contact lists.</span>

*   <span>For performance reasons and to implement new features they had to diverge from the core XMPP protocol.</span>

*   <span>Chose Ejabberd as their server. It worked well in the beginning. Erlang has many benefits. It’s built with concurrency, distribution and scalability in mind. It supports hot code reloading so bugs can be patched without stopping a service.</span>

*   <span>Goal was</span> [<span>share nothing architecture</span>](http://en.wikipedia.org/wiki/Shared_nothing_architecture) <span>to achieve massive linear horizontal scalability. Also allows for better error isolation and traceability. Not there yet, but making progress towards this goal.</span>

*   <span>With hundreds of chat server and only a few people to manage them it’s important the servers be fault tolerant. That not every failure requires human intervention.</span>

*   <span>Let it crash. Don’t try to slowly recover from a major failure. Instead, restart from a known state. For example, if there’s a large backlog of pending queries to the database the database is restarted. All the new queries are processed in real-time while the queued up queries will be rescheduled for processing.</span>

*   <span>Each physical server runs Ejabberd and Riak. Riak is used as the database. Servers can be added horizontally as necessary. Ejabberd runs in a cluster. Risk servers also run in their own cluster.</span>

*   <span>Riak servers use multi datacenter replication to export persistent data to a secondary Riak cluster. Costly ETL queries, like social graph queries, are run on the secondary cluster without interrupting the primary cluster. Backups are also run off the secondary cluster.</span>

*   <span>Over time, with the necessity to focus on scalability, performance, and fault tolerance, most of Ejabberd was rewritten.</span>

    *   <span>Rewrote to match their requirements. For example, in LoL friendship is bidirectional only, XMPP allows asymmetrical friendship. XMPP friendship creation required about 16 messages between client and servers, which was a hit on the database.  The new protocol is three messages.</span>

    *   <span>Removed unnecessary and unwanted code.</span>

    *   <span>Optimised the protocol itself.</span>

    *   <span>Wrote a lot of tests to make sure nothing was broken.</span>

*   <span>Profile code to remove clear bottlenecks.</span>

*   <span>Avoid shared mutable state so it can scale linearly on a single server as well as in a clustered environment.</span>

    *   <span>Multi User Chat (MUC). Every chat server can handle hundreds of thousands of connections. For every user connection there’s a session process. Every time a user wanted to update their presence or send a message to a room that event had to go to a single process called MUC router. It would then relay messages to the relevant group chats. This was a clear bottleneck. The solution was to parallelize the routing. Now lookup for a group chat room happens in the user session. Able to use all available cores.</span>

    *   <span>Every Ejabberd server contains a copy of the session table, which contains a mapping between user IDs and sessions. Sending a message requires looking where the user’s session is in the cluster. Messages were written to the session table. By checking if the session exists, checking presence priority, and some other checks the number of distributed writes was reduced by 96%. Huge win. More users could login much faster and presence updates could occur much more frequently.</span>

*   <span>Added functionality get more visibility about code that was upgraded into production. Made is so code could be updated on multiple servers at once in a transactional context.</span>

*   <span>To optimize servers debug functionality was added into the Erlang VM. Needed the ability to see how memory was being used in sessions so they could better optimize memory usage.</span>

*   <span>Database scalability was designed in from the start. Started with MySQL but hit multiple performance, reliability, and scalability issues. For example, it wasn’t possible to update the schema fast enough to track changes made in code.</span>

    *   <span>So they chose Riak. Riak is distributed, fault tolerant, key-value store. Truly masterless so there is no single point of failure. Even two servers dying doesn’t degrade service or lose data.</span>

    *   <span>Had to spent a lot of work in the chat server to implement eventual consistency. Implemented a Ejabberd</span> [<span>CRDT library</span>](http://blog.joeljacobson.com/riak-2-0-data-types/) <span>(<span>convergent</span> replicated data types). Takes care of all the write conflicts. Tries to converge objects to a stable state.</span>

    *   <span>How does CRDT work? Instead of appending a new player directly to a friends lists an operational log is maintained for the objects. In the log are entries like “Add Player 1” and “Add Player 2”. Next time the object read the log is consulted and any conflicts are resolved. Then the logs are applied in any order to the object because order doesn’t matter. This way the friend’s list is in a consistent state. The idea is the value is updated in place, instead a long log of operations is built for the objects and the operations are applied whenever the object is read.</span>

    *   <span>Riak was a huge success. Allowed to scale linearly. Also gave schema flexibility because the objects could be changed on the fly.</span>

    *   <span>It was a huge mindshift and a lot of work. It changed how they tested services and built tools around it.</span>

## <span>Monitoring</span>

*   <span>Built over 500 real-time counters that are collected every minute and posted into a monitoring system (Graphite, Zabbix, Nagios).</span>

*   <span>Counters have thresholds which generate alerts when crossed. Problems can be addressed long before players notice any service problems.</span>

*   <span>For example, a recent client update entered an infinite loop of  broadcasting its own presence. Looking at Graphite it was immediately clear chat servers were hammered with presence updates that began with the new client release.</span>

## <span>Implementation Feature Toggles (feature flags)</span>

*   <span>Have the ability to turn new feature on and off on the fly without having to restart the service.</span>

*   <span>When a new feature is developed it is surround it by on/off toggles. If a feature causes problems it can be disabled.</span>

*   <span>Partial deployments. New code can be enabled only for certain users or a percentage of users can have the new code activated. This allows testing potentially dangerous features at far less than full load. If the new feature works it can be turned on for everyone.</span>

## <span>Code Reloading on the Fly</span>

*   <span>One of the great features of Erlang is the ability to hot load new code on the fly.</span>

*   <span>In one case third party clients (like pidgin) were not well tested and it turned out they were sending different kinds of events than the official client. The fixes could be deployed and integrated into the chat servers without having to restart the whole chat. This means lower downtime for players.</span>

## <span>Logging</span>

*   <span>Log all abnormal conditions like errors and warnings.</span>

*   <span>Servers also report healthy state which gives the ability to look at the logs and determine if a server is OK because it’s logging users, accepting new connections,  modifying friends lists.</span>

*   <span>Built-in the ability to enter debug mode for selected user sessions. If there’s a suspicious user or an experimental user (QA testing on production servers), even though there are 100,000 sessions on a chat server only a particular session need be logged. Logging includes XML traffic, events, and metrics. This saves huge on log storage.</span>

*   <span>With a combination of feature toggles, partial deployment, and selective logging it’s possible to deploy a feature to production servers so it can be tested only by a few people. The relevant logs can be collected and analyzed without noise from all users.</span>

## <span>Load Testing Code</span>

*   <span>Every night an automatic verification system deploys a build of all changes to a load test environment and runs a battery of load tests.</span>

*   <span>Server health is monitored during the tests. Metrics are pulled and analyzed. A Confluence page is generated with all the metrics and test results. An email summary is sent with a summary of the test results.</span>

*   <span>Changes can be compared to the previous build so it’s possible to track of how code changes impact tests, finding problems like it was a disaster or changes like memory consumption was improved by X percent.</span>

## <span>The Future</span>

*   <span>Migrating data off of MySQL.</span>

*   <span>Would like to make chat available outside of the game so players can enjoy their friendships without having to log into the game.</span>

*   <span>Would like to use the social graph to make the experience better. Analyze player connections and understand how that impacts enjoyment in the game.</span>

*   <span>Plan to migrate in-game chat to the out-of-game chat servers.</span>

## <span>Lessons Learned</span>

*   **Things will fail**. You don’t have full control. Even if your code is bug free what happens if an ISP’s router dies and 100,000 players are lost at the same time? Be prepared. Make sure your system can handle losing half of your players at once. Or losing 5 out of 20 chat servers without degrading performance.

*   <span>**Scale surfaces bugs**</span><span>. Even if a bug only happens once in a billion times that means at the scale of League of Legends the bug will occur once a day. Even unlikely events will happen over time.</span>

*   <span>**Key to success is understanding what the system is actually doing**</span><span>. Know if your system is in a healthy state or about to crash.</span>

*   <span>**Have a strategy**</span><span>.  LoL has a strategy of horizontal scaling their chat service. To support their strategy they did something different. They bought into not just NoSQL with Riak, but they changed their approach to leverage CRDTs to make horizontal scaling as seamless and powerful as possible.</span>

*   <span>**Make it work**</span><span>. Start somewhere and evolve. Ejabberd got them off the ground. Would it have been easier to make a from scratch? Maybe, but they were able to evolve a system to match their requirements as they learned what those requirements were.</span>

*   <span>**Make it visible**</span><span>. Add tracing, logging, alerting, monitoring, graphs, and all that good stuff.</span>

*   <span>**Make it DevOps**</span><span>. LoL added transactions to software updates, feature flags, hot updates, automated load testing, highly configurable log levels, etc. to make the system easier to manage.</span>

*   <span>**Reduce chatty protocols**</span><span>. Tailor functionality to what your system needs. If your system only supported bidirectional friendships, for example, then you don’t a more general and costly protocol.</span>

*   <span>**Avoid shared mutable state**</span><span>. A common tactic, but it’s always interesting to see how shared state causes more and more problems at each step up in scale.</span>

*   <span>**Leverage your social graph**</span><span>. A chat service naturally provides a social graph. That information can be used to both improve user experience and implement novel new features.</span>

## <span>Related Articles</span>

*   [On Reddit](http://www.reddit.com/r/programming/comments/2j95dv/how_league_of_legends_scaled_chat_to_70_million/)

*   <span>[New Facebook Chat Feature Scales To 70 Million Users Using Erlang](http://highscalability.com/blog/2008/5/14/new-facebook-chat-feature-scales-to-70-million-users-using-e.html)</span> <span>(2008)</span>

*   [<span>How HipChat Stores And Indexes Billions Of Messages Using ElasticSearch And Redis</span>](http://highscalability.com/blog/2014/1/6/how-hipchat-stores-and-indexes-billions-of-messages-using-el.html) <span>(uses XMPP)</span>

*   [<span>The WhatsApp Architecture Facebook Bought For $19 Billion</span>](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html) <span>(uses XMPP)</span>

*   <span>[Myth: Eric Brewer On Why Banks Are BASE Not ACID - Availability Is Revenue](http://highscalability.com/blog/2013/5/1/myth-eric-brewer-on-why-banks-are-base-not-acid-availability.html)</span>

*   [Inside League of Legends, E-Sports’s Main Event](http://www.nytimes.com/2014/10/12/technology/riot-games-league-of-legends-main-attraction-esports.html)

</div>