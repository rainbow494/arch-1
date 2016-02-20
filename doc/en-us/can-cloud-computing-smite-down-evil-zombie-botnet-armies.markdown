## [Can cloud computing smite down evil zombie botnet armies?](/blog/2008/7/10/can-cloud-computing-smite-down-evil-zombie-botnet-armies.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, July 10, 2008 at 5:14AM</span></div>

<div class="body">

In the more cool stuff I've never heard of before department is something called _[Self Cleansing Intrusion Tolerance](https://www.linuxworld.com/news/2008/061908-scit.html)_ (SCIT). [Botnets](http://en.wikipedia.org/wiki/Botnet) are created when vulnerable computers live long enough to become infected with the will to do the evil bidding of their evil masters. Security is almost always about removing vulnerabilities (a process which to outside observers often looks like a [dog chasing its tail](http://video.google.com/videoplay?docid=-6983410998096885679)). SCIT takes a different approach, it works on the availability angle. Something I never thought of before, but which makes a great deal of sense once I thought about it.  

With SCIT you stop and restart VM instances every minute (or whatever depending in your desired window vulnerability)....

This short exposure window means worms and viri do not have long enough to fully infect a machine and carry out a coordinated attack. A machine is up for a while. Does work. And then is torn down again only to be reborn as a clean VM with no possibility of infection (unless of course the VM mechanisms become infected). It's like curing cancer by constantly moving your consciousness to new blemish free bodies. Hmmm...  

SCIT is really a genius approach to scalable (I have to work in scalability somewhere) security and and fits perfectly with cloud computing and swarm (cloud of clouds) computing. Clouds provide plenty of VMs so there is a constant ready supply of new hosts. From a software design perspective EC2 has been training us to expect failures and build [Crash Only Software](http://en.wikipedia.org/wiki/Crash-only_software). We've gone stateless where we can so load balancing to a new VM is not problem. Where we can't go stateless we use work queues and clusters so again, reincarnating to new VMs is not a problem. So purposefully restarting VMs to starve zombie networks was born for cloud computing.  

If a wider move could be made to cloud backed thin clients the internet might be a safer place to live, play, and work. Imagine being free(er) from spam blasts and DDOS attacks. Oh what a [wonderful world](http://video.google.com/videoplay?docid=-6983410998096885679) it would be...

</div>