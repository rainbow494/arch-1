## [Mailinator Architecture](/blog/2008/1/24/mailinator-architecture.html)

    

    

**Update:** A fun exploration of applied searching in [How to search for the word "pen1s" in 185 emails every second](http://mailinator.blogspot.com/2008/01/how-to-search-for-word-pen1s-in-185.html). When indexOf doesn't cut it you just trie harder.  

Has a drunken friend ever inspired you to create a first of its kind internet service that is loved by millions, deemed subversive by thousands, all while handling over 1.2 billion emails a year on one [rickity](http://www.flickr.com/photos/arcticstylings/814612466) old server? That's how Paul Tyma came to build Mailinator.  

Mailinator is a free no-setup web service for thwarting evil spammers by creating throw-away registration email addresses. If you don't give web sites you real email address they can't spam you. They spam Mailinator instead :-)  

I love design with a point-of-view and Mailinator has a big giant harry one: performance first, second, and last. Why? Because Mailinator is free and that allows Paul to showcase his different perspective on design. While competitors buy big Iron to handle load, Paul uses a big idea instead: pick the right problem and create a design to fit the problem. No more. No less. The result is a perfect system architecture sonnet, beauty within the constraints of form.  

How does Mailinator carry out its work as a spam busting super hero?

Site: http://mailinator.com/

## Information Sources

*   [The Architecture of Mailinator](http://mailinator.blogspot.com/2007/01/architecture-of-mailinator.html)*   [Mailinator's 2006 Stats](http://mailinator.blogspot.com/2007/02/mailinators-2006-stats.html)  

    ## The Platform

    *   Linux*   Tomcat*   Java  

    ## The Stats

    *   Will process an estimated 1.29 BILLION emails for 2007\. 450.74 million in 2006\. 280.68 million in 2005.*   Peak rate of 6.5 million emails/day or 4513/min or 75/sec.*   Mailinator runs on a very modest machine with an AMD 2Ghz Athlon processor, 1GB of RAM (much less is used), and a low-performance 80G IDE hard drive. And the machine is not very busy at all.*   Mailinator runs for months unattended and very few emails are lost, even under constant spam attacks and high peak loads.  

    ## The Architecture

    *   Having a free system means the system doesn't have to be perfect. So the design goals are:  
    - Design a system that values survival above all else, even users. Survival is key because Mailinator must fight off attacks on a daily basis.  
    - Provide 99.99% uptime and accuracy for users. Higher uptime goals would be impractical and costly. And since the service is free this is just part of rules of the game for users.  
    - Support the following service model: user signs-up for something, goes to Mailinator, clicks on the subscription link, and forgets about it. This means email doesn't have to be stored persistently on disk. Email can reside in RAM because it is temporary (3-4 hours). If you want a real mailbox then use another service.*   The original flow of email handling was:  
    - Sendmail received email in a single on-disk mailbox.  
    - The Java based Mailinator grabbed emails using IMAP and/or POP (it changed over time) and deleted them.  
    - The system then loaded all emails into memory and let them sit there.  
    - The oldest email was pushed out once the 20,000 in memory limit was reached.*   The original architecture worked well:  
    - It was stable and stayed up for months at a time.  
    - It used almost all the 1GB of RAM.  
    - Problems started when the incoming email rate started surpassing 800,000 a day. The system broke down because of disk contention between Mailinator and the email subsystem.*   The New Architecture:  
    - The idea was to remove the path through the disk which was accomplished with a complete system rewrite.  
    - The web application, the email server, and all email storage run in one JVM.  
    - Sendmail was replaced with a custom built SMTP server. Because of the nature of Mailinator a full SMTP server was not necessary. Mailinator does not need to send email. And it's primary duty is to accept or reject email as fast as possible. This is the downside of layering. Layering is very often given as a key strategy in scaling, but it can kill performance because crucial decisions are best handled at the highest levels of the stack. So work flows through the system only to be dumped at the lower layers when many of the RAM and cycle stealing operations have already been accomplished. So the decision to go with a custome SMTP server is an interesting and brave decision. Most people at this point would just add more hardware. And they wouldn't be wrong, but it's interesting to see this path taken as well. Maybe with more [DOM and AOP](http://radio.weblogs.com/0103955/categories/stupidHumanProgramming/2007/06/20.html#a244) like architectures we can flatten the stack and get better performance when needed.  
    - Now Mailinator receives an email directly, parses it, and stores it into memory. The disk is bypassed completely and the disk remains fairly idle.  
    - Emails are written to disk when the system is coming down so they can be reloaded on startup.  
    - Logging was shut-off to remove the risk of subpoenaes. When logging was performed log data was written in batches so several thousand logs lines would be written in one disk write. This minimized at disk contention at the risk of losing helpful diagnostic information.  
    - The system uses under 300 threads. More aren't needed.  
    - On arrival each email passes through a filter system and is stored in RAM if all filters are passed.  
    - Every inbox is limited to only 10 emails so popular inboxes, like joe@mailinator.com, can't blow the system.  
    - No incoming email can be over 100k and all attachments are immediately discarded. This saves on RAM.*   Emails are compressed in RAM:  
    - Since 99% of emails are never looked at, compressed email saves RAM. They are only  
    ever decompressed when someone looks at them.  
    - Mailinator can store about 80,000 emails in RAM, using under 300MB of RAM compared to the 20,000 emails which were stored in 1GB RAM in the original design.  
    - With this pool the average email lifespan is about 3-4 hours.  
    - It's likely 200,000 emails could fit in memory, but there hasn't been a real need.  
    - This is one of the design details I love because it's based on real application usage patterns. RAM is precious and CPU is not, so use compression to save RAM at the expense of CPU, knowing you won't have to take the CPU hit twice, most of the time.*   Mailinator does not guarantee anonymity and privacy:  
    - There is no privacy. Anyone can read any inbox at anytime.  
    - Relaxing these constrains, while shocking, makes the design much simpler.  
    - For the user it is simple because there is no sign up needed. When a web site asks you for an email address you can just enter an mailinator address. You don't need to create a separate account. Typing in the email address effectively creates the mailinator account. Simple.  
    - In practice users still get a high level of privacy.*   Goal of survivability leads to aggressive SPAM filtering.  
    - Mailinator doesn't have anything against SPAM, but because it gets so much SPAM, it must be filtered out when it threatens the up time of the system.  
    - Which leads to this rule: If you do anything (spammer or not) that starts affecting the system - your emails will be refused and you may be locked out.*   To be accepted an email must pass the following filter chain:  
    - Bounce: all bounced emails are dropped.  
    - IP: too much email from a single IP are dropped  
    - Subject: too much email on the same subject is dropped  
    - Potty: subjects containing words that indicate hate or crimes or just downright nastiness are dropped.*   Surviving Email Floods from a Single IP Adress  
    - An AgingHashmap is used to filter out spammers from a particular IP address. When an email arrives on a IP address the IP is put in the map and a counter is increased for all subsequent emails.  
    - After a certain period of time with no emails the counter is cleared.  
    - When a sender reaches a threshold email count the sender is blocked. This prevents a sender from flooding the system.  
    - Many systems use this sort of logic to protect all sorts of resources, like comments. You can use memcached for the same purpose in a distributed system.*   Protecting Against Zombie Attacks:  
    - Spam can be sent from a large coordinates sets of different IP addresses, called zombie networks. The same message is sent from thousands of different IP addresses so the techniques for stopping email from a single IP address are not sufficient.  
    - This filtering is a little more complex than IP blocking because you have to parse enough  
    of the email to get the subject line and matching subject strings is a little more resource intensive.  
    - When something like 20 emails with the same subject within 2 minutes, all emails with that subject are then banned for 1 hour.  
    - Interestingly, subjects are not banned forever because that would mean Mailinator would have to track subjects forever and the system design is inherently transient. This is pretty clever I think. At the cost of a few "bad" emails getting through the system is much simpler because no persistent list must be managed and that list surely would become a bottleneck. A system with more stringent SPAM filtering goals would have to create a much more complex and less robust architecture.  
    - Nealy 9% of emails are blocked with this filter.  
    - From my reading Mailinator filters only on IP and subject, so it doesn't have to read the body of the email body to accept or reject the email. This minimizes resource usage when most email will be rejected.*   To lessen the danger from DOS attacks:  
    - All connections that are silent for a specific period of time are droped.  
    - Mailinator sends replies to email senders very slowly, like 10 or 20 or 30 seconds, even for a very small amount of data. This slows down spammers who are trying to send out spam as fast as possible and may make them rethink sending email again to that address. The wait period is reduced during busy periods so email isn't dropped.  

    ## Lessons Learned

    *   **Perfection is a trap**. How many systems are made much more complicated by the drive to be 100% everything. If you've been in those meetings you know what they are like. Oh, we can't do it this way or that way because there's .01% chance of something going wrong. Instead ask: how imperfect can you be and be good enough?  

    *   **What you throw out is as important as what you keep in**. We have many preconceptions of how to design systems. We make take for granted that you need to scale-out, you need to have email accessible days later, and that you must provide  
    private accounts for everyone. But you really need these things? What can you toss?  

    *   **Know the purpose of your system and design accordingly**. Being everything to everyone means you are nothing to nobody. Keeping emails for a short period of time, allowing some SPAM to get through, and accepting less than 100% uptime create a strong vision for the system that help drive the design in all areas. You would only build your own SMTP server if you had a very strong idea of what your system was about and what you needed. I know this would have never occurred to me as an idea. I would have added more hardware.  

    *   **Fail fast for the common case before committing resources**. A high percentage of email is rejected so it makes sense to reject it as early as possible in the stack to minimize resources to accomplish the task. Figure out how to short circuit frequently failed items as fast as possible. This is important and often over looked scaling strategy.  

    *   **Efficiency often means build it yourself**. Off the shelf tools tend to do the whole job. If you only need part of the job done you may be able to write a custom component that runs much faster.  

    *   **Adaptively forget**. A little failure is OK. All the blocked IP addresses don't need to be remembered forever. Let the block decisions build up from local data rather than global state. This is powerfully simple and robust architecture.  

    *   **Java doesn't have to be slow**. Enough said.  

    *   **Avoid the disk**. Many applications need to hit the disk, but the disk is always a bottleneck. Can you design around the disk using other creative strategies?  

    *   **Constrain resource usage**. Put in constraints, like inbox size, that will keep your system for spiking uncontrollably. Unconstrained resource usage must be avoided with limited resources.  

    *   **Compress data**. Compression can be a major win  
    when trying to conserve RAM. I've seen memory usage drop by more than half when using compression with very little overhead. If you are communicating locally, just have the client encode the data and keep it encoded. Build APIs to access the data without have to decode the full message.  

    *   **Use fixed size resource pools to handle load**. Many applications don't control resource usage, like memory, and they crash when too much is used. To create a really robust system fix your resources and drop work when those resources are full. You can age resources, give priority access, give fair access, or use any other logic to arbitrate resource access, but because the resource will be limited, you will stay up under load.  

    *   **If you don't keep data it can't be subpoenaed**. Because Mailinator doesn't store email or logs on disk noting can be subpoenaed.  

    *   **Use what you know**. We've seen this lesson a few times. Paul knew Java better than anything else, so he used it, made it work, and he got the job got done.  

    *   **Find your own Mailinators**. Sure, Mailinator is a small system. In a large system it would just be a small feature, but your system is composed of many Mailinator sized projects. What if you developed some of  
    those like Mailinator?  

    *   **KISS exists, though it's rare**. Keeping it simple is always talked about, but rarely are we shown real examples. It's mostly just your way is complex and my way is simple because it's my way. Mailinator is a good example of simple design.  

    *   **Robustness is a function of architecture**. To create a design that efficiently uses memory and survives massive spam attacks required an architectural approach that looked at the entire stack.  

    ## Related Articles

    *   [PlentyOfFish](http://highscalability.com/plentyoffish-architecture) champions straight forward bare bones simplicity.*   [Varnish](http://highscalability.com/links/goto/117/) smartly uses OS features to find incredible performance.*   [ThemBid](http://highscalability.com/thembid-architecture) gracefully pieces together open source components.    