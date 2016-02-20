## [Paper: Dynamo: Amazon’s Highly Available Key-value Store](/blog/2007/10/30/paper-dynamo-amazons-highly-available-key-value-store.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, October 30, 2007 at 11:43PM</span></div>

<div class="body">

**Update 2**: Read/WriteWeb has a good article talking about the scalability issues of relational databases and how Dynamo solves them: [Amazon Dynamo: The Next Generation Of Virtual Distributed Storage](http://www.readwriteweb.com/archives/amazon_dynamo.php). But since Dynamo is just another frustrating walled garden protected by barbed wire and guard dogs, its relevance is somewhat overstated.  

**Update**: [Greg Linden](http://glinden.blogspot.com/2007/10/highly-available-distributed-hash.html) has a take on the paper where he questions some of Amazon's design choices: emphasizing write availability over fast reads, a lack of indexing support, use of random distribution for load balancing, and punting on some scalability issues.  

Werner Vogels, Amazon's avuncular CTO, just announced a new [paper](http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf) on the internal database technology Amazon uses to handle tens of millions customers. I'll dive into more details later, but I thought you'd want to read it hot off the blog. The bad news is it won't be a service. They are keeping this tech not so secret, but very safe. Happily, it's another real-life example to learn from. As many top websites use a highly tuned key-value database at their core instead of a RDBMS, it's an important technology to understand.  

From the [abstract](http://s3.amazonaws.com/AllThingsDistributed/sosp/amazon-dynamo-sosp2007.pdf) you can get a feel for what the paper is about: 

> _Reliability at massive scale is one of the biggest challenges we  
> face at Amazon.com, one of the largest e-commerce operations in  
> the world; even the slightest outage has significant financial  
> consequences and impacts customer trust. The Amazon.com  
> platform, which provides services for many web sites worldwide,  
> is implemented on top of an infrastructure of tens of thousands of  
> servers and network components located in many datacenters  
> around the world. At this scale, small and large components fail  
> continuously and the way persistent state is managed in the face  
> of these failures drives the reliability and scalability of the  
> software systems.  
>   
> This paper presents the design and implementation of Dynamo, a  
> highly available key-value storage system that some of Amazon’s  
> core services use to provide an “always-on” experience. To  
> achieve this level of availability, Dynamo sacrifices consistency  
> under certain failure scenarios. It makes extensive use of object  
> versioning and application-assisted conflict resolution in a manner  
> that provides a novel interface for developers to use._

My first impressions after reading the paper:

*   Wow. But crap, I'll never be able to build anything like that. This is really competition through better infrastructure. Take that [Google](http://highscalability.com/google-architecture) :-)*   Their purposeful embracing of probability and manged centers of uncertainty must be dizzying for those from a RDBMS background. In a RDBMS it's all right angles. You write something and it's assumed consistent, correct, and durable. Now, how do you do this at scale across multiple data centers under failure conditions? There's the rub. So Amazon says writes must go through and we will deal with the complexities that model generates. They version objects and merge them later. Who does that? I love it, because when delve into these problems you realize you need this type of functionality, but it's too complex, so you back away and continue trying to force a square peg in a round whole. To have no fear to go where your requirements leads you is real engineering.*   Can you imagine finding a problem in that system? I'd love to be a fly in those debugging sessions. But infrastructure takes on self-consciousness of its own when dealing with complex problems, so you just have to deal with knowing you don't know anymore.  

    A lot of this thinking is driven by the [CAP](http://citeseer.ist.psu.edu/cache/papers/cs/26764/http:zSzzSztheory.lcs.mit.eduzSztdszSzpaperszSzGilbertzSzBrewer6.pdf/brewer-s-conjecture-and.pdf) conjecture which states it's impossible for a web service to simultaneously guarantee consistency, availability, and partition-tolerance. When you get over your initial "that can't be true" reaction and embrace it, you get something like Dynamo.  

    I'd really love to hear what you guys think about Dynamo.</div>