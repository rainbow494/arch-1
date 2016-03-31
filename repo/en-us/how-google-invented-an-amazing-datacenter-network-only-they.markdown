## [How Google Invented an Amazing Datacenter Network Only They Could Create](/blog/2015/8/10/how-google-invented-an-amazing-datacenter-network-only-they.html)

    

    

    ![](https://farm1.staticflickr.com/397/20448196031_800175f1fa.jpg)    

    Google with justly earned pride recently     [    announced    ](http://googlecloudplatform.blogspot.com/2015/06/A-Look-Inside-Googles-Data-Center-Networks.html)    :    

>     Today at the 2015 Open Network Summit, we are revealing for the first time the details of five generations of our in-house network technology. From Firehose, our first in-house datacenter network, ten years ago to our latest-generation Jupiter network, we’ve increased the capacity of a single datacenter network more than 100x. Our current generation — Jupiter fabrics — can deliver more than 1 Petabit/sec of total         bisection bandwidth        . To put this in perspective, such capacity would be enough for 100,000 servers to exchange information at 10Gb/s each, enough to read the entire scanned contents of the Library of Congress in less than 1/10th of a second.    

    Google’s datacenter network is the magic behind what makes much of Google really work. But what is “bisectional bandwidth” and why does it matter? We talked about bisectional bandwidth a while back in     [    Changing Architectures: New Datacenter Networks Will Set Your Code And Data Free    ](http://highscalability.com/blog/2012/9/4/changing-architectures-new-datacenter-networks-will-set-your.html)    . In short, bisectional bandwidth refers to the networks Google servers use to talk to each other.    

    Historically datacenter networks were oriented around talking to users. Let’s say a request for a web page came in from a browser. The request would go to a server and a reply was crafted by talking to just a few other servers, or perhaps even none at all, and the reply would be sent back to the client. This style of network is called a North/South oriented network. Very little internal communication was needed to implement a request.    

    That all changed as website and API services grew richer over time. Now     [    literally thousands    ](http://highscalability.com/blog/2012/6/18/google-on-latency-tolerant-systems-making-a-predictable-whol.html)     of backend requests can be made to create a single web page. Mind blowing. This meant communication shifted from being dominated by talking to users to talking to other machines within a datacenter. So these are called East/West oriented networks.    

    The shift to East/West dominate communication patterns meant a different     [    topology    ](https://en.wikipedia.org/wiki/Network_topology)     was needed for datacenter networks. The old traditional     [    fat tree    ](https://storagemojo.com/2008/08/24/fat-trees-and-skinny-switches/)     network designs were out and something new needed to take its place.    

    Google has been on the forefront of developing new rich service supportive network designs largely because of their guiding vision of seeing     [    The Datacenter as a Computer    ](http://research.google.com/pubs/pub35290.html)    . Once your datacenter is the computer then your network is equivalent to a     [    backplane    ](https://en.wikipedia.org/wiki/Backplane)     on a single computer, so it must be as fast and reliable as possible so remote disk and remote storage can be accessed as if they were local.    

    Google’s efforts revolve around a three pronged plan of attack: use a     [    Clos topology    ](https://en.wikipedia.org/wiki/Clos_network)    , use     [    SDN    ](https://www.youtube.com/watch?v=ED51Ts4o3os)     (Software Defined Networking), and build their own custom gear in their own Googlish way.    

    Until now we’ve had a limited exposure to Google’s network designs. While we don’t exactly have an all access pass,     [    Amin Vahdat    ](http://research.google.com/pubs/AminVahdat.html)    , Google Fellow and Technical Lead for networking at Google, shared a lot of juicy details in a great talk:     [    ONS [Open Networking Summit] 2015: Wednesday Keynote    ](https://www.youtube.com/watch?v=FaAZAII2x0w)    . There’s also a paper:     [    Jupiter Rising: A Decade of Clos Topologies and Centralized Control in Google’s Datacenter Network    ](http://conferences.sigcomm.org/sigcomm/2015/pdf/papers/p183.pdf)    .    

    Why release details earlier than they usually do? Google has some real competition with Amazon and they need to find compelling points of differentiation. Google hopes their datacenter network is one such point.    

    So what makes Google different? The overall message:    

*       The end of Moore’s Law means how programs are built is changing.    

*       Google has figured it out. Google knows how to build great networks and achieve proper datacenter balance.    

*       You can prosper by taking advantage of the innovations and capabilities of Google’s Cloud Platform, the very same platform that powers Google Search.    

*   So climb on board, the network is fine! 

Is that enough? Perhaps it's not a message with mass appeal, but it may find a home with the discriminating buyer. 

    Some key points from the talk for me:    

*       **We don’t know how to build big networks that deliver lots of bandwidth**. Google says their network provides 1 Pb/sec of total bisection bandwidth, but it turns out that’s not nearly enough. To support a datacenter’s worth of large compute servers you’ll need 5 Pb/sec networks. Keep in mind the entire internet today is probably near 200Tb/s.    

*   **It’s more efficient to schedule jobs over huge clusters**. Otherwise you have leftover CPU in one place and leftover memory in another. So if you can build your system correctly, a datacenter scale computer gives you a decided economy of scale.

*   **Google built their datacenter network system using lessons they learned from the server and storage world**: scale out, logically centralize, use commodity components, and never ever manage singlets of anything. Manage all your servers, storage, and networks as a unified whole.

*   **The I/O gap is huge**. Amin says it has to get solved, if it doesn’t then we’ll stop innovating. Storage capacity has increased through disaggregation. The opportunity is to access global datacenter storage as if it were local. This will get harder and harder with flash and NVM. A new tier of flash and NWM will completely change programming models. Note: unfortunately he didn’t expand on this notion, I dearly wished he had. Amin, can we talk?

    What you look for in a good story are characters that act from a core identity. Here we see Google operating from a unique vision that grew organically from their deep experience building scalable software systems. Probably only Google would have had the guts to follow their vision through and build a datacenter network so completely different from accepted wisdom. That takes huge huevos. And it makes for a good story.    

    Here’s my hopelessly inadequate gloss on the talk:    

*       Hundreds of people contributed to the effort over a decade.    

*       Distributed programming has faced the same set of challenges since sockets were introduced into BSD 30 or so years ago. That’s about to change.    

*       With the end of Moore’s Law how we have to think about computing has to change and what distributed computing means is going to change.    

*       A key part of computing and how people like to build their systems is around storage.    

*       We’ve seen tremendous increases in storage capacity, roughly following Moore’s law.    

*       The I/O gap remains. The distance between processors and the underlying data they need to process is continuing to increase.    

*       We can think of disks spread across entire buildings as being available to any server. This is fantastic. At the same time they are looking further and further away given the processing power we have.    

*       Next generation flash at scale in a distributed computing environment remains largely untapped.    

*       Networking is going to be playing a critical role in what computing means going forward.    

*       Networking is at inflection point. What computing means is going to be largely determined by your ability to build great networks.    

*       So datacenter networking a key differentiator.    

*       Google builds building scale computers. Row after row of computes and row after row of storage.    

*       Over the years Google has built software infrastructure to leverage their hardware infrastructure:    

    *       GFS (2002), MapReduce (2004), BigTable (2006), Borg (2006), Pregel (2008), Colossus (2010), FlumeJava (2012), Dremel, Spanner.    

    *       Many of these efforts have defined what it means to do distributed computing today.    

*       You can’t build world class distributed computing infrastructure without world class networking infrastructure. How could you build a system like GFS to harness 100,000 disks without a world class network to put it all together?    

*       Google innovations in networking:    

    *   Google Global Cache (2006): how content is delivered, including videos and static content from points across the world)
    *   Watchtower (2008)
    *       Freedome: campus-level interconnect    
    *   Onix (2010
    *   BwE (2010)
    *   B4: Google’s Software Defined WAN that connects all their datacenters together across the world. It’s bigger than the public facing network and it’s growing faster.
    *       Jupiter (2012): high bandwidth datacenter scale networking    
    *       Andromeda (2014): network virtualization. How do we take our underlying network infrastructure and slice it up for individual virtual machines with the illusion that they have their own high performance network available to them? How do we open up the same kind network virtualization that Google has been using for their services like load balancing, DDoS protection, access control lists, VPNs, routing, etc., and make if available to these isolated networks running on top of raw hardware?    
    *       QUIC    
    *   gRPC: multi-platform RPC used internally at Google, load-balancing, app level flow control, call-cancellation, serialization, open source, HTTP/2\.  A nice basis for building distributed services.

##     Datacenter Networking    

*       Datacenter networking is different than traditional Internet networks.    

    *       Run by a single organization and is preplanned. The Internet has to be discovered to see what it looks like. You actually know what the network looks like in a datacenter. You planned it. You built it. So you can manage it in a central way rather than discover it as you go along. Because it’s under the control of single organization the software you can run can be very different. It doesn’t have to interoperate with 20 years of legacy, so it can be optimized towards what you need it to do.    

    *       What often draws people to networking is the beauty of the problem. Beauty can mean it's a hard problem. Can the problem be defined to be simpler and more scalable and easier?    

*   Google needed a new network. A decade or so ago Google found traditional network architectures, both hardware and software, **could not keep up** with their bandwidth demands and the scale of their distributed computing infrastructure in the datacenter.

*       Can we buy it?    

    *       Google could not buy at any price a datacenter network that would meet Google’s requirements of their distributed systems.    

*       Can we run it?    

    *   Box-centric deployments incurred the cost of high **operational complexity**. Even if Google bought the biggest datacenter network that they could, the model for operating these networks was around individual boxes, with individual command line interfaces.

    *   Google already knew how to handle 10s of thousands of servers as if they were a single computer and hundreds of thousands of disks as if they were a single storage system. The idea of having to manage a 1000 switches as if they were a thousand switches **didn’t make a lot of sense and seemed unnecessarily hard**.

*       We’ll build it.    

    *       Inspired by what was learned in the server and storage world:         **scale out**        .    

    *   Rather than figure out how to buy the next biggest network or the next biggest router, be able to scale out the switching and routing infrastructure with **additional commodity elements**, just as was done with servers and storage.

    *   Why not when the need arose **plug in another network element** to give more ports and more bandwidth?

*       Three key principles that allowed Google to build scale out datacenter networks:    

    *       **Clos topologies**        . The idea is leverage small commodity switches to build a non-blocking very large switch that can scale more or less infinitely.    

    *       **Merchant silicon**        . Since Google wasn’t building a wide area Internet infrastructure they did not need massive routing tables or deep buffering. This mean commodity merchant silicon could do the job of building Clos based topologies in the datacenter.    

    *       **Centralized control**        . Software agents knew what the network should look like, setup routing, and react to exceptions from the underlying plan. It’s much easier to manage a network when you know its shape compared to when you are constantly trying to discover what it should look like. If you want to change the network then you tell the centralized control software to change the network.    

*       Approaches to the datacenter network also applied to the campus network, connecting up building together and to the wide area network. B4 and Andromeda were inspired by and informed by the work in datacenter networking where they had been practicing building their own hardware and centralized control infrastructure for many years.    

*       Over 6 years datacenter bandwidth growth has increased 50x.    

    *       The amount of bandwidth that needs to be delivered to Google’s servers is outpacing Moore’s Law.    

    *       This means to keep up with bandwidth needs Google has to be able to scale out, it’s not possible to continually rip out old equipment and put in new networking.    

*       Scale drives architecture.    

    *   A typical network today (not necessarily Google) may have 10K+ switches, 250K+ links, 10M+ routing rules. **How you deal with networks at this scale is fundamentally different than smaller scale networks**.

*       Why build networks at the scale of entire datacenters?    

    *       Some of the largest datacenters have 10s and 10s of megawatts of compute infrastructure.    

    *       There is substantial         **resource stranding**         if you can not schedule at scale. Imagine a number of different jobs that need to run across a shared infrastructure. If jobs must be scheduled within the boundary of a single cluster and you have, say, 10 1,000 server clusters versus 1 10,000 server cluster, you’ll get much much better efficiency if you can schedule arbitrarily in the 10,000 server cluster. If you can place your jobs anywhere across 10,000 servers rather than have to fit within a 1,000 servers the number work out to be quite stark. So if a network could be built to scale to an entire building there is much more efficiency out of the computes and out of the disks. And those wind up being the dominate costs. The network can quite inexpensive and can be quite an enabler for compute and storage.    

    *       “Resource stranding” is when you have leftover CPU in one place and leftover memory in another (    [    source    ](http://jturgasen.github.io/2014/10/26/tech-takeaway-012-cluster-management-at-google/)    ).    

*       Balancing your datacenter.    

    *       Once you get to the scale of a building you have to make sure you deliver sufficient bandwidth.    

    *   An unbalanced datacenter has **some resource being scarce which limits your value**. If one resource is scarce it means some other resources are sitting idle, which increases your costs.

    *       Typically at datacenter scale the resource that is most scarce is the network. The network tends to be under provisioned because we don’t know how to build big networks that deliver lots of bandwidth.    

*       Bandwidth. Amdahl has another law.    

    *       You need 1 Mbit/sec of IO for every 1 Mhz of computation for parallel computation.    

    *       Let’s say for example purposes only in a future datacenter near you that compute servers have 64*2.5 Ghz cores then to be balanced each server needs about 100 Gb/s of bandwidth. This is not local IO. Local IO doesn’t matter. You need to access datacenter wide IO. The datacenter may have 50k servers; flash storage at 100k+ IOPS, 100 microseconds access time, petabytes of storage; and in the future some other Non Volatile Memory technology will have 1M+ IOPS, 10 microsecond access times, and terabytes of storage.    

    *       So you would need a 5 Pb/s network and correspondingly capable switches. Even with a 10:1 oversubscription ratio this means you would need a 500Tb/s datacenter network to come close to achieving balance. For perspective, a  500Tb/s network has more capacity than the entire internet today (which is probably near 200Tb/s).    

*       Latency. To achieve goals of storage infrastructure disaggregation you need predictable low latency.    

    *       Disks are slow with 10 millisecond access latency so they are easy to make look local. The network is faster than that.    

    *       Flash and NVM are much harder.    

*       Availability. You have a building worth of computes, new servers need to be introduced continuously, servers need to be upgraded to go from 1G -> 10G -> 40G -> 100G -> ???    

    *       The building can’t be taken down for the upgrade to happen. The investment is just too large. Refreshing the network and servers must be constant. The old must live with the new without interrupting very much capacity at all.    

    *       Scale up networking doesn’t work. It’s not possible to do perform a scorched earth upgrade on a network that criss crosses the entire datacenter.    

*       Google Cloud Platform is built on a datacenter network infrastructure that supports Google scale, performance, and availability. This is being opened up to the public. The hope is the next great service can leverage this infrastructure without having to invent it.    

*       Key challenges are providing isolation while opening up the raw capacity of the hardware.     

##     Software Defined Networking    

*       Networking is a key enabler and differentiator for enabling next-generation computer architecture. SDN will play a key role.    

*       SDN is about a software     [    control plane    ](http://sdntutorials.com/difference-between-control-plane-and-data-plane/)     that abstracts and manages complexity. It’s about moving beyond the box. It’s about changing the interface to your network not to be standard protocols, but being about the software control plane that manages the complexity, hides it, and can make 10,000 switches look like one.    

*       Hardware will be there. How do you manage it as if it were one large entity? That’s what it comes down to on the server side, storage side, and the networking side.    

*       Google started where everyone started in building datacenter networks by using a four-post cluster network. The size and bandwidth of the network was decided by the biggest router they could buy. When more capacity was needed they need to build another datacenter network with an even larger router.    

*       The approach around Clos topologies is to use commodity switch silicon and build over five generations networks that look something like:      

##     ![](https://lh4.googleusercontent.com/xK48BYy9f3FMGsmk8KAkpcfsTUVLrKpst2uqYiIKiogHgS7IGpEpEeT9iLd5DOjggK7IcQ6PIysC2varP7O1g39Od2O3B67Cvg6cJ2ip3CkZF4ktxUvBI0ptcYD3Mppo10ZgtQ)    

*       The hierarchical Clos based topology says leverage merchant silicon, in a Clos topology to provide high bandwidth at the scale of a building. The same switch silicon that powers the top of rack switches, that connect servers together, also powers the aggregation blocks, which provide connectivity among some number of racks. The same silicon powers the spine layer that connects the edge aggregation layers together.    

*       Ten years ago aggregate bandwidth was 2 terabits/s. The latest generation Jupiter fabrics provide 1.3 Pb/s bandwidth. Their Jupiter Superblock switch delivers 80Tb/s of bandwidth, it hosts their SDN software stack through OpenFlow and is remotely controlled. The switch powers all of their datacenters with the balance that Google has acquired.    

##     Network Control    

*       In the early days Google faced a choice of how to build their control plane. Use the existing service provider related stack of OSPF, ISIS, BGP, etc. Or build their own.    

*       They chose to build their own for several reasons.    

    *       The topologies Google was considering a decade ago needed multipath forwarding to work. Lots and lots of paths between the source destination were needed to achieve the desired bandwidth. Existing protocols didn’t support multipath. They were about connectivity. Finding a path from the source to the destination, not the best best, and not multiple paths.    

    *       Ten years ago there were no high-quality open source stacks.    

    *       All the protocols were based broadcasts and at the scale Google was looking to operate at they were worried about the scalability of broadcast based protocols.    

    *       Once the broadcast based protocols are installed you have to configure each individual box to use them and Google doesn’t like that.    

*       Putting it all together with the new conventional wisdom learned at Google in building massive systems at scale:    

    *   **Logically centralize**(this does not mean a single server) with a hierarchical control plane with peer to peer data plane beats full decentralization. Seen it play out over and over again, and played out with datacenter networks as well.

    *       **Scale out**         is much more convenient than scale up. This was seen with GFS, MapReduce, BigTable, and Andromeda.    

    *       **Centralized configuration and management**         dramatically simplifies all system aspects as well as deploying.    

##     Related Articles    

*   [On HackerNews](https://news.ycombinator.com/item?id=10037960) / [Jupiter Rising on HackerNews](https://news.ycombinator.com/item?id=9977414)

*       [Google's experience with Software Defined Network Function Virtualization at Scale](https://www.youtube.com/watch?v=n4gOZrUwWmc)         (Google video, 2014)    

*   [A visual look at Google’s innovation in Global Network Infrastructure](http://googlecloudplatform.blogspot.com/2015/08/a-visual-look-at-googles-innovation-in.html)

*   [Pulling Back the Curtain on Google’s Network Infrastructure](http://googleresearch.blogspot.com/2015/08/pulling-back-curtain-on-googles-network.html)

*       [A look inside Google’s Data Center Networks](http://googlecloudplatform.blogspot.com/2015/06/A-Look-Inside-Googles-Data-Center-Networks.html)         (Google blog post,     [    on HackerNews    ](https://news.ycombinator.com/item?id=9734305)    ,     [    on Reddit    ](https://www.reddit.com/r/sysadmin/comments/3a7kwc/a_look_inside_googles_data_center_networks/)    )    

*   [    Jupiter Rising: A Decade of Clos Topologies and Centralized Control in Google’s Datacenter Network    ](http://conferences.sigcomm.org/sigcomm/2015/pdf/papers/p183.pdf)     (Google paper, 2015)    

*   [    SDN Stack for Service Provider Networks    ](https://www.youtube.com/watch?v=ED51Ts4o3os)     (Google video, 2012).    

*   [    Show 222 – Introducing The OpenClos Project    ](http://packetpushers.net/show-222-introducing-openclos-project/)     (networking nitty gritty from Packet Pushers)    

*   [    Google Throws Open Doors to Its Top-Secret Data Center    ](http://www.wired.com/2012/10/ff-inside-google-data-center/)     (Wired, 2012)    

*   [    Changing Architectures: New Datacenter Networks Will Set Your Code And Data Free    ](http://highscalability.com/blog/2012/9/4/changing-architectures-new-datacenter-networks-will-set-your.html)     (from HighScalability)    

*   [    VL2: A Scalable and Flexible Data Center Network    ](http://research.microsoft.com/apps/pubs/default.aspx?id=80693)     (paper from Microsoft)    

*   [    Google On Latency Tolerant Systems: Making A Predictable Whole Out Of Unpredictable Parts    ](http://highscalability.com/blog/2012/6/18/google-on-latency-tolerant-systems-making-a-predictable-whol.html)     (from HighScalability)    

*   [    Introducing data center fabric, the next-generation Facebook data center network    ](https://code.facebook.com/posts/360346274145943/introducing-data-center-fabric-the-next-generation-facebook-data-center-network)

*   [    The Practice of Cloud System Administration: Designing and Operating Large Distributed Systems    ](http://the-cloud-book.com/)     (looks like a good book)    

*   [    Inside A Decade Of Google Homegrown Datacenter Networks    ](http://www.theplatform.net/2015/06/19/inside-a-decade-of-google-homegrown-datacenter-networks/)

*   [    Tech Takeaway 012: Cluster Management at Google    ](http://jturgasen.github.io/2014/10/26/tech-takeaway-012-cluster-management-at-google/)

*       [Evaluating job packing in warehouse-scale computing](http://www.e-wilkes.com/john/papers/2014-IEEECluster-job-packing.pdf)    

*   [Exascale Computing—a fact or a fiction?](http://www.ipdps.org/ipdps2013/SBorkar_IPDPS_May_2013.pdf)

*   [RECONCILING HIGH EFFICIENCY WITH LOW LATENCY IN THE DATACENTER](http://web.stanford.edu/~davidlo/resources/2015.thesis.pdf)

*   [ONS 2015: Wednesday Keynote - Mark Russinovich](https://www.youtube.com/watch?v=RffHFIhg5Sc) - Microsoft's take on networking.

    