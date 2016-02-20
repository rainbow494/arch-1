## [GoogleTalk Architecture](/blog/2007/7/23/googletalk-architecture.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, July 23, 2007 at 8:47AM</span></div>

<div class="body">

Google Talk is Google's instant communications service. Interestingly the IM messages aren't the major architectural challenge, handling user presence indications dominate the design. They also have the challenge of handling small low latency messages and integrating with many other systems. How do they do it?

Site: http://www.google.com/talk

## Information Sources

*   [GoogleTalk Architecture](http://video.google.com/videoplay?docid=6202268628085731280)  

    ## Platform

    *   Linux*   Java*   Google Stack*   Shard  

    ## What's Inside?

    ### The Stats

    *   Support presence and messages for millions of users.*   Handles billions of packets per day in under 100ms.*   IM is different than many other applications because the requests are small packets.*   Routing and application logic are applied per packet for sender and receiver.*   Messages must be delivered in-order.*   Architecture extends to new clients and Google services.  

    ## Lessons Learned

    *   Measure the right thing.  
    - People ask about how many IMs do you deliver or how many active users. Turns out not to be the right engineering question.  
    - Hard part of IM is how to show correct present to all connected users because growth is non-linear: ConnectedUsers * BuddyListSize * OnlineStateChanges  
    - A linear user grown can mean a very non-linear server growth which requires serving many billions of presence packets per day.  
    - Have a large number friends and presence explodes. The number IMs not that  
    big of deal.  

    *   Real Life Load Tests  
    - Lab tests are good, but don't tell you enough.  
    - Did a backend launch before the real product launch.  
    - Simulate presence requests and going on-line and off-line for weeks  
    and months, even if real data is not returned. It works out many of the  
    kinks in network, failover, etc.  

    *   Dynamic Resharding  
    - Divide user data or load across shards.  
    - Google Talk backend servers handle traffic for a subset of users.  
    - Make it easy to change the number of shards with zero downtime.  
    - Don't shard across data centers. Try and keep users local.  
    - Servers can bring down servers and backups take over. Then you can bring up new servers and data migrated automatically and clients auto detect and go to new servers.  

    *   Add Abstractions to Hide System Complexity  
    - Different systems should have little knowledge of each other, especially when separate groups are working together.  
    - Gmail and Orkut don't know about sharding, load-balancing, or fail-over, data center architecture, or number of servers. Can change at anytime without cascading changes throughout the system.  
    - Abstract these complexities into a set of gateways that are discovered at runtime.  
    - RPC infrastructure should handle rerouting.  

    *   Understand Semantics of Lower Level Libraries  
    - Everything is abstracted, but you must still have enough knowledge of how they work to architect your system.  
    - Does your RPC create TCP connections to all or some of your servers? Very different implications.  
    - Does the library performance health checking? This is architectural implications as you can have separate system failing independently.  
    - Which kernel operation should you use? IM requires a lot connections but few have any activity. Use epoll vs poll/select.  

    *   Protect Again Operation Problems  
    - Smooth out all spoke in server activity graphs.  
    - What happens when servers restart with an empty cache?  
    - What happens if traffic shifts to a new data center?  
    - Limit cascading problems. Back of from busy servers. Don't accept work when sick.  
    - Isolate in emergencies. Don't infect others with your problems.  
    - Have intelligent retry logic policies abstracted away. Don't sit in hard 1msec retry loops, for example.  

    *   Any Scalable System is a Distributed System  
    - Add fault tolerance to every component of the system. Everything fails.  
    - Add ability to profile live servers without impacting server. Allows continual improvement.  
    - Collect metrics from server for monitoring. Log everything about your system so you see patterns in cause and effects.  
    - Log end-to-end so you can reconstruct an entire operation from beginning to end across all machines.  

    *   Software Development Strategies  
    - Make sure binaries are both backward and forward compatible so you can have old clients work with new code.  
    - Build an experimentation framework to try new features.  
    - Give engineers access to product machines. Gives end-to-end ownership. This is very different than many companies who have completely separate OP teams in their data centers. Often developers can't touch production machines.</div>