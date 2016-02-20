## [Netflix: Use Less Chatty Protocols in the Cloud - Plus 26 Fixes](/blog/2010/12/20/netflix-use-less-chatty-protocols-in-the-cloud-plus-26-fixes.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, December 20, 2010 at 9:08AM</span></div>

<div class="body">

![](http://farm6.static.flickr.com/5081/5270646354_0713fc28e0_m.jpg)

In [5 Lessons We’ve Learned Using AWS](http://techblog.netflix.com/2010/12/5-lessons-weve-learned-using-aws.html#), Netflix's John Ciancutti says one of the big lessons they've learned is to create less [chatty protocols](http://kb.pert.geant.net/PERTKB/ChattyProtocols):

> <div id="_mcePaste">In the Netflix data centers, we have a high capacity, super fast, highly reliable network. This has afforded us the luxury of designing around chatty APIs to remote systems. AWS networking has more variable latency. We’ve had to be much more structured about “over the wire” interactions, even as we’ve transitioned to a more highly distributed architecture.</div>

There's not a lot of advice out there on how to create protocols. Combine that with a rush to the cloud and you have a perfect storm for chatty applications crushing application performance. Netflix is far from the first to be surprised by the less than stellar networks inside AWS. 

A chatty protocol is one where a client makes a series of requests to a server and the client must wait on each reply before sending the next request. On a LAN this can work great. LAN's are typically fast, wide, and drop few packets.

Move that same application to a different network, one where round trip times can easily be an order of magnitude or larger because either the network is slow, lossy or poorly designed, and if a protocol takes many requests to complete a transaction, then it will make a dramatic difference in performance.

My WAN acceleration friends says Microsoft's Common Internet File System (CIFS) is infamous for being chatty. Transferring a 30MB file could [tally something like 300msecs](http://searchenterprisewan.techtarget.com/definition/chatty-protocol) of latency on a LAN. On a WAN that could stretch to 7 minutes. Very unexpected results. What is key here is how the quality characteristics of the pipe interacts with the protocol design.

OK, chatty protocols are bad. What can you do about it? The overall goal is to reduce <span style="color: #000000;"><span style="line-height: 15px;">the number of times a client has to wait for a response and reduce the number of packets sent. </span></span>

1.  <span style="color: #000000; line-height: 15px;">eduPERT has a [great example](http://kb.pert.geant.net/PERTKB/ChattyProtocols) of how sending mail using SMTP was reduced from 9 to 4 rountrips through pipelining.</span>
2.  <span style="color: #000000; line-height: 15px;">Great paper on improving [X Window System Network Performance](http://keithp.com/~keithp/talks/usenix2003/html/net.html) using _protocol-level performance analysis combined with passive network monitoring._</span> 

<span style="color: #000000;"><span style="line-height: 15px;">Some possible strategies are:</span></span> 

1.  **Identify and remove unnecessary roundtrips.** This may be obvious, but take a look at your protocols and see if any of the rountrips can be combined or eliminated. There may be a lot of low hanging fruit here with just a little thought. 
2.  **Think services, not objects**. One of the reasons protocols become so chatty is programmers are thinking in terms of [fine-grained](http://www.ibm.com/developerworks/webservices/library/ws-soa-granularity/) distributed objects rather coarser-grained services. This leads to chatty protocols because there is a lot of back and forth for each method call as methods tend to be pretty low in granularity. What you want is a service interface that does as large an amount of work for each request as possible. To transform an image, for example, you want to make one call and pass in all the transforms so they can be done in that one call rather than making a separate remote method invocation for each transform. This is a classic and common mistake when first using CORBA, for example. 
3.  **Aggregate data to reduce the number roundtrips**.
    1.  **Use token buckets.**  [Token buckets](http://en.wikipedia.org/wiki/Token_bucket) are atechnique for allowing _a certain amount of burstiness while imposing a limit on the average data transmission rate_.
    2.  **Batch/pipeline requests.** Send more than one operation in a request and receive a single response.
    3.  **Batch via integration timers**. Queue up requests for X milliseconds so they can be sent in a batch rather than one by one.
    4.  **Use fat pings**. [Fat pings](http://techcrunch.com/2009/09/09/rsscloud-vs-pubsubhubbub-why-the-fat-pings-win/) reduce the amount of data that needs to be sent, especially compared to constant polling. 
4.  **Data Reduction Techniques. **Keep data as small as possible so more work can fit inside a single TCP segment.
    1.  **Data compression**. 
    2.  **Use a binary protocol**. Please, no flamewars, but a custom binary protocol will generally be more space efficient than text + compression.
    3.  **Do not transmit redundant information**.  This is more effective than compressing the information.
    4.  **Use high level descriptions rather than low level descriptions**. Can you create a DSL or other way of describing your data and what you want done using a more compact code?
    5.  **Allow requests to reuse recently transmitted information**. If data has already be sent can later day refer back to it so it doesn't have to be transmitted again?
5.  **<span style="font-weight: normal;">**Keep a local cache**. Simply avoid going over the network by caching data locally. This of course pushes the problem out to become one of maintaining consistency.</span>**
6.  **Use Asynchronous APIs**. Don't pause an application waiting for replies. This doesn't fix the protocol necessarily, but may lessen the effect.
7.  **Use ackless protocols**. Don't even worry about replies. Fire off requests to a message queue. When a worker processes a message it can forward the response on to the next stage of the processing pipeline.
8.  **Lazy and bulk initialization**. When applications start they sometimes need to go through an elaborate initial configuration phase. Try to minimize the amount of configuration an application needs. When configuration is needed try to do it in bulk so that a program will get all it's configuration attributes in one big structure rather than making a lot of individual calls. Another approach is to use lazy initialization so you just get configuration as needed rather than all at once. Maybe you won't need it?
9.  **Move functionality to the client and out of the server**. Is it possible for the client to do more work so nothing has to go back to the server? The X Window System saved a lot by moving font storage to the client.
10.  **Realize more bandwidth is usually not a fix**. Latency and packet loss are usually the problems so simply adding more bandwidth won't solve the problem, unless of course a lack of bandwidth is really the problem.
11.  **Use rack affinity and availability zones**. Latency and RTT times are increased by going through a long series of routers, switches etc. Take care that data stays within the same availability zone as much as possible. Some databases like Cassandra even let you specify rack affinity to further reduce this overhead.
12.  **Avoid fine grained operations**.  Don't, for example, implement a drag and drop operation over the network that can take 100s of requests to implement. Older versions of Windows would go get the directory on the remote side and for every file it would perform individual file stat operations for each file attribute. What a WAN accelerator does is get all the file data at once, create a local cache, and serve those requests from the local cache. A good transparent work around, but of course the idea is to avoid these fine grained type operations completely.
13.  **Use persistent connections**. Create a connection and keep it nailed up. This avoids the overhead of handshaking and connection establishment. On the web consider using [Comet](http://en.wikipedia.org/wiki/Comet_(programming)). 
14.  **<span style="font-weight: normal;">**<span style="font-weight: normal;">**Get right to business**</span>**</span>**. Pass real data back and forth as soon as possible. Don't begin with a long negotiation phase or other overhead. 
15.  **Use stateful protocols**. Do you need to send the same state all the time? For an application protocol the answer is probably no. For something more general like HTTP statelessness is probably the higher goal.  
16.  **Tune TCP**.  This is one of the typical functions of a [WAN accelerator](http://highscalability.com/blog/2007/10/10/wan-accelerate-your-way-to-lightening-fast-transfers-between.html). TCP was not developed with high latency  in mind, but you can fake it into behaving. On a WAN you could try creating your own MPLS network, but inside a cloud you don't have a lot of options. Hopefully [in the future](http://www.openflowswitch.org/) the L2/L3 network fabrics inside datacenters will improve as will datacenter interconnects.
17.  **Colocate behavior and state**. [Collocating behavior and state](/blog/2010/11/1/hot-trend-move-behavior-to-data-for-a-new-interactive-applic.html) reduces the number of networks hops necessary to carry out operations.
18.  **Use the High Performance Computing (HPC) cluster**. HPC cluster is backed by a high performance network, and if your use case fits, it could be a big win. 
19.  **Measure the effect of changes in a systematic fashion.** Profile your application to see what's really going on. Surprises may abound. When a change is made verify an improvement is actually being made.

<div id="followup10450666" class="journal-entry-follow-up">

<div class="follow-up-caption">**Update** on Friday, February 11, 2011 at 11:26AM by [![Registered Commenter](/universal/images/transparent.png "Registered Commenter")Todd Hoff](/member/highscalability "Registered Commenter") </div>

<div class="follow-up-body">

<div>

Netflix posted a related article, [Redesigning the Netflix API](http://techblog.netflix.com/2011/02/redesigning-netflix-api.html#), where they go into a more details about their process. The idea is to ask why is a device so chatty? Can the number of requests be cut in half? Since this will drive up payload sizes, they will try using [partial response](http://googlecode.blogspot.com/2010/03/making-apis-faster-introducing-partial.html)s as a counter measure. Their goal is to _conceptualize the API as a database_. What does this mean? A partial response is like a SQL select statement where you can specify only the fields you want back. Previously all fields for objects were returned, even if the client didn't need them. So the goal is reduce payload sizes by being more selective about what data is returned. 

</div>

</div>

</div>

</div>