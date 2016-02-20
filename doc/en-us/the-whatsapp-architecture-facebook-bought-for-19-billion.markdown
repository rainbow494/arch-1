## [The WhatsApp Architecture Facebook Bought For $19 Billion](/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, February 26, 2014 at 8:56AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7380/12787493923_e3f0797ea1_m.jpg)

[<span>Rick Reed</span>](http://www.linkedin.com/pub/rick-reed/2/186/427) <span>in an upcoming talk in March titled</span> [<span>That's 'Billion' with a 'B': Scaling to the next level at WhatsApp</span>](http://lanyrd.com/2014/erlangfactory/scwqrt/) <span>reveals some eye popping</span> [<span>WhatsApp</span>](http://en.wikipedia.org/wiki/WhatsApp) <span>stats:</span>

> <span>What has hundreds of nodes, thousands of cores, hundreds of terabytes of RAM, and hopes to serve the billions of smartphones that will soon be a reality around the globe? The Erlang/FreeBSD-based server infrastructure at WhatsApp. We've faced many challenges in meeting the ever-growing demand for our messaging services, but as we continue to push the envelope on size (>8000 cores) and speed (>70M Erlang messages per second) of our serving system.</span>

<span>But since we don’t have that talk yet, let’s take a look at a talk Rick Reed gave two years ago on WhatsApp:</span> [<span>Scaling to Millions of Simultaneous Connections</span>](http://vimeo.com/44312354)<span>.</span>

<span>Having built a high performance messaging bus in C++ while at Yahoo, Rick Reed is not new to the world of high scalability architectures. The founders are also ex-Yahoo guys with not a little experience scaling systems. So WhatsApp comes by their scaling prowess honestly. And since they have a Big Hairy Audacious of Goal of being on every smartphone in the world, which could be as many as 5 billion phones in a few years, they’ll need to make the most of that experience.</span>

<span>Before we get to the facts, let’s digress for a moment on this absolutely fascinating conundrum: How can WhatsApp possibly be worth $19 billion to Facebook?</span>

<span>As a programmer if you ask me if WhatsApp is worth that much I’ll answer expletive no! It’s just sending stuff over a network. Get real. But I’m also the guy that thought we don’t need blogging platforms because how hard is it to remote login to your own server, edit the index.html file with vi, then write your post in HTML? It has taken quite a while for me to realize it’s not the code stupid, it’s getting all those users to love and use your product that is the hard part. You can’t buy love</span>

<span>What is it that makes WhatsApp so valuable? The technology? Ignore all those people who say they could write WhatsApp in a week with PHP. That’s simply not true. It is as we’ll see pretty cool technology. But certainly Facebook has sufficient chops to build WhatsApp if they wished.</span>

<span>Let’s look at features. We know WhatsApp is a</span> [<span>no gimmicks</span>](http://sequoiacapital.tumblr.com/post/77211282835/four-numbers-that-explain-why-facebook-acquired) <span>(no ads, no gimmicks, no games) product with loyal</span> [<span>users from across the world</span>](http://www.wired.com/wiredenterprise/2014/02/whatsapp-rules-rest-world/)<span>. It offers free texting in a cruel world where SMS charges can be abusive. As a sheltered American it has surprised me the most to see how many real people use WhatsApp to really stay in touch with family and friends. So when you get on WhatsApp it’s likely people you know are already on it, since everyone has a phone, which mitigates the empty social network problem. It is aggressively cross platform so everyone you know can use it and it will just work. It “just works” is a phrase often used. It is full featured (shared locations, video, audio, pictures, push-to-talk, voice-messages and photos, read receipt, group-chats, send messages via WiFi, and all can be done regardless of whether the recipient is online or not). It handles the display of native languages well. And using your cell number as identity and your contacts list as a social graph is diabolically simple. There’s no email verification, username and password, and no credit card number required. So it just works.</span>

<span>All impressive, but that can’t be worth $19 billion. Other products can compete on features.</span>

[<span>Google wanted it</span>](https://www.theinformation.com/Google-Was-Willing-to-Beat-Facebook-s-19-Billion-Offer-for-WhatsApp) <span>is a possible reason. It’s</span> [<span>a threat</span>](http://www.forbes.com/sites/parmyolson/2013/12/19/watch-out-facebook-whatsapp-climbs-past-400-million-active-users/)<span>. It’s for the .99 cents a user. Facebook is</span> [<span>just desperate</span>](http://www.foxbusiness.com/technology/2014/02/21/facebook-buying-whatsapp-is-desperate-move/)<span>. It’s for</span> [<span>your phone book</span>](http://www.theregister.co.uk/2014/02/20/facebook_whatsapp_19bn_buy_also_45_for_your_phonebook/)<span>. It’s for the meta-data (even though WhatsApp keeps none).</span>

<span>It’s for the</span> [<span>450 million active users</span>](http://sequoiacapital.tumblr.com/post/77211282835/four-numbers-that-explain-why-facebook-acquired)<span>, with a user based growing at one million users a day, with a potential for a billion users. Facebook needs WhatApp for its next billion users. Certainly that must be part if it. And a cost of about $40 a user doesn’t seem unreasonable, especially with the bulk paid out in stock.  Facebook acquired Instagram for about</span> [<span>$30 per user</span>](http://finance.fortune.cnn.com/2012/04/10/why-instagram-is-worth-1b-to-facebook/)<span>. A Twitter user is</span> [<span>worth $110</span>](http://www.forbes.com/sites/georgeanders/2013/11/07/a-twitter-user-is-worth-110-facebooks-98-linkedins-93/)<span>.</span>

<span>Benedict Evans</span> [<span>makes a great case</span>](https://www.youtube.com/watch?v=VnhbvS0MBXE) <span>that Mobile is a 1+ trillion dollar business, WhatsApp is disrupting the SMS part of this industry, which globally has over $100 billion in revenue, by sending 18 billion SMS messages a day when the global SMS system only sends 20 billion SMS messages a day.  With a fundamental change in the transition from PCs to nearly universal smartphone adoption, the size of the opportunity is a much larger addressable market than where Facebook normally plays.</span>

<span>But Facebook has promised no ads and no interference, so where’s the win?</span>

<span>There’s the interesting development of</span> [<span>business use over mobile</span>](http://www.happymarketer.com/singapore-is-progressively-doing-business-over-whatsapp-are-you)<span>. WhatsApp is used to create group conversations for project teams and venture capitalists carry out deal flow conversations over WhatsApp.</span>

<span>Instagram is used in Kuwait to</span> [<span>sell sheep</span>](http://storify.com/cbccommunity/kuwait-entrepreneurs-use-instagram-to-sell-sheep)<span>.</span>

<span>WeChat, a WhatsApp competitor, launched a taxi-cab hailing service in January. In the first month</span> [<span>21 million cabs were hailed</span>](http://www.techinasia.com/wechat-21-million-taxi-rides-booked/)<span>.</span>

<span>With the future of e-commerce looking like it will be funneled through mobile messaging apps, it must be an e-commerce play?</span>

<span>It’s not just businesses using WhatsApp for applications that were once on the desktop or on the web. Police officers in Spain use WhatsApp to catch criminals. People in Italy use it to organize basketball games.</span>

<span>Commerce and other applications are jumping on to mobile for obvious reasons. Everyone has mobile and these messaging applications are powerful, free, and cheap to use. No longer do you need a desktop or a web application to get things done. A lot of functionality can be overlayed on a messaging app.</span>

<span>So messaging is a threat to Google and Facebook. The desktop is dead. The web is dying. Messaging + mobile is an</span> [<span>entire ecosystem that sidesteps their channel</span>](https://news.ycombinator.com/item?id=7282876)<span>. Messaging has become the [center of engagement](http://cubed.fm/2014/02/cubed-episode-18-the-paradigm-shift-of-mobile-engagement-models/) on mobile, not search, changing how things are found and the very nature of what applications will win the future. We are not just pre-pagerank, we are pre-web.  
</span>

<span>Facebook needs to get into this market or become irrelevant?</span>

<span>With the move to mobile we are seeing deportalization of Facebook. The desktop web interface for Facebook is a portal style interface providing access to all the features made available by the backend. It’s big, complicated, and creaky. Who really loves the Facebook UI?</span>

When Facebook moved to mobile they tried the portal approach and it didn’t work. So they are going with a strategy of smaller, more focussed, [purpose built apps](http://www.theverge.com/2014/1/16/5269664/facebook-plans-suite-of-standalone-mobile-apps-for-2014). Mobile first! There’s only so much you can do on a small screen. On mobile it’s easier to go find a special app than it is to find a menu buried deep within a complicated portal style application.

<span>But Facebook is going one step further. They are not only creating purpose built apps, they are providing multiple competing apps that provide similar functionality and these apps may not even share a backend infrastructure. We see this with Messenger and WhatsApp, Instagram and Facebook’s photo app. Paper is an alternate interface to Facebook that provides very limited functionality, but it does what it does very well.</span>

<span>Conway's law may be operating here. The idea that “organizations which design systems ... are constrained to produce designs which are copies of the communication structures of these organizations.” With a monolithic backend infrastructure we get a Borg-like portal design. The move to mobile frees the organization from this way of thinking. If apps can be built that provide a view of just a slice of the Facebook infrastructure then apps can be built that don’t use Facebook’s infrastructure at all. And if they don't need Facebook's infrastructure then they are free not to be built by Facebook at all. So exactly what is Facebook then?</span>

<span>Facebook CEO Mark Zuckerberg has his own take, saying in a</span> [<span>keynote presentation at the Mobile World Congress</span>](http://techcrunch.com/2014/02/24/whatsapp-is-actually-worth-more-than-19b-says-facebooks-zuckerberg/) <span>that Facebook's acquisition of WhatsApp was closely related to the Internet.org vision:</span>

> <span>The idea is to develop a group of basic internet services that would be free of charge to use — “a 911 for the internet." These could be a social networking service like Facebook, a messaging service, maybe search and other things like weather. Providing a bundle of these free of charge to users will work like a gateway drug of sorts — users who may be able to afford data services and phones these days just don’t see the point of why they would pay for those data services. This would give them some context for why they are important, and that will lead them to paying for more services like this — or so the hope goes.</span>

<span>This is the long play, which is a game that having a huge reservoir of valuable stock allows you to play. </span>

<span>Have we reached a conclusion? I don’t think so. It’s such a stunning dollar amount with such tenuous apparent immediate rewards, that the long term play explanation actually does make some sense. We are still in the very early days of mobile. Nobody knows what the future will look like, so it pays not try to force the future to look like your past. Facebook seems to be doing just that.</span>

<span>But enough of this. How do you support 450 million active users with only 32 engineers? Let’s find out...</span>

## <span>Sources</span>

<span>A warning here, we don’t know a lot about the WhatsApp over all architecture. Just bits and pieces gathered from various sources. Rick Reed’s main talk is about the optimization process used to get to 2 million connections a server while using Erlang, which is interesting, but it’s not a complete architecture talk.</span>

*   [<span>Scaling to Millions of  Simultaneous Connections</span>](http://vimeo.com/44312354) <span>from 2012 (</span>[<span>slides</span>](http://www.erlang-factory.com/upload/presentations/558/efsf2012-whatsapp-scaling.pdf)<span>) by Rick Reed</span>

*   [Erlang Factory interview](http://mirkobonadei.com/interview-with-rick-reed/) with Rick Reed.

*   [<span>An interview with Eugene Fooksman from WhatsApp</span>](http://pdincau.wordpress.com/2013/03/27/an-interview-with-eugene-fooksman-erlang/) <span>on Erlang.</span>

*   <span>[DLD14 - What's Up WhatsApp? (Jan Koum, David Rowan)](http://www.youtube.com/watch?v=WgAtBTpm6Xk&feature=youtu.be)</span>

*   [yowsup](https://github.com/tgalal/yowsup/wiki/Yowsup-Library-Documentation) was an Open Source version of WhatsApp's API. The repository is now unavailable [due to a DMCA takedown](http://openwhatsapp.org/blog/2014/02/13/mass-dmca-takedowns-whatsapp/), but they did document some of WhatsApp internal workings. Diversity in all things.

*   <span>Some sources listed under the</span> <span>_Related Articles_</span><span>.</span>

## <span>Stats</span>

<span>These stats are generally for the current system, not the system we have a talk on. The talk on the current system will include more on hacks for data storage, messaging, meta-clustering, and more BEAM/OTP patches.</span>

*   <span>450 million active users, and reached that number faster than any other company in history.</span>

*   <span>32 engineers, one developer supports 14 million active users</span>

*   <span>50 billion messages every day across seven platforms (inbound + outbound)</span>

*   <span>1+ million people sign up every day</span>

*   <span>$0 invested in advertising</span>

*   <span>$60 million [investment from Sequoia Capital](http://techcrunch.com/2011/04/08/sequoia-whatsapp-funding/); $3.4 billion is the amount Sequoia will make</span>

*   <span>35% is how much of Facebook's cash is [being used for the deal](http://finance.fortune.cnn.com/2014/02/19/facebook-whatsapp-the-other-numbers/)  
    </span>

*   <span>Hundreds of nodes</span>

*   <span>>8000 cores</span>

*   <span>Hundreds of terabytes of RAM</span>

*   <span>>70M Erlang messages per second</span>

*   <span>In 2011 WhatsApp achieved</span> [<span>1 million established tcp sessions</span>](http://blog.whatsapp.com/index.php/2011/09/one-million/) <span>on a single machine with memory and cpu to spare. In 2012 that was pushed to over</span> [<span>2 million tcp connections</span>](http://blog.whatsapp.com/index.php/2012/01/1-million-is-so-2011/)<span>. In 2013 WhatsApp </span>[<span>tweeted out</span>](https://twitter.com/WhatsApp/status/286591302185938946)<span>: On Dec 31st we had a new record day: 7B msgs inbound, 11B msgs outbound = 18 billion total messages processed in one day! Happy 2013!!!</span>

## <span>Platform</span>

### <span>Backend</span>

*   <span>Erlang</span>

*   <span>FreeBSD</span>

*   <span>Yaws, lighttpd</span>

*   <span>PHP</span>

*   <span>Custom patches to</span> [<span>BEAM</span>](http://www.erlang-factory.com/upload/presentations/708/HitchhikersTouroftheBEAM.pdf) <span>(BEAM is like Java’s JVM, but for Erlang)</span>

*   <span>Custom XMPP</span>

*   <span>Hosting [may be in Softlayer](http://www.ip-adress.com/whois/whatsapp.com)  
    </span>

### <span>Frontend</span>

*   <span>Seven client platforms: iPhone, Android, Blackberry, Nokia Symbian S60, Nokia S40, Windows Phone, ?</span>

*   <span>SQLite</span>

### <span>Hardware</span>

*   <span>Standard user facing server:</span>

    *   <span>Dual Westmere Hex-core (24 logical CPUs);</span>

    *   <span>100GB RAM, SSD;</span>

    *   <span>Dual NIC (public user-facing network, private back-end/distribution);</span>

## <span>Product</span>

*   **Focus is on messaging**. Connecting people all over the world, regardless of where they are in the world, without having to pay a lot of money. Founder Jan Koum remembers how difficult it was in 1992 to connect to family all over the world.

*   <span>**Privacy**</span><span>. Shaped by Jan Koum’s experiences growing up in the Ukraine, where nothing was private. Messages are not stored on servers; chat history is not stored; goal is to know as little about users as possible; your name and your gender are not known; chat history is only on your phone.</span>

## <span>General</span>

*   <span>WhatsApp server is almost completely implemented in Erlang.</span>

    *   <span>Server systems that do the backend message routing are done in Erlang.</span>

    *   <span>Great achievement is that the number of active users is managed with a really small server footprint. Team consensus is that it is largely because of Erlang.</span>

    *   Interesting to note [Facebook Chat](http://www.erlang-factory.com/upload/presentations/31/EugeneLetuchy-ErlangatFacebook.pdf) was written in Erlang in 2009, but they went away from it because it was hard to find qualified programmers.

*   <span>WhatsApp server has started from ejabberd</span>

    *   <span>Ejabberd is a famous open source Jabber server written in Erlang.</span>

    *   <span>Originally chosen because its open, had great reviews by developers, ease of start and the promise of Erlang’s long term suitability for large communication system.</span>

    *   <span>The next few years were spent re-writing and modifying quite a few parts of ejabberd, including switching from XMPP to internally developed protocol, restructuring the code base and redesigning some core components, and making lots of important modifications to Erlang VM to optimize server performance.</span>

*   <span>To handle 50 billion messages a day the focus is on making a reliable system that works. Monetization is something to look at later, it’s far far down the road.</span>

*   <span>A primary gauge of system health is message queue length. The message queue length of all the processes on a node is constantly monitored and an alert is sent out if they accumulate backlog beyond a preset threshold. If one or more processes falls behind that is alerted on, which gives a pointer to the next bottleneck to attack.</span>

*   <span>Multimedia messages are sent by uploading the image, audio or video to be sent to an HTTP server and then sending a link to the content along with its Base64 encoded thumbnail (if applicable).</span>

*   <span>Some code is usually pushed every day. Often, it’s multiple times a day, though in general peak traffic times are avoided. Erlang helps being aggressive in getting fixes and features into production. Hot-loading means updates can be pushed without restarts or traffic shifting. Mistakes can usually be undone very quickly, again by hot-loading. Systems tend to be much more loosely-coupled which makes it very easy to roll changes out incrementally.</span>

*   <span>What protocol is used in Whatsapp app? SSL socket to the WhatsApp server pools. All messages are queued on the server until the client reconnects to retrieve the messages. The successful retrieval of a message is sent back to the whatsapp server which forwards this status back to the original sender (which will see that as a "checkmark" icon next to the message). Messages are wiped from the server memory as soon as the client has accepted the message</span>

*   <span>How does the registration process work internally in Whatsapp? WhatsApp used to create a username/password based on the phone IMEI number. This was changed recently. WhatsApp now uses a general request from the app to send a unique 5 digit PIN. WhatsApp will then send a SMS to the indicated phone number (this means the WhatsApp client no longer needs to run on the same phone). Based on the pin number the app then request a unique key from WhatsApp. This key is used as "password" for all future calls. (this "permanent" key is stored on the device). This also means that registering a new device will invalidate the key on the old device.</span>

*   <span>Google’s push service is used on Android.</span>

*   <span>More users on Android. Android is more enjoyable to work with. Developers are able to prototype a feature and push it out to hundreds of millions of users overnight, if there’s an issue it can be fixed quickly. iOS, not so much.</span>

## <span>The Quest for 2+ Million Connections Per Server</span>

*   <span>Experienced lots of user growth, which is a good problem to have, but it also means having to spend money buying more hardware and increased operational complexity of managing all those machines.</span>

*   <span>Need to plan for bumps in traffic. Examples are soccer games and earthquakes in Spain or Mexico. These happen near peak traffic loads, so there needs to be enough spare capacity to handle peaks + bumps. A recent soccer match generated a 35% spike in outbound message rate right at the daily peak.</span>

*   <span>Initial server loading was 200 simultaneous connections per server.</span>

    *   <span>Extrapolated out would mean a lot of servers with the hoped for growth pattern.</span>

    *   <span>Servers were brittle in the face of burst loads. Network glitches and other problems would occur. Needed to decouple components so things weren’t so brittle at high capacity.</span>

    *   <span>Goal was a million connections per server. An ambitious goal given at the time they were running at 200K connections. Running servers with headroom to allow for world events, hardware failures, and other types of glitches would require enough resilience to handle the high usage levels and failures.</span>

## <span>Tools and Techniques Used to Increase Scalability</span>

*   <span>Wrote system activity reporter tool (wsar):</span>

    *   <span>Record system stats across the system, including OS stats, hardware stats, BEAM stats. It was build so it was easy to plugin metrics from other systems, like virtual memory. Track CPU utilization, overall utilization, user time, system time, interrupt time, context switches, system calls, traps, packets sent/received, total count of messages in queues across all processes, busy port events, traffic rate, bytes in/out, scheduling stats, garbage collection stats, words collected, etc.</span>

    *   <span>Initially ran once a minute. As the systems were driven harder one second polling resolution was required because events that happened in the space if a minute were invisible. Really fine grained stats to see how everything is performing.</span>

*   <span>Hardware performance counters in CPU (pmcstat):</span>

    *   <span>See where the CPU is at as a percentage of time. Can tell how much time is being spent executing the emulator loop. In their case it is 16% which tells them that only 16% is executing emulated code so even if you were able to remove all the execution time of all the Erlang code it would only save 16% out of the total runtime. This implies you should focus in other areas to improve efficiency of the system.</span>

*   <span>dtrace, kernel lock-counting, fprof</span>

    *   <span>Dtrace was mostly for debugging, not performance.</span>

    *   <span>Patched BEAM on FreeBSD to include CPU time stamp.</span>

    *   <span>Wrote scripts to create an aggregated view of across all processes to see where routines are spending all the  time.</span>

    *   <span>Biggest win was compiling the emulator with lock counting turned on.</span>

*   <span>Some Issues:</span>

    *   <span>Earlier on saw more time spent in the garbage collections routines, that was brought down.</span>

    *   <span>Saw some issues with the networking stack that was tuned away.</span>

    *   <span>Most issues were with lock contention in the emulator which shows strongly in the output of the lock counting.</span>

*   <span>Measurement:</span>

    *   <span>Synthetic workloads, which means generating traffic from your own test scripts, is of little value for tuning user facing systems at extreme scale.</span>

        *   <span>Worked well for simple interfaces like a user table, generating inserts and reads as quickly as possible.</span>

        *   <span>If supporting a million connections on a server it would take 30 hosts to open enough IP ports to generate enough connections to test just one server. For two million servers that would take 60 hosts. Just difficult to generate that kind of scale.</span>

        *   <span>The type of traffic that is seen during production is difficult to generate. Can guess at a normal workload, but in actuality see networking events, world events, since multi-platform see varying behaviour between clients, and varying by country.</span>

    *   <span>Tee’d workload:</span>

        *   <span>Take normal production traffic and pipe it off to a separate system.</span>

        *   <span>Very useful for systems for which side effects could be constrained. Don’t want to tee traffic and do things that would affect the permanent state of a user or result in multiple messages going to users.</span>

        *   <span>Erlang supports hot loading, so could be under a full production load, have an idea, compile, load the change as the program is running and instantly see if that change is better or worse.</span>

        *   <span>Added knobs to change production load dynamically and see how it would affect performance. Would be tailing the sar output looking at things like CPU usage, VM utilization, listen queue overflows, and turn knobs to see how the system reacted.</span>

    *   <span>True production loads:</span>

        *   <span>Ultimate test. Doing both input work and output work.</span>

        *   <span>Put server in DNS a couple of times so it would get double or triple the normal traffic. Creates issues with TTLs because clients don’t respect DNS TTLs and there’s a delay, so can’t quickly react to getting more traffic than can be dealt with.</span>

        *   <span>IPFW. Forward traffic from one server to another so could give a host exactly the number of desired client connections. A bug caused a kernel panic so that didn't work very well.</span>

*   <span>Results:</span>

    *   <span>Started at 200K simultaneous connections per server.</span>

    *   <span>First bottleneck showed up at 425K. System ran into a lot of contention. Work stopped. Instrumented the scheduler to measure how much useful work is being done, or sleeping, or spinning. Under load it started to hit sleeping locks so 35-45% CPU was being used across the system but the schedulers are at 95% utilization.</span>

    *   <span>The first round of fixes got to over a million connections.</span>

        *   <span>VM usage is at 76%. CPU is at 73%. BEAM emulator running at 45% utilization, which matches closely to user percentage, which is good because the emulator runs as user.</span>

        *   <span>Ordinarily CPU utilization isn’t a good measure of how busy a system is because the scheduler uses CPU.</span>

    *   <span>A month later tackling bottlenecks 2 million connections per server was achieved.</span>

        *   <span>BEAM utilization at 80%, close to where FreeBSD might start paging. CPU is about the same, with double the connections. Scheduler is hitting contention, but running pretty well.</span>

    *   <span>Seemed like a good place to stop so started profiling Erlang code.</span>

        *   <span>Originally had two Erlang processes per connection. Cut that to one.</span>

        *   <span>Did some things with timers.</span>

    *   <span>Peaked at 2.8M connections per server</span>

        *   <span>571k pkts/sec, >200k dist msgs/sec</span>

        *   <span>Made some memory optimizations so VM load was down to 70%.</span>

    *   <span>Tried 3 million connections, but failed.</span>

        *   <span>See long message queues when the system is in trouble. Either a single message queue or a sum of message queues.</span>

        *   <span>Added to BEAM instrumentation on message queue stats per process. How many messages are being sent/received, how fast.</span>

        *   <span>Sampling every 10 seconds, could see a process had 600K messages in its message queue with a dequeue rate of 40K with a delay of 15 seconds. Projected drain time was 41 seconds.</span>

*   <span>Findings:</span>

    *   <span>Erlang + BEAM + their fixes - has awesome SMP scalability. Nearly linear scalability. Remarkable. On a 24-way box can run the system with 85% CPU utilization and it’s keeping up running a production load. It can run like this all day.</span>

        *   <span>Testament to Erlang’s program model.</span>

        *   <span>The longer a server has been up it will accumulate long running connections that are mostly idle so it can handle more connections because these connections are not as busy per connection.</span>

    *   <span>Contention was biggest issue.</span>

        *   <span>Some fixes were in their Erlang code to reduce BEAM’s contention issues.</span>

        *   <span>Some patched to BEAM.</span>

        *   <span>Partitioning workload so work didn’t have to cross processors a lot.</span>

        *   <span>Time-of-day lock. Every time a message is delivered from a port it looks to update the time-of-day which is a single lock across all schedulers which means all CPUs are hitting one lock.</span>

        *   <span>Optimized use of timer wheels. Removed bif timer</span>

        *   <span>Check IO time table grows arithmetically. Created VM thrashing has the hash table would be reallocated at various points. Improved to use geometric allocation of the table.</span>

        *   <span>Added write file that takes a port that you already have open to reduce port contention.</span>

        *   <span>Mseg allocation is single point of contention across all allocators. Make per scheduler.</span>

        *   <span>Lots of port transactions when accepting a connection. Set option to reduce expensive port interactions.</span>

        *   <span>When message queue backlogs became large garbage collection would destabilize the system. So pause GC until the queues shrunk.</span>

    *   <span>Avoiding some common things that come at a price.</span>

        *   <span>Backported a TSE time counter from FreeBSD 9 to 8\. It’s a cheaper to read timer. Fast to get time of day, less expensive than going to a chip.</span>

        *   <span>Backported igp network driver from FreeBSD 9 because having issue with multiple queue on NICs locking up.</span>

        *   <span>Increase number of files and sockets.</span>

        *   <span>Pmcstat showed a lot of time was spent looking up PCBs in the network stack. So bumped up the size of the hash table to make lookups faster.</span>

    *   <span>BEAM Patches</span>

        *   <span>Previously mentioned instrumentation patches. Instrument scheduler to get utilization information, statistics for message queues, number of sleeps, send rates, message counts, etc. Can be done in Erlang code with procinfo, but with a million connections it’s very slow.</span>

        *   <span>Stats collection is very efficient to gather so they can be run in production.</span>

        *   <span>Stats kept at 3 different decay intervals: 1, 10, 100 second intervals. Allows seeing issues over time.</span>

        *   <span>Make lock counting work for larger async thread counts.</span>

        *   <span>Added debug options to debug lock counters.</span>

    *   <span>Tuning</span>

        *   <span>Set the scheduler wake up threshold to low because schedulers would go to sleep and would never wake up.</span>

        *   <span>Prefer mseg allocators over malloc.</span>

        *   <span>Have an allocator per instance per scheduler.</span>

        *   <span>Configure carrier sizes start out big and get bigger. Causes FreeBSD to use super pages. Reduced TLB thrash rate and improves throughput for the same CPU.</span>

        *   <span>Run BEAM at real-time priority so that other things like cron jobs don’t interrupt schedule. Prevents glitches that would cause backlogs of important user traffic.</span>

        *   <span>Patch to dial down spin counts so the scheduler wouldn’t spin.</span>

    *   <span>Mnesia</span>

        *   <span>Prefer os:timestamp to erlang:now.</span>

        *   <span>Using no transactions, but with remote replication ran into a backlog. Parallelized replication for each table to increase throughput.</span>

    *   <span>There are actually lots more changes that were made.</span>

## <span>Lessons</span>

*   <span>**Optimization is dark grungy work suitable only for trolls and engineers**</span><span>. When Rick is going through all the changes that he made to get to 2 million connections a server it was mind numbing. Notice the immense amount of work that went into writing tools, running tests, backporting code, adding gobs of instrumentation to nearly every level of the stack, tuning the system, looking at traces, mucking with very low level details and just trying to understand everything. That’s what it takes to remove the bottlenecks in order to increase performance and scalability to extreme levels.</span>

*   **Get the data you need**. **Write tools. Patch tools. Add knobs.** Ken was relentless in extending the system to get the data they needed, constantly writing tools and scripts to the data they needed to manage and optimize the system. Do whatever it takes.

*   **Measure. Remove Bottlenecks. Test. Repeat.** That’s how you do it.

*   **Erlang rocks!** Erlang continues to prove its capability as a versatile, reliable, high-performance platform. Though personally all the tuning and patching that was required casts some doubt on this claim.

*   **Crack the virality code and profit****.** Virality is an allusive quality, but as WhatsApp shows, if you do figure out, man, it’s worth a lot of money.

*   **Value and employee count are now officially divorced****.** There are a lot of force-multipliers out in the world today. An advanced global telecom infrastructure makes apps like WhatsApp possible. If WhatsApp had to make the network or a phone etc it would never happen. Powerful cheap hardware and Open Source software availability is of course another multiplier. As is being in the right place at the right time with the right product in front of the right buyer.

*   **There’s something to this brutal focus on the user idea****.** WhatsApp is focussed on being a simple messaging app, not being a gaming network, or an advertising network, or a disappearing photos network. That worked for them. It guided their no ads stance, their ability to keep the app simple while adding features, and the overall no brainer it just works philosohpy on any phone.

*   **Limits in the cause of simplicity are OK****.** Your identity is tied to the phone number, so if you change your phone number your identity is gone. This is very un-computer like. But it does make the entire system much simpler in design.

*   **Age ain’t no thing****.** If it was age discrimination that prevented WhatsApp co-founder Brian Acton from getting a job at both Twitter and Facebook in 2009, then shame, shame, shame.

*   **Start simply and then customize****.** When chat was launched initially the server side was based on ejabberd. It’s since been completely rewritten, but that was the initial step in the Erlang direction. The experience with the scalability, reliability, and operability of Erlang in that initial use case led to broader and broader use.

*   **Keep server count low****.** Constantly work to keep server counts as low as possible while leaving enough headroom for events that create short-term spikes in usage. Analyze and optimize until the point of diminishing returns is hit on those efforts and then deploy more hardware.

*   <span>**Purposely overprovision hardware.**</span> <span>This ensures that users have uninterrupted service during their festivities and employees are able to enjoy the holidays without spending the whole time fixing overload issues.</span>

*   **Growth stalls when you charge money****.** Growth was super fast when WhatsApp was free, 10,000 downloads a day in the early days. Then when switching over to paid that declined to 1,000 a day. At the end of the year, after adding picture messaging, they settled on charging a one-time download fee, later modified to an annual payment.

*   **Inspiration comes from the strangest places****.** Experience with forgetting the username and passwords on Skype accounts drove the passion for making the app "just work."

## <span>Related Articles</span>

*   [On Hacker News](https://news.ycombinator.com/item?id=7306066)

*   [<span>Keynote: Benedict Evans - InContext 2014</span>](https://www.youtube.com/watch?v=VnhbvS0MBXE) <span>(related</span> [<span>slides</span>](http://ben-evans.com/presentations/2013/11/5/mobile-is-eating-the-world-autumn-2013-edition)<span>)</span>

*   [<span>Whatsapp and $19bn</span>](http://ben-evans.com/benedictevans/2014/2/19/whatsapp-and-19bn) <span>by Benedict Evans</span>

*   [<span>WhatsApp's blog: The telling diary of a 16 billion dollar startup</span>](http://www.slideshare.net/socialmarketingfella/the-diary-of-a-16-billion-startup-31457350) <span>- nice timeline of events from Andre Bourque.</span>

*   <span>Rick’s Changes to</span> [<span>Erlang on GitHub</span>](https://github.com/reedr)

*   [<span>WhatsApp Blog</span>](http://blog.whatsapp.com/)<span>:</span>

    *   [<span>ONE MILLION!</span>](http://blog.whatsapp.com/index.php/2011/09/one-million/) <span>(</span>[<span>Hacker News</span>](https://news.ycombinator.com/item?id=3028547)<span>)</span>

    *   [<span>1 million is so 2011</span>](http://blog.whatsapp.com/index.php/2012/01/1-million-is-so-2011/)

    *   [<span>400 Million Stories</span>](http://blog.whatsapp.com/index.php/2013/12/400-million-stories/)

*   [<span>WhatsApp: The inside story</span>](http://www.wired.co.uk/news/archive/2014-02/19/whatsapp-exclusive)

*   [<span>The Open Source projects used at WhatsApp</span>](http://www.whatsapp.com/opensource/)

*   [<span>Whatsapp, Facebook, Erlang and realtime messaging: It all started with ejabberd</span>](http://blog.process-one.net/whatsapp-facebook-erlang-and-realtime-messaging-it-all-started-with-ejabberd/)

*   <span>Quora:</span> [<span>How does WhatsApp Work?</span>](http://www.quora.com/How-does-WhatsApp-work-1)<span>,</span> [<span>How does WhatsApp work out of mobile, network?</span>](http://www.quora.com/How-does-WhatsApp-work-out-of-mobile-network)<span>,</span> [<span>How did WhatsApp grow so big?</span>](http://www.quora.com/WhatsApp-Messenger/How-did-WhatsApp-grow-so-big)

*   [<span>WhatsApp is broken, really broken</span>](http://fileperms.org/whatsapp-is-broken-really-broken/) <span>- early security problems</span>

*   [<span>WhatsApp CEO Jan Koum Hates Advertising and the Tech Rumor Mill (Full Dive Video)</span>](http://allthingsd.com/20130510/whatsapp-ceo-jan-koum-hates-advertising-and-the-tech-rumor-mill-full-dive-video/)

*   [<span>Singapore is progressively doing business over WhatsApp. Are You?</span>](http://www.happymarketer.com/singapore-is-progressively-doing-business-over-whatsapp-are-you)

*   [<span>Four Numbers That Explain Why Facebook Acquired WhatsApp</span>](http://sequoiacapital.tumblr.com/post/77211282835/four-numbers-that-explain-why-facebook-acquired)

*   [<span>Announcement from Mark Zuckerberg</span>](https://www.facebook.com/zuck/posts/10101272463589561?stream_ref=10)

*   [<span>A Million-user Comet Application with Mochiweb, Part 3</span>](http://www.metabrew.com/article/a-million-user-comet-application-with-mochiweb-part-3)

*   [<span>Inside Erlang, The Rare Programming Language Behind WhatsApp's Success</span>](http://www.fastcolabs.com/3026758/inside-erlang-the-rare-programming-language-behind-whatsapps-success)

*   <span>[WhatsApp Is Actually Worth More Than $19B, Says Facebook’s Zuckerberg, And It Was Internet.org That Sealed The Deal](http://techcrunch.com/2014/02/24/whatsapp-is-actually-worth-more-than-19b-says-facebooks-zuckerberg/)</span>

*   [<span>Facebook buys Whatsapp for $19 billion: Value and Pricing Perspectives</span>](http://aswathdamodaran.blogspot.in/2014/02/facebook-buys-whatsapp-for-19-billion.html)

*   [<span>Facebook's $19 Billion Craving, Explained By Mark Zuckerberg</span>](http://www.forbes.com/sites/georgeanders/2014/02/19/facebook-justifies-19-billion-by-awe-at-whatsapp-growth/)

*   [<span>IMHO: Lessons learned from WhatsApp</span>](http://blog.mindcrime-ilab.de/2012/12/16/imho-lessons-learned-from-whatsapp/)

*   [<span>You May Not Use WhatsApp, But the Rest of the World Sure Does</span>](http://www.wired.com/wiredenterprise/2014/02/whatsapp-rules-rest-world/)

*   [<span>The WhatsApp Story Challenges Some Of The Valley’s Conventional Wisdom</span>](http://techcrunch.com/2014/02/23/the-whatsapp-story-challenges-the-valleys-conventional-wisdom/)

*   [<span>What WhatsApp Did Right, According to Jan Koum (Video)</span>](http://recode.net/2014/02/20/what-whatsapp-did-right-according-to-jan-koum-video/)

*   [<span>Why did Facebook buy WhatsApp?</span>](http://kottke.org/14/02/why-did-facebook-buy-whatsapp)

*   [<span>Can Someone Explain WhatsApp's Valuation To Me?</span>](http://www.linkedin.com/today/post/article/20140220134541-79695780-can-someone-explain-whatsapp-s-valuation-to-me)

*   [<span>Google’s Unusual Offer to WhatsApp</span>](https://www.theinformation.com/Google-s-Unusual-Offer-to-WhatsApp)<span>.</span>

</div>