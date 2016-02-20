## [Facebook's Memcached Multiget Hole: More machines != More Capacity ](/blog/2009/10/26/facebooks-memcached-multiget-hole-more-machines-more-capacit.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, October 26, 2009 at 7:00AM</span></div>

<div class="body">

When you are on the bleeding edge of scale like Facebook is, you run into some interesting problems. As of 2008 [Facebook had over 800 memcached servers](http://www.facebook.com/note.php?note_id=39391378919) supplying over 28 terabytes of cache. With those staggering numbers it's a fair bet to think they've seen their share of Dr. House worthy memcached problems.

Jeff Rothschild, Vice President of Technology at Facebook, [describes one such problem](http://highscalability.com/blog/2009/10/12/high-performance-at-massive-scale-lessons-learned-at-faceboo-1.html) they've dubbed the **Multiget Hole.**

**<span style="font-weight: normal;">You fall into the multiget hole when memcached servers are</span> <span style="font-weight: normal;">CPU bound</span><span style="font-weight: normal;">, adding more memcached servers seems like the right way to add more capacity so more requests can be served, but against all logic adding servers doesn't help serve more requests. This puts you in a hole that simply adding more servers can't dig you out of. What's the treatment?</span>**

**<span style="font-weight: normal;">Dr. House would immediately notice the hidden clue, we are talking requests not memory. We aren't running out of memory to store stuff, we are</span> <span style="font-weight: normal;">running out of CPU power to process requests</span><span style="font-weight: normal;">. </span>**

**<span style="font-weight: normal;">What happens when you add more servers is that</span> <span style="font-weight: normal;">the number of requests is not reduced, only the number of keys in each request is reduced</span><span style="font-weight: normal;">. The number keys returned in a request</span> <span style="font-weight: normal;">only matters if you are bandwidth limited</span><span style="font-weight: normal;">. The server is still on the hook for processing the same number of requests. Adding more machines doesn't change the number of request a server has to process and since these servers are already CPU bound they simply can't handle more load. So adding more servers doesn't help you handle more requests. Not what we usually expect. This is another example of why architecture matters.</span>**

### <span style="font-weight: normal;"><span style="font-weight: normal;">Understanding Multiget</span></span>

**<span style="font-weight: normal;">To understand why we are serving the same number of requests we have to understand memcached's [multiget request](http://code.google.com/p/memcached/wiki/FAQ#Batch_your_requests_with_get_multi). The multiget request allows the</span> <span style="font-weight: normal;">multiple keys to be retrieved in one request</span><span style="font-weight: normal;">. If a user has 100 friends, for example, the changes for each of those friends can be retrieved by making one request. This if far more efficient than making 100 individual requests.</span>**

**<span style="font-weight: normal;">Multiget allows library makers to transparently use two classic scalability tactics: **batching** and **parallelization**.</span>**

**<span style="font-weight: normal;">Let's say there's a memcached pool containing two servers and 50 friends are stored on each server. What a smart library implementation can do is batch up the requests destined for each memcached server and run those requests in parallel. Memcached works by mapping keys to memcached servers, in practice there's no reason keys would be distributed 50 to each server, but this is just an example. Instead of sending a request per friend we are just sending one request per server. The power of batching is to radically reduce request latency by reducing the number of requests.</span>**

**<span style="font-weight: normal;">Now let's say we make 50 requests for all 100 hundred friends of our moderately popular user. Each server will see 50 requests because half the friends are on each server. If we see that the pool servers are running out of CPU our most likely reaction is to add another server to the pool.  

What does adding another server to the pool accomplish? It means 33ish friends will be stored on each server. When we send out 50 requests to gather info for the 100 friends each server is still seeing 50 requests because to collect all 100 friends we have to hit each server. We've done absolutely nothing to reduce the usage of our scarce resource which is CPU. True, we'll use less bandwidth per server, but that doesn't matter because we have enough bandwidth.   

The astounding result of this exercise is that adding more machines</span> <span style="font-weight: normal;">does not add more capacity</span><span style="font-weight: normal;">. Mr. Rothschild said this isn't a problem they sat down and reasoned through from first principles. This is a problem they saw in the field and learned about from experience. They saw that adding more machines to increase capacity and had to work out what the heck was happening.  
</span>**

### <span style="font-weight: normal;">How do you solve the multiget hole problem?</span>

**<span style="font-weight: normal;">One solution to the multiget hole problem is replication. Since the problem is a lack of CPU power more CPU needs to be applied.</span>**

**<span style="font-weight: normal;">One classic technique to allow more CPU to churn on data is to replicate the data and load balance requests between the replicas. In our example we would create two pools of two servers each. Each pool would get half the requests so they do half the work and they would no longer CPU bound. Now you've doubled the capacity of your system and avoided stepping into the hole.</span>**

### **<span style="font-weight: normal;">Related Articles</span>**

*   [dormando - The "multiget hole" and how none of this is new](http://dormando.livejournal.com/521163.html) - 

**<span style="font-weight: normal;">  
</span>**</div>