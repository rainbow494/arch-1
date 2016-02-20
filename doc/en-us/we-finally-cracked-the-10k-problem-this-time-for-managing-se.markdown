## [We Finally Cracked the 10K Problem - This Time for Managing Servers with 2000x Servers Managed Per Sysadmin](/blog/2013/11/19/we-finally-cracked-the-10k-problem-this-time-for-managing-se.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, November 19, 2013 at 8:54AM</span></div>

<div class="body">

![](http://farm6.staticflickr.com/5527/10932165644_271467dbb7_m.jpg)

<span>In 1999 Dan Kegel issued a big hairy audacious challenge to web servers:</span>

> <span>It's time for web servers to handle ten thousand clients simultaneously, don't you think? After all, the web is a big place now.</span>

<span>This became known as the</span> [<span>C10K problem</span>](http://www.kegel.com/c10k.html)<span>. Engineers solved the C10K scalability problems by fixing OS kernels and moving away from threaded servers like Apache to event-driven servers like Nginx and Node.</span>

<span>Today we are considering an even bigger goal, how to support</span> [<span>10 Million Concurrent Connections</span>](http://highscalability.com/blog/2013/5/13/the-secret-to-10-million-concurrent-connections-the-kernel-i.html)<span>, which requires even more radical techniques.</span>

<span>No similar challenge was issued for managing servers in a datacenter, but according to</span> [<span>Dave Neary</span>](https://twitter.com/nearyd) <span>from Red Hat, in a recent</span><span> [FLOSS Weekly](http://twit.tv/show/floss-weekly/273) episode</span><span>, we have passed the 10K barrier for server management with 10,000 or more servers managed per sysadmin.</span>

## <span>Should we let this milestone pass without mention?</span>

<span>Absolutely not! It’s a stunning accomplishment with 200x-2000x increases in productivity. Dave said he remembered in the 1990s it took one sysadmin to manage 4 or 5 Windows servers. A Linux sysadmin could manage 50 to 60 servers.</span>

<span>Now companies are managing over 10,000 servers per sysadmin. This huge change is rooted both in</span> [<span>IaaS</span>](http://en.wikipedia.org/wiki/Cloud_computing)<span>, treating a datacenter as an elastic programmable resource, divorcing operations from infrastructure deployment, and in the</span> [<span>DevOps revolution</span>](http://en.wikipedia.org/wiki/DevOps)<span>, with its emphasis on tools, culture, automation, metrics, sharing of resources, and infrastructure as code.</span>

## <span>What will it take to manage 10 million servers per sysadmin?  </span>

<span>Who might know? Google of course.</span>

<span>As James Hamilton says,</span> [<span>Counting Servers is Hard</span>](http://perspectives.mvdirona.com/2013/07/17/CountingServersIsHard.aspx)<span>, but Microsoft says they have</span> [<span>1 million servers</span>](http://www.extremetech.com/extreme/161772-microsoft-now-has-one-million-servers-less-than-google-but-more-than-amazon-says-ballmer)<span>, and Google is planning for</span> <span>[10 million servers](http://www.theregister.co.uk/2009/10/23/google_spanner/), s</span><span>o it may take a while before we can get to 10 million servers per sysadmin.</span>

<span>But when it does happen the base will be built on:</span>

*   <span>Treating</span> [<span>The Datacenter As A Computer</span>](http://highscalability.com/blog/2013/8/22/the-datacenter-as-a-computer-an-introduction-to-the-design-o.html)<span>.</span>

*   <span>And within the datacenter</span> [<span>Multiplex Multiple Works Loads On Computers To Increase Machine Utilization And Save Money</span>](http://highscalability.com/blog/2013/11/13/google-multiplex-multiple-works-loads-on-computers-to-increa.html)<span>.</span>

*   <span>But that’s just a single datacenter. That doesn’t get you 10 to million servers. For 10 million servers you have to exploit many datacenters, so you build a system like</span> [<span>Spanner</span>](http://highscalability.com/blog/2012/9/24/google-spanners-most-surprising-revelation-nosql-is-out-and.html) <span>that can scale up to millions of machines across hundreds of datacenters and trillions of database rows.</span>

*   <span>Then of course you’ll need to create an amazing</span> [<span>world spanning network</span>](http://www.opennetsummit.org/archives/apr12/vahdat-wed-sdnstack.pdf) <span>to connect it all together.</span>

*   <span>But to really get 10 million servers per sysadmin you’ll probably need a huge dose of</span> [<span>Deep Learning</span>](http://www.technologyreview.com/featuredstory/513696/deep-learning/) <span>to make sense of it all.</span>

<span>At a high level the approach of scaling to 10 million connections per server and scaling 10 million machines per sysadmin are the same:</span> <span>**scalability is specialization**</span><span>.</span>

<span>But at lower level they differ completely. Scaling to 10 million connections is about removing layers and doing the work yourself. Scaling to 10 million servers is all about putting the intelligence into smarter and smarter layers. A lot like how human body utilizes trillions of individual components mediated by many autonomous systems all directed by a parallelized and decentralized brain.</span>

## <span>Related Articles</span>

*   [Facebook Ops: Each Staffer Manages 20,000 Servers](http://www.datacenterknowledge.com/archives/2013/11/20/facebook-ops-staffer-manages-20000-servers/)

</div>