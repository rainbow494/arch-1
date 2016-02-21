## [The WhatsApp Architecture Facebook Bought For $19 Billion](/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html)

    

    

![](http://farm8.staticflickr.com/7380/12787493923_e3f0797ea1_m.jpg)

[    Rick Reed    ](http://www.linkedin.com/pub/rick-reed/2/186/427)     in an upcoming talk in March titled     [    That's 'Billion' with a 'B': Scaling to the next level at WhatsApp    ](http://lanyrd.com/2014/erlangfactory/scwqrt/)     reveals some eye popping     [    WhatsApp    ](http://en.wikipedia.org/wiki/WhatsApp)     stats:    

>     What has hundreds of nodes, thousands of cores, hundreds of terabytes of RAM, and hopes to serve the billions of smartphones that will soon be a reality around the globe? The Erlang/FreeBSD-based server infrastructure at WhatsApp. We've faced many challenges in meeting the ever-growing demand for our messaging services, but as we continue to push the envelope on size (>8000 cores) and speed (>70M Erlang messages per second) of our serving system.    

    But since we don’t have that talk yet, let’s take a look at a talk Rick Reed gave two years ago on WhatsApp:     [    Scaling to Millions of Simultaneous Connections    ](http://vimeo.com/44312354)    .    

    Having built a high performance messaging bus in C++ while at Yahoo, Rick Reed is not new to the world of high scalability architectures. The founders are also ex-Yahoo guys with not a little experience scaling systems. So WhatsApp comes by their scaling prowess honestly. And since they have a Big Hairy Audacious of Goal of being on every smartphone in the world, which could be as many as 5 billion phones in a few years, they’ll need to make the most of that experience.    

    Before we get to the facts, let’s digress for a moment on this absolutely fascinating conundrum: How can WhatsApp possibly be worth $19 billion to Facebook?    

    As a programmer if you ask me if WhatsApp is worth that much I’ll answer expletive no! It’s just sending stuff over a network. Get real. But I’m also the guy that thought we don’t need blogging platforms because how hard is it to remote login to your own server, edit the index.html file with vi, then write your post in HTML? It has taken quite a while for me to realize it’s not the code stupid, it’s getting all those users to love and use your product that is the hard part. You can’t buy love    

    What is it that makes WhatsApp so valuable? The technology? Ignore all those people who say they could write WhatsApp in a week with PHP. That’s simply not true. It is as we’ll see pretty cool technology. But certainly Facebook has sufficient chops to build WhatsApp if they wished.    

    Let’s look at features. We know WhatsApp is a     [    no gimmicks    ](http://sequoiacapital.tumblr.com/post/77211282835/four-numbers-that-explain-why-facebook-acquired)     (no ads, no gimmicks, no games) product with loyal     [    users from across the world    ](http://www.wired.com/wiredenterprise/2014/02/whatsapp-rules-rest-world/)    . It offers free texting in a cruel world where SMS charges can be abusive. As a sheltered American it has surprised me the most to see how many real people use WhatsApp to really stay in touch with family and friends. So when you get on WhatsApp it’s likely people you know are already on it, since everyone has a phone, which mitigates the empty social network problem. It is aggressively cross platform so everyone you know can use it and it will just work. It “just works” is a phrase often used. It is full featured (shared locations, video, audio, pictures, push-to-talk, voice-messages and photos, read receipt, group-chats, send messages via WiFi, and all can be done regardless of whether the recipient is online or not). It handles the display of native languages well. And using your cell number as identity and your contacts list as a social graph is diabolically simple. There’s no email verification, username and password, and no credit card number required. So it just works.    

    All impressive, but that can’t be worth $19 billion. Other products can compete on features.    

[    Google wanted it    ](https://www.theinformation.com/Google-Was-Willing-to-Beat-Facebook-s-19-Billion-Offer-for-WhatsApp)     is a possible reason. It’s     [    a threat    ](http://www.forbes.com/sites/parmyolson/2013/12/19/watch-out-facebook-whatsapp-climbs-past-400-million-active-users/)    . It’s for the .99 cents a user. Facebook is     [    just desperate    ](http://www.foxbusiness.com/technology/2014/02/21/facebook-buying-whatsapp-is-desperate-move/)    . It’s for     [    your phone book    ](http://www.theregister.co.uk/2014/02/20/facebook_whatsapp_19bn_buy_also_45_for_your_phonebook/)    . It’s for the meta-data (even though WhatsApp keeps none).    

    It’s for the     [    450 million active users    ](http://sequoiacapital.tumblr.com/post/77211282835/four-numbers-that-explain-why-facebook-acquired)    , with a user based growing at one million users a day, with a potential for a billion users. Facebook needs WhatApp for its next billion users. Certainly that must be part if it. And a cost of about $40 a user doesn’t seem unreasonable, especially with the bulk paid out in stock.  Facebook acquired Instagram for about     [    $30 per user    ](http://finance.fortune.cnn.com/2012/04/10/why-instagram-is-worth-1b-to-facebook/)    . A Twitter user is     [    worth $110    ](http://www.forbes.com/sites/georgeanders/2013/11/07/a-twitter-user-is-worth-110-facebooks-98-linkedins-93/)    .    

    Benedict Evans     [    makes a great case    ](https://www.youtube.com/watch?v=VnhbvS0MBXE)     that Mobile is a 1+ trillion dollar business, WhatsApp is disrupting the SMS part of this industry, which globally has over $100 billion in revenue, by sending 18 billion SMS messages a day when the global SMS system only sends 20 billion SMS messages a day.  With a fundamental change in the transition from PCs to nearly universal smartphone adoption, the size of the opportunity is a much larger addressable market than where Facebook normally plays.    

    But Facebook has promised no ads and no interference, so where’s the win?    

    There’s the interesting development of     [    business use over mobile    ](http://www.happymarketer.com/singapore-is-progressively-doing-business-over-whatsapp-are-you)    . WhatsApp is used to create group conversations for project teams and venture capitalists carry out deal flow conversations over WhatsApp.    

    Instagram is used in Kuwait to     [    sell sheep    ](http://storify.com/cbccommunity/kuwait-entrepreneurs-use-instagram-to-sell-sheep)    .    

    WeChat, a WhatsApp competitor, launched a taxi-cab hailing service in January. In the first month     [    21 million cabs were hailed    ](http://www.techinasia.com/wechat-21-million-taxi-rides-booked/)    .    

    With the future of e-commerce looking like it will be funneled through mobile messaging apps, it must be an e-commerce play?    

    It’s not just businesses using WhatsApp for applications that were once on the desktop or on the web. Police officers in Spain use WhatsApp to catch criminals. People in Italy use it to organize basketball games.    

    Commerce and other applications are jumping on to mobile for obvious reasons. Everyone has mobile and these messaging applications are powerful, free, and cheap to use. No longer do you need a desktop or a web application to get things done. A lot of functionality can be overlayed on a messaging app.    

    So messaging is a threat to Google and Facebook. The desktop is dead. The web is dying. Messaging + mobile is an     [    entire ecosystem that sidesteps their channel    ](https://news.ycombinator.com/item?id=7282876)    . Messaging has become the [center of engagement](http://cubed.fm/2014/02/cubed-episode-18-the-paradigm-shift-of-mobile-engagement-models/) on mobile, not search, changing how things are found and the very nature of what applications will win the future. We are not just pre-pagerank, we are pre-web.  
    

    Facebook needs to get into this market or become irrelevant?    

    With the move to mobile we are seeing deportalization of Facebook. The desktop web interface for Facebook is a portal style interface providing access to all the features made available by the backend. It’s big, complicated, and creaky. Who really loves the Facebook UI?    

When Facebook moved to mobile they tried the portal approach and it didn’t work. So they are going with a strategy of smaller, more focussed, [purpose built apps](http://www.theverge.com/2014/1/16/5269664/facebook-plans-suite-of-standalone-mobile-apps-for-2014). Mobile first! There’s only so much you can do on a small screen. On mobile it’s easier to go find a special app than it is to find a menu buried deep within a complicated portal style application.

    But Facebook is going one step further. They are not only creating purpose built apps, they are providing multiple competing apps that provide similar functionality and these apps may not even share a backend infrastructure. We see this with Messenger and WhatsApp, Instagram and Facebook’s photo app. Paper is an alternate interface to Facebook that provides very limited functionality, but it does what it does very well.    

    Conway's law may be operating here. The idea that “organizations which design systems ... are constrained to produce designs which are copies of the communication structures of these organizations.” With a monolithic backend infrastructure we get a Borg-like portal design. The move to mobile frees the organization from this way of thinking. If apps can be built that provide a view of just a slice of the Facebook infrastructure then apps can be built that don’t use Facebook’s infrastructure at all. And if they don't need Facebook's infrastructure then they are free not to be built by Facebook at all. So exactly what is Facebook then?    

    Facebook CEO Mark Zuckerberg has his own take, saying in a     [    keynote presentation at the Mobile World Congress    ](http://techcrunch.com/2014/02/24/whatsapp-is-actually-worth-more-than-19b-says-facebooks-zuckerberg/)     that Facebook's acquisition of WhatsApp was closely related to the Internet.org vision:    

>     The idea is to develop a group of basic internet services that would be free of charge to use — “a 911 for the internet." These could be a social networking service like Facebook, a messaging service, maybe search and other things like weather. Providing a bundle of these free of charge to users will work like a gateway drug of sorts — users who may be able to afford data services and phones these days just don’t see the point of why they would pay for those data services. This would give them some context for why they are important, and that will lead them to paying for more services like this — or so the hope goes.    

    This is the long play, which is a game that having a huge reservoir of valuable stock allows you to play.     

    Have we reached a conclusion? I don’t think so. It’s such a stunning dollar amount with such tenuous apparent immediate rewards, that the long term play explanation actually does make some sense. We are still in the very early days of mobile. Nobody knows what the future will look like, so it pays not try to force the future to look like your past. Facebook seems to be doing just that.    

    But enough of this. How do you support 450 million active users with only 32 engineers? Let’s find out...    

##     Sources    

    A warning here, we don’t know a lot about the WhatsApp over all architecture. Just bits and pieces gathered from various sources. Rick Reed’s main talk is about the optimization process used to get to 2 million connections a server while using Erlang, which is interesting, but it’s not a complete architecture talk.    

*   [    Scaling to Millions of  Simultaneous Connections    ](http://vimeo.com/44312354)     from 2012 (    [    slides    ](http://www.erlang-factory.com/upload/presentations/558/efsf2012-whatsapp-scaling.pdf)    ) by Rick Reed    

*   [Erlang Factory interview](http://mirkobonadei.com/interview-with-rick-reed/) with Rick Reed.

*   [    An interview with Eugene Fooksman from WhatsApp    ](http://pdincau.wordpress.com/2013/03/27/an-interview-with-eugene-fooksman-erlang/)     on Erlang.    

*       [DLD14 - What's Up WhatsApp? (Jan Koum, David Rowan)](http://www.youtube.com/watch?v=WgAtBTpm6Xk&feature=youtu.be)    

*   [yowsup](https://github.com/tgalal/yowsup/wiki/Yowsup-Library-Documentation) was an Open Source version of WhatsApp's API. The repository is now unavailable [due to a DMCA takedown](http://openwhatsapp.org/blog/2014/02/13/mass-dmca-takedowns-whatsapp/), but they did document some of WhatsApp internal workings. Diversity in all things.

*       Some sources listed under the         _Related Articles_        .    

##     Stats    

    These stats are generally for the current system, not the system we have a talk on. The talk on the current system will include more on hacks for data storage, messaging, meta-clustering, and more BEAM/OTP patches.    

*       450 million active users, and reached that number faster than any other company in history.    

*       32 engineers, one developer supports 14 million active users    

*       50 billion messages every day across seven platforms (inbound + outbound)    

*       1+ million people sign up every day    

*       $0 invested in advertising    

*       $60 million [investment from Sequoia Capital](http://techcrunch.com/2011/04/08/sequoia-whatsapp-funding/); $3.4 billion is the amount Sequoia will make    

*       35% is how much of Facebook's cash is [being used for the deal](http://finance.fortune.cnn.com/2014/02/19/facebook-whatsapp-the-other-numbers/)  
        

*       Hundreds of nodes    

*       >8000 cores    

*       Hundreds of terabytes of RAM    

*       >70M Erlang messages per second    

*       In 2011 WhatsApp achieved     [    1 million established tcp sessions    ](http://blog.whatsapp.com/index.php/2011/09/one-million/)     on a single machine with memory and cpu to spare. In 2012 that was pushed to over     [    2 million tcp connections    ](http://blog.whatsapp.com/index.php/2012/01/1-million-is-so-2011/)    . In 2013 WhatsApp     [    tweeted out    ](https://twitter.com/WhatsApp/status/286591302185938946)    : On Dec 31st we had a new record day: 7B msgs inbound, 11B msgs outbound = 18 billion total messages processed in one day! Happy 2013!!!    

##     Platform    

###     Backend    

*       Erlang    

*       FreeBSD    

*       Yaws, lighttpd    

*       PHP    

*       Custom patches to     [    BEAM    ](http://www.erlang-factory.com/upload/presentations/708/HitchhikersTouroftheBEAM.pdf)     (BEAM is like Java’s JVM, but for Erlang)    

*       Custom XMPP    

*       Hosting [may be in Softlayer](http://www.ip-adress.com/whois/whatsapp.com)  
        

###     Frontend    

*       Seven client platforms: iPhone, Android, Blackberry, Nokia Symbian S60, Nokia S40, Windows Phone, ?    

*       SQLite    

###     Hardware    

*       Standard user facing server:    

    *       Dual Westmere Hex-core (24 logical CPUs);    

    *       100GB RAM, SSD;    

    *       Dual NIC (public user-facing network, private back-end/distribution);    

##     Product    

*   **Focus is on messaging**. Connecting people all over the world, regardless of where they are in the world, without having to pay a lot of money. Founder Jan Koum remembers how difficult it was in 1992 to connect to family all over the world.

*       **Privacy**        . Shaped by Jan Koum’s experiences growing up in the Ukraine, where nothing was private. Messages are not stored on servers; chat history is not stored; goal is to know as little about users as possible; your name and your gender are not known; chat history is only on your phone.    

##     General    

*       WhatsApp server is almost completely implemented in Erlang.    

    *       Server systems that do the backend message routing are done in Erlang.    

    *       Great achievement is that the number of active users is managed with a really small server footprint. Team consensus is that it is largely because of Erlang.    

    *   Interesting to note [Facebook Chat](http://www.erlang-factory.com/upload/presentations/31/EugeneLetuchy-ErlangatFacebook.pdf) was written in Erlang in 2009, but they went away from it because it was hard to find qualified programmers.

*       WhatsApp server has started from ejabberd    

    *       Ejabberd is a famous open source Jabber server written in Erlang.    

    *       Originally chosen because its open, had great reviews by developers, ease of start and the promise of Erlang’s long term suitability for large communication system.    

    *       The next few years were spent re-writing and modifying quite a few parts of ejabberd, including switching from XMPP to internally developed protocol, restructuring the code base and redesigning some core components, and making lots of important modifications to Erlang VM to optimize server performance.    

*       To handle 50 billion messages a day the focus is on making a reliable system that works. Monetization is something to look at later, it’s far far down the road.    

*       A primary gauge of system health is message queue length. The message queue length of all the processes on a node is constantly monitored and an alert is sent out if they accumulate backlog beyond a preset threshold. If one or more processes falls behind that is alerted on, which gives a pointer to the next bottleneck to attack.    

*       Multimedia messages are sent by uploading the image, audio or video to be sent to an HTTP server and then sending a link to the content along with its Base64 encoded thumbnail (if applicable).    

*       Some code is usually pushed every day. Often, it’s multiple times a day, though in general peak traffic times are avoided. Erlang helps being aggressive in getting fixes and features into production. Hot-loading means updates can be pushed without restarts or traffic shifting. Mistakes can usually be undone very quickly, again by hot-loading. Systems tend to be much more loosely-coupled which makes it very easy to roll changes out incrementally.    

*       What protocol is used in Whatsapp app? SSL socket to the WhatsApp server pools. All messages are queued on the server until the client reconnects to retrieve the messages. The successful retrieval of a message is sent back to the whatsapp server which forwards this status back to the original sender (which will see that as a "checkmark" icon next to the message). Messages are wiped from the server memory as soon as the client has accepted the message    

*       How does the registration process work internally in Whatsapp? WhatsApp used to create a username/password based on the phone IMEI number. This was changed recently. WhatsApp now uses a general request from the app to send a unique 5 digit PIN. WhatsApp will then send a SMS to the indicated phone number (this means the WhatsApp client no longer needs to run on the same phone). Based on the pin number the app then request a unique key from WhatsApp. This key is used as "password" for all future calls. (this "permanent" key is stored on the device). This also means that registering a new device will invalidate the key on the old device.    

*       Google’s push service is used on Android.    

*       More users on Android. Android is more enjoyable to work with. Developers are able to prototype a feature and push it out to hundreds of millions of users overnight, if there’s an issue it can be fixed quickly. iOS, not so much.    

##     The Quest for 2+ Million Connections Per Server    

*       Experienced lots of user growth, which is a good problem to have, but it also means having to spend money buying more hardware and increased operational complexity of managing all those machines.    

*       Need to plan for bumps in traffic. Examples are soccer games and earthquakes in Spain or Mexico. These happen near peak traffic loads, so there needs to be enough spare capacity to handle peaks + bumps. A recent soccer match generated a 35% spike in outbound message rate right at the daily peak.    

*       Initial server loading was 200 simultaneous connections per server.    

    *       Extrapolated out would mean a lot of servers with the hoped for growth pattern.    

    *       Servers were brittle in the face of burst loads. Network glitches and other problems would occur. Needed to decouple components so things weren’t so brittle at high capacity.    

    *       Goal was a million connections per server. An ambitious goal given at the time they were running at 200K connections. Running servers with headroom to allow for world events, hardware failures, and other types of glitches would require enough resilience to handle the high usage levels and failures.    

##     Tools and Techniques Used to Increase Scalability    

*       Wrote system activity reporter tool (wsar):    

    *       Record system stats across the system, including OS stats, hardware stats, BEAM stats. It was build so it was easy to plugin metrics from other systems, like virtual memory. Track CPU utilization, overall utilization, user time, system time, interrupt time, context switches, system calls, traps, packets sent/received, total count of messages in queues across all processes, busy port events, traffic rate, bytes in/out, scheduling stats, garbage collection stats, words collected, etc.    

    *       Initially ran once a minute. As the systems were driven harder one second polling resolution was required because events that happened in the space if a minute were invisible. Really fine grained stats to see how everything is performing.    

*       Hardware performance counters in CPU (pmcstat):    

    *       See where the CPU is at as a percentage of time. Can tell how much time is being spent executing the emulator loop. In their case it is 16% which tells them that only 16% is executing emulated code so even if you were able to remove all the execution time of all the Erlang code it would only save 16% out of the total runtime. This implies you should focus in other areas to improve efficiency of the system.    

*       dtrace, kernel lock-counting, fprof    

    *       Dtrace was mostly for debugging, not performance.    

    *       Patched BEAM on FreeBSD to include CPU time stamp.    

    *       Wrote scripts to create an aggregated view of across all processes to see where routines are spending all the  time.    

    *       Biggest win was compiling the emulator with lock counting turned on.    

*       Some Issues:    

    *       Earlier on saw more time spent in the garbage collections routines, that was brought down.    

    *       Saw some issues with the networking stack that was tuned away.    

    *       Most issues were with lock contention in the emulator which shows strongly in the output of the lock counting.    

*       Measurement:    

    *       Synthetic workloads, which means generating traffic from your own test scripts, is of little value for tuning user facing systems at extreme scale.    

        *       Worked well for simple interfaces like a user table, generating inserts and reads as quickly as possible.    

        *       If supporting a million connections on a server it would take 30 hosts to open enough IP ports to generate enough connections to test just one server. For two million servers that would take 60 hosts. Just difficult to generate that kind of scale.    

        *       The type of traffic that is seen during production is difficult to generate. Can guess at a normal workload, but in actuality see networking events, world events, since multi-platform see varying behaviour between clients, and varying by country.    

    *       Tee’d workload:    

        *       Take normal production traffic and pipe it off to a separate system.    

        *       Very useful for systems for which side effects could be constrained. Don’t want to tee traffic and do things that would affect the permanent state of a user or result in multiple messages going to users.    

        *       Erlang supports hot loading, so could be under a full production load, have an idea, compile, load the change as the program is running and instantly see if that change is better or worse.    

        *       Added knobs to change production load dynamically and see how it would affect performance. Would be tailing the sar output looking at things like CPU usage, VM utilization, listen queue overflows, and turn knobs to see how the system reacted.    

    *       True production loads:    

        *       Ultimate test. Doing both input work and output work.    

        *       Put server in DNS a couple of times so it would get double or triple the normal traffic. Creates issues with TTLs because clients don’t respect DNS TTLs and there’s a delay, so can’t quickly react to getting more traffic than can be dealt with.    

        *       IPFW. Forward traffic from one server to another so could give a host exactly the number of desired client connections. A bug caused a kernel panic so that didn't work very well.    

*       Results:    

    *       Started at 200K simultaneous connections per server.    

    *       First bottleneck showed up at 425K. System ran into a lot of contention. Work stopped. Instrumented the scheduler to measure how much useful work is being done, or sleeping, or spinning. Under load it started to hit sleeping locks so 35-45% CPU was being used across the system but the schedulers are at 95% utilization.    

    *       The first round of fixes got to over a million connections.    

        *       VM usage is at 76%. CPU is at 73%. BEAM emulator running at 45% utilization, which matches closely to user percentage, which is good because the emulator runs as user.    

        *       Ordinarily CPU utilization isn’t a good measure of how busy a system is because the scheduler uses CPU.    

    *       A month later tackling bottlenecks 2 million connections per server was achieved.    

        *       BEAM utilization at 80%, close to where FreeBSD might start paging. CPU is about the same, with double the connections. Scheduler is hitting contention, but running pretty well.    

    *       Seemed like a good place to stop so started profiling Erlang code.    

        *       Originally had two Erlang processes per connection. Cut that to one.    

        *       Did some things with timers.    

    *       Peaked at 2.8M connections per server    

        *       571k pkts/sec, >200k dist msgs/sec    

        *       Made some memory optimizations so VM load was down to 70%.    

    *       Tried 3 million connections, but failed.    

        *       See long message queues when the system is in trouble. Either a single message queue or a sum of message queues.    

        *       Added to BEAM instrumentation on message queue stats per process. How many messages are being sent/received, how fast.    

        *       Sampling every 10 seconds, could see a process had 600K messages in its message queue with a dequeue rate of 40K with a delay of 15 seconds. Projected drain time was 41 seconds.    

*       Findings:    

    *       Erlang + BEAM + their fixes - has awesome SMP scalability. Nearly linear scalability. Remarkable. On a 24-way box can run the system with 85% CPU utilization and it’s keeping up running a production load. It can run like this all day.    

        *       Testament to Erlang’s program model.    

        *       The longer a server has been up it will accumulate long running connections that are mostly idle so it can handle more connections because these connections are not as busy per connection.    

    *       Contention was biggest issue.    

        *       Some fixes were in their Erlang code to reduce BEAM’s contention issues.    

        *       Some patched to BEAM.    

        *       Partitioning workload so work didn’t have to cross processors a lot.    

        *       Time-of-day lock. Every time a message is delivered from a port it looks to update the time-of-day which is a single lock across all schedulers which means all CPUs are hitting one lock.    

        *       Optimized use of timer wheels. Removed bif timer    

        *       Check IO time table grows arithmetically. Created VM thrashing has the hash table would be reallocated at various points. Improved to use geometric allocation of the table.    

        *       Added write file that takes a port that you already have open to reduce port contention.    

        *       Mseg allocation is single point of contention across all allocators. Make per scheduler.    

        *       Lots of port transactions when accepting a connection. Set option to reduce expensive port interactions.    

        *       When message queue backlogs became large garbage collection would destabilize the system. So pause GC until the queues shrunk.    

    *       Avoiding some common things that come at a price.    

        *       Backported a TSE time counter from FreeBSD 9 to 8\. It’s a cheaper to read timer. Fast to get time of day, less expensive than going to a chip.    

        *       Backported igp network driver from FreeBSD 9 because having issue with multiple queue on NICs locking up.    

        *       Increase number of files and sockets.    

        *       Pmcstat showed a lot of time was spent looking up PCBs in the network stack. So bumped up the size of the hash table to make lookups faster.    

    *       BEAM Patches    

        *       Previously mentioned instrumentation patches. Instrument scheduler to get utilization information, statistics for message queues, number of sleeps, send rates, message counts, etc. Can be done in Erlang code with procinfo, but with a million connections it’s very slow.    

        *       Stats collection is very efficient to gather so they can be run in production.    

        *       Stats kept at 3 different decay intervals: 1, 10, 100 second intervals. Allows seeing issues over time.    

        *       Make lock counting work for larger async thread counts.    

        *       Added debug options to debug lock counters.    

    *       Tuning    

        *       Set the scheduler wake up threshold to low because schedulers would go to sleep and would never wake up.    

        *       Prefer mseg allocators over malloc.    

        *       Have an allocator per instance per scheduler.    

        *       Configure carrier sizes start out big and get bigger. Causes FreeBSD to use super pages. Reduced TLB thrash rate and improves throughput for the same CPU.    

        *       Run BEAM at real-time priority so that other things like cron jobs don’t interrupt schedule. Prevents glitches that would cause backlogs of important user traffic.    

        *       Patch to dial down spin counts so the scheduler wouldn’t spin.    

    *       Mnesia    

        *       Prefer os:timestamp to erlang:now.    

        *       Using no transactions, but with remote replication ran into a backlog. Parallelized replication for each table to increase throughput.    

    *       There are actually lots more changes that were made.    

##     Lessons    

*       **Optimization is dark grungy work suitable only for trolls and engineers**        . When Rick is going through all the changes that he made to get to 2 million connections a server it was mind numbing. Notice the immense amount of work that went into writing tools, running tests, backporting code, adding gobs of instrumentation to nearly every level of the stack, tuning the system, looking at traces, mucking with very low level details and just trying to understand everything. That’s what it takes to remove the bottlenecks in order to increase performance and scalability to extreme levels.    

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

*       **Purposely overprovision hardware.**         This ensures that users have uninterrupted service during their festivities and employees are able to enjoy the holidays without spending the whole time fixing overload issues.    

*   **Growth stalls when you charge money****.** Growth was super fast when WhatsApp was free, 10,000 downloads a day in the early days. Then when switching over to paid that declined to 1,000 a day. At the end of the year, after adding picture messaging, they settled on charging a one-time download fee, later modified to an annual payment.

*   **Inspiration comes from the strangest places****.** Experience with forgetting the username and passwords on Skype accounts drove the passion for making the app "just work."

##     Related Articles    

*   [On Hacker News](https://news.ycombinator.com/item?id=7306066)

*   [    Keynote: Benedict Evans - InContext 2014    ](https://www.youtube.com/watch?v=VnhbvS0MBXE)     (related     [    slides    ](http://ben-evans.com/presentations/2013/11/5/mobile-is-eating-the-world-autumn-2013-edition)    )    

*   [    Whatsapp and $19bn    ](http://ben-evans.com/benedictevans/2014/2/19/whatsapp-and-19bn)     by Benedict Evans    

*   [    WhatsApp's blog: The telling diary of a 16 billion dollar startup    ](http://www.slideshare.net/socialmarketingfella/the-diary-of-a-16-billion-startup-31457350)     - nice timeline of events from Andre Bourque.    

*       Rick’s Changes to     [    Erlang on GitHub    ](https://github.com/reedr)

*   [    WhatsApp Blog    ](http://blog.whatsapp.com/)    :    

    *   [    ONE MILLION!    ](http://blog.whatsapp.com/index.php/2011/09/one-million/)     (    [    Hacker News    ](https://news.ycombinator.com/item?id=3028547)    )    

    *   [    1 million is so 2011    ](http://blog.whatsapp.com/index.php/2012/01/1-million-is-so-2011/)

    *   [    400 Million Stories    ](http://blog.whatsapp.com/index.php/2013/12/400-million-stories/)

*   [    WhatsApp: The inside story    ](http://www.wired.co.uk/news/archive/2014-02/19/whatsapp-exclusive)

*   [    The Open Source projects used at WhatsApp    ](http://www.whatsapp.com/opensource/)

*   [    Whatsapp, Facebook, Erlang and realtime messaging: It all started with ejabberd    ](http://blog.process-one.net/whatsapp-facebook-erlang-and-realtime-messaging-it-all-started-with-ejabberd/)

*       Quora:     [    How does WhatsApp Work?    ](http://www.quora.com/How-does-WhatsApp-work-1)    ,     [    How does WhatsApp work out of mobile, network?    ](http://www.quora.com/How-does-WhatsApp-work-out-of-mobile-network)    ,     [    How did WhatsApp grow so big?    ](http://www.quora.com/WhatsApp-Messenger/How-did-WhatsApp-grow-so-big)

*   [    WhatsApp is broken, really broken    ](http://fileperms.org/whatsapp-is-broken-really-broken/)     - early security problems    

*   [    WhatsApp CEO Jan Koum Hates Advertising and the Tech Rumor Mill (Full Dive Video)    ](http://allthingsd.com/20130510/whatsapp-ceo-jan-koum-hates-advertising-and-the-tech-rumor-mill-full-dive-video/)

*   [    Singapore is progressively doing business over WhatsApp. Are You?    ](http://www.happymarketer.com/singapore-is-progressively-doing-business-over-whatsapp-are-you)

*   [    Four Numbers That Explain Why Facebook Acquired WhatsApp    ](http://sequoiacapital.tumblr.com/post/77211282835/four-numbers-that-explain-why-facebook-acquired)

*   [    Announcement from Mark Zuckerberg    ](https://www.facebook.com/zuck/posts/10101272463589561?stream_ref=10)

*   [    A Million-user Comet Application with Mochiweb, Part 3    ](http://www.metabrew.com/article/a-million-user-comet-application-with-mochiweb-part-3)

*   [    Inside Erlang, The Rare Programming Language Behind WhatsApp's Success    ](http://www.fastcolabs.com/3026758/inside-erlang-the-rare-programming-language-behind-whatsapps-success)

*       [WhatsApp Is Actually Worth More Than $19B, Says Facebook’s Zuckerberg, And It Was Internet.org That Sealed The Deal](http://techcrunch.com/2014/02/24/whatsapp-is-actually-worth-more-than-19b-says-facebooks-zuckerberg/)    

*   [    Facebook buys Whatsapp for $19 billion: Value and Pricing Perspectives    ](http://aswathdamodaran.blogspot.in/2014/02/facebook-buys-whatsapp-for-19-billion.html)

*   [    Facebook's $19 Billion Craving, Explained By Mark Zuckerberg    ](http://www.forbes.com/sites/georgeanders/2014/02/19/facebook-justifies-19-billion-by-awe-at-whatsapp-growth/)

*   [    IMHO: Lessons learned from WhatsApp    ](http://blog.mindcrime-ilab.de/2012/12/16/imho-lessons-learned-from-whatsapp/)

*   [    You May Not Use WhatsApp, But the Rest of the World Sure Does    ](http://www.wired.com/wiredenterprise/2014/02/whatsapp-rules-rest-world/)

*   [    The WhatsApp Story Challenges Some Of The Valley’s Conventional Wisdom    ](http://techcrunch.com/2014/02/23/the-whatsapp-story-challenges-the-valleys-conventional-wisdom/)

*   [    What WhatsApp Did Right, According to Jan Koum (Video)    ](http://recode.net/2014/02/20/what-whatsapp-did-right-according-to-jan-koum-video/)

*   [    Why did Facebook buy WhatsApp?    ](http://kottke.org/14/02/why-did-facebook-buy-whatsapp)

*   [    Can Someone Explain WhatsApp's Valuation To Me?    ](http://www.linkedin.com/today/post/article/20140220134541-79695780-can-someone-explain-whatsapp-s-valuation-to-me)

*   [    Google’s Unusual Offer to WhatsApp    ](https://www.theinformation.com/Google-s-Unusual-Offer-to-WhatsApp)    .    

    