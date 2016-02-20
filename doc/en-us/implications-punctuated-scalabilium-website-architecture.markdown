## [The Implications of Punctuated Scalabilium for Website Architecture](/blog/2009/3/11/the-implications-of-punctuated-scalabilium-for-website-archi.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, March 11, 2009 at 5:29AM</span></div>

<div class="body">**Update:** [How do you design and handle peak load on the Cloud?](http://www.cloudiquity.com/2009/03/how-do-you-design-and-handle-peak-load-on-the-cloud/#) by Cloudiquity. Gives a formula to try and predict and plan for peak load and talks about how GigaSpaces XAP, Scalr, RightScale and FreedomOSS can be used to handle peak load within EC2.  

Theo Schlossnagle, with his usual insight, talks about in [Dissecting today's surges](http://www.lethargy.org/~jesus/archives/118-Dissecting-todays-surges.html) how the nature of internet traffic has evolved over time. Traffic now spikes like a heart attack, larger and more quickly than ever from traffic inflow sources like Digg and The New York Times. Theo relates how _At least eight times in the past month, we've experienced from 100% to 1000% sudden increases in traffic across many of our clients_ and those spike can happen as quickly as 60 seconds. To me this sounds a lot like [Punctuated equilibrium](http://en.wikipedia.org/wiki/Punctuated_equilibrium) in evolution, a force that accounts for much creative growth in species...  

VMs don't spin up in less than 60 seconds so your ability to respond to such massive quick spikes is limited. This assumes of course that you've created an architecture that can automatically scale by adding VMs. Such elastic demand is usually met with a reservoir. You have more VMs in reserve to soak up temporary spikes. But who would do this in reality? Money would be going to non productive VMs, so you are likely to already have put those VMs into production.  

Interestingly, Theo ties handling sudden unexpected spikes back to performance. We are always told performance and scalability are separate issues. And while I accept this notionally, in my heart of hearts I think they have more in common than not and I think Theo nails why. A well performing system acts as a kind of reservoir for handling spikes before you can ever notice there's a spike. That gives you some time to add more resources to your site if a spike continues. With that reservoir you are just crushed.  

Theo gives four rules for for handling spikes: Be alert, Be prepared, Perform triage, and Be calm. Please see his site for more discussion of these rules.  

A few things that might help:  
*   Create fast booting VMs. It's easy to create VMs that boot glacially (intentional irony). The more you leave to run-time like software downloads and configuration, the slower your VMs boot and the slower you can react to spikes.  
    *   Cloud vendors offer a service to maintain an image cache. It would be useful if a service was offered that could guaranteed faster provisioning of VMs and quicker download of images.  
    *   Would an in-cloud service to offer stem cell VMs make sense? This is a VM that could quickly become any one of a number of different images on demand. So a service could keep a reservoir of stem cell VMs up and running, shared by a number of customers, and an application could request the low latency spin up of one of the reserved VMs.  

    The idea that internet traffic patterns have evolved such that even our cloud architectures can't easily cope is an interesting one. I find it ironic that many of the techniques needed to build real-time systems are helpful to handle this new world too when at first glance the problems look nothing alike. Sometimes piling on more resources isn't enough, efficiency matters too.  
    </div>