## [We Finally Cracked the 10K Problem - This Time for Managing Servers with 2000x Servers Managed Per Sysadmin](/blog/2013/11/19/we-finally-cracked-the-10k-problem-this-time-for-managing-se.html)

    

    

![](http://farm6.staticflickr.com/5527/10932165644_271467dbb7_m.jpg)

    In 1999 Dan Kegel issued a big hairy audacious challenge to web servers:    

>     It's time for web servers to handle ten thousand clients simultaneously, don't you think? After all, the web is a big place now.    

    This became known as the     [    C10K problem    ](http://www.kegel.com/c10k.html)    . Engineers solved the C10K scalability problems by fixing OS kernels and moving away from threaded servers like Apache to event-driven servers like Nginx and Node.    

    Today we are considering an even bigger goal, how to support     [    10 Million Concurrent Connections    ](http://highscalability.com/blog/2013/5/13/the-secret-to-10-million-concurrent-connections-the-kernel-i.html)    , which requires even more radical techniques.    

    No similar challenge was issued for managing servers in a datacenter, but according to     [    Dave Neary    ](https://twitter.com/nearyd)     from Red Hat, in a recent         [FLOSS Weekly](http://twit.tv/show/floss-weekly/273) episode        , we have passed the 10K barrier for server management with 10,000 or more servers managed per sysadmin.    

##     Should we let this milestone pass without mention?    

    Absolutely not! It’s a stunning accomplishment with 200x-2000x increases in productivity. Dave said he remembered in the 1990s it took one sysadmin to manage 4 or 5 Windows servers. A Linux sysadmin could manage 50 to 60 servers.    

    Now companies are managing over 10,000 servers per sysadmin. This huge change is rooted both in     [    IaaS    ](http://en.wikipedia.org/wiki/Cloud_computing)    , treating a datacenter as an elastic programmable resource, divorcing operations from infrastructure deployment, and in the     [    DevOps revolution    ](http://en.wikipedia.org/wiki/DevOps)    , with its emphasis on tools, culture, automation, metrics, sharing of resources, and infrastructure as code.    

##     What will it take to manage 10 million servers per sysadmin?      

    Who might know? Google of course.    

    As James Hamilton says,     [    Counting Servers is Hard    ](http://perspectives.mvdirona.com/2013/07/17/CountingServersIsHard.aspx)    , but Microsoft says they have     [    1 million servers    ](http://www.extremetech.com/extreme/161772-microsoft-now-has-one-million-servers-less-than-google-but-more-than-amazon-says-ballmer)    , and Google is planning for         [10 million servers](http://www.theregister.co.uk/2009/10/23/google_spanner/), s        o it may take a while before we can get to 10 million servers per sysadmin.    

    But when it does happen the base will be built on:    

*       Treating     [    The Datacenter As A Computer    ](http://highscalability.com/blog/2013/8/22/the-datacenter-as-a-computer-an-introduction-to-the-design-o.html)    .    

*       And within the datacenter     [    Multiplex Multiple Works Loads On Computers To Increase Machine Utilization And Save Money    ](http://highscalability.com/blog/2013/11/13/google-multiplex-multiple-works-loads-on-computers-to-increa.html)    .    

*       But that’s just a single datacenter. That doesn’t get you 10 to million servers. For 10 million servers you have to exploit many datacenters, so you build a system like     [    Spanner    ](http://highscalability.com/blog/2012/9/24/google-spanners-most-surprising-revelation-nosql-is-out-and.html)     that can scale up to millions of machines across hundreds of datacenters and trillions of database rows.    

*       Then of course you’ll need to create an amazing     [    world spanning network    ](http://www.opennetsummit.org/archives/apr12/vahdat-wed-sdnstack.pdf)     to connect it all together.    

*       But to really get 10 million servers per sysadmin you’ll probably need a huge dose of     [    Deep Learning    ](http://www.technologyreview.com/featuredstory/513696/deep-learning/)     to make sense of it all.    

    At a high level the approach of scaling to 10 million connections per server and scaling 10 million machines per sysadmin are the same:         **scalability is specialization**        .    

    But at lower level they differ completely. Scaling to 10 million connections is about removing layers and doing the work yourself. Scaling to 10 million servers is all about putting the intelligence into smarter and smarter layers. A lot like how human body utilizes trillions of individual components mediated by many autonomous systems all directed by a parallelized and decentralized brain.    

##     Related Articles    

*   [Facebook Ops: Each Staffer Manages 20,000 Servers](http://www.datacenterknowledge.com/archives/2013/11/20/facebook-ops-staffer-manages-20000-servers/)

    