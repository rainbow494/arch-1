## [WAN Accelerate Your Way to Lightening Fast Transfers Between Data Centers](/blog/2007/10/10/wan-accelerate-your-way-to-lightening-fast-transfers-between.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, October 10, 2007 at 3:18PM</span></div>

<div class="body">How do you keep in sync a crescendo of data between data centers over a slow WAN? That's the question [Alberto](http://highscalability.com/sync-data-all-servers) posted a few weeks ago. Normally I'm not into all boy bands, but I was frustrated there wasn't a really good answer for his problem. It occurred to me later a WAN accelerator might help turn his slow WAN link into more of a LAN, so the overhead of copying files across the WAN wouldn't be so limiting. Many might not consider a WAN accelerator in this situation, but since my friend Damon Ennis works at the WAN accelerator vendor [Silver Peak](http://www.silver-peak.com), I thought I would ask him if their product would help. Not surprisingly his answer is yes! Potentially a lot, depending on the nature of your data. Here's a no BS overview of their product:  

*   What is it?  
    - Scalable WAN Accelerator from Silver Peak (http://www.silver-peak.com)  

    *   What does it do?  
    - You can send 5x-100x times more data across your expensive, low-bandwidth WAN link.  

    *   Why should you care?  
    - Your data centers become more like co-located real-time peers.  
    - You can sync a lot more media and other large files across data centers. 50x improvement in data replication performance over a WAN.  
    - You may be able to operate on remote database more like a local database. 5x-20x improvement is SQL data manipulation and unique query performance.  
    - A 2 hour database backup would take 4 minutes. 10x-30x improvement in transferring large data sets over SQL. A good disaster planning feature.  

    *   How does it work?  
    - You buy an accelerator appliance for both sides of you link. All your WAN traffic flows through these boxes.  
    - The appliances then use various techniques to effectively decrease latency and increase bandwidth across the link:  
    -- Traffic reduction. Accelerators look for patterns in data across a link, caching the data on either side of the link, and then not sending the data when similar patterns are seen again. This can lead to a 90% reduction in traffic.  
    -- Compression. Data are compressed across the link. compression ratios from 0 to 2-5x are seen, depending on the content type.  
    -- TCP Manipulation. The TCP/IP protocol is gamed to yield better performance. For example, a proxy on both sides is used to get a bigger window size.  
    -- Application Manipulation. Various application protocols, like CIFS, NFS, and Outlook, can be gamed to improve performance.  

    *   How much does it cost?  
    - $10k to $130k per box. $10k for the 2Mbps appliance and $130k for the 500Mbps.  
    - They are the scale leaders and are specifically good at "high-end" (> 50Mbps) replication.  

    *   Who uses it?  
    - Fidelity Bank, Ernst & Young, Panasonic.  

    *   Is it for real?  
    - Yes. It works and is installed and running in many data centers.  

    *   How do you get it?  
    - Contact sales at http://www.silver-peak.com/Contact/contact.asp.  

    *   Where do you go for more information?  
    - White paper Directory - http://www.silver-peak.com/InfoCenter/index.htm#whitepapers  
    - Understanding WAN Acceleration Techniques - http://www.silver-peak.com/assets/download/pdf/technologydescriptions.pdf  

    *   Is there anything else interesting you should know?  
    - The appliance performs encryption and compression so you don't need perform those functions on your own CPUs.  
    - The appliances fail to wire so if a box fails traffic passes unaccelerated. If you can't live with that you need to buy 2 boxes per end of the link (4 boxes total).  

    *   How much will you benefit?  
    - The more duplication in your data the better job they can do. There's tons of duplicated data in a database feed , for example, so they can really help supercharge database performance.  
    - Latency/time improvements depend on the link. The higher the latency the link has the less bandwidth you can use. For example, a 100ms link is limited to 5Mbps throughput per flow due to the TCP window size (64KB/100ms ~ 5Mbps). They can take this to several hundred Mbps per flow.  
    - Image files are often pre-compressed. As compression removes duplicate information they can't be as efficient at the de-duplication as in other scenarios, though they can still improve throughput.  

    An interesting side-effect of speeding up the WAN link is that it often reveals bottlenecks in other parts of the system. A slow WAN might be hiding:  
    *   Underpowered servers. Servers that could process a trickle of data may be overwhelmed by a flood of data.  
    *   Slow applications. Apps that could pump data at slow WAN speeds may not be able drive a faster WAN. You may need to take a look at your software architecture or storage network.  
    *   Underpowered server links. Accelerate a 2mbps link to a 20mbps link and your network infrastructure on the data center side may not be able to handle the truth.  

    Obviously the cost of the solution means its targeted more for moderate sized companies or a service provider offering their customers a quality upsell. But if you are stuck wondering how the heck you are going to squeeze more bits between your data centers, it may be just the magic bullet you need.</div>