## [Problem: Mobbing the Least Used Resource Error](/blog/2008/3/14/problem-mobbing-the-least-used-resource-error.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Friday, March 14, 2008 at 3:04AM</span></div>

<div class="body">

_A thoughtful reader recently suggested creating a series of posts based on real-life problems people have experienced and the solutions they've created to slay the little beasties. It's a great idea. Often we learn best from great trials and tribulations. I'll start off the new "Problem Report"  
feature with a diabolical little problem I dubbed the "Mobbing the Least Used Resource Error." Please post your own. And if you know someone with an interesting problem report, please tag them too. It could be a lot of fun. Of course, feel free to scrub your posts of all embarrassing details, but be sure to keep the heroic parts in :-)_  

### The Problem

There's an unexpected and frequently fatal type of error that can happen when new resources are added to a horizontally scaled architecture. Because the new resource has the least of something, load or connections or whatever, a load balancer configured with a least metric will instantaneously direct all new traffic to that new resource. And bam! Your system dies. All the traffic that was meant to be spread across your entire cluster is now directed like a laser beam to one small part of it.  

I love this problem because it's such a Heisenberg. Everyone is screaming for more storage space so you bring up a new filer. All new data streams flow to the new filer and it crumbles and crawls because it can't handle the load for the entire system. It's in the very act of turning up more storage you bring your system down. How "cruel world the universe hates me" is that?

Let's say you add database slaves to handle load. Your load balancer redirects traffic to the new slaves, but the slaves are trying to sync, yet they can't sink because they are getting hammered by the new traffic. Down goes Frazier.  

This is the dark side of partitioning. You partition data to get high performance via parallelization. For example, you hash on the user name to a cluster dedicated to handle those users. Unless your system is very flexible you can't scale anymore by adding resources because you can't repartition the data. All users are handled by their cluster. If you want a different organization you would have to redistribute data across all the clusters. Most systems can't handle that and you end not being able to scale out as easily as you hoped.  

### The Solution

The solution depends of course on the resource in question. Butting knowing a potential problem is present gives you the heads up you need to avoid destruction.  

*   For filers migrate storage from existing filers to the new filers so storage is evened out. Then new storage will be allocated evenly across all the filers.*   For services have a life cycle state machine indicating when a service is up and ready for work. Simply being alive doesn't mean it's ready.*   [Consistent Hashing](http://www.spiteful.com/2008/03/17/programmers-toolbox-part-3-consistent-hashing/) to assign resources to a pool of servers in a scalable fashion.*   For servers use random or round-robin balancing when the load balancer can receive incorrect feedback from pool servers.  

    The [Thundering Herd Problem](http://en.wikipedia.org/wiki/Thundering_herd_problem) is supposedly the same problem described here, but it doesn't seem the same to me.</div>