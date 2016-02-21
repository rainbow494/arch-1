## [Low Level Scalability Solutions - The Conditioning Collection](/blog/2013/3/11/low-level-scalability-solutions-the-conditioning-collection.html)

    

    

![](http://farm9.staticflickr.com/8251/8534554068_6f95503f2f_o.jpg)

We talked about [42 Monster Problems That Attack As Loads Increase](http://highscalability.com/blog/2013/2/27/42-monster-problems-that-attack-as-loads-increase.html). And in [The Aggregation Collection](/blog/2013/3/6/low-level-scalability-solutions-the-aggregation-collection.html) we talked about the value of prioritizing work and making smart queues as a way of absorbing and not reflecting traffic spikes.

Now we move on to our next batch of strategies where the theme is _conditioning_, which is the idea of shaping and controlling flows of work within your application...

## Use Resources Proportional To a Fixed Limit

This is probably the most important rule for achieving scalability within an application. What it means:

*   Find the resource that has a fixed limit that you know you can support. For example, a guarantee to handle a certain number of objects in memory. So if we always use resources proportional to the number of objects it is likely we can prevent resource exhaustion.
*   Devise ways of tying what you need to do to the individual resources.

Some examples:

*   Keep a list of purchase orders with line items over $20 (or whatever). Do not keep a list of the line items because the number of items can be much larger than the number of purchase orders. You have kept the resource usage proportional to the number purchase orders and you know how many purchases orders you can support.
*   Merging, via aggregation, all operations for an object into one request instead of having a separate request for each operation. A create and update, for example could be one request.

## Load Shedding

Rejected work requests until enough resources become available to accept them.

For example, in a call processing system, the number of calls would be limited. Any calls beyond that limit would be rejected. This would give time for the current calls to be processed.

In some systems it is not possible to shed load because you are required to handle all requests. This puts a lot more emphasis on other facets of the scalability solution. Some examples of load shedding:

*   limit the number of ftp sessions a node can support.
*   a web server redirecting to another server when it is too busy
*   the public telephone system saying all calls are busy which prevents new calls from being accepted

## Tie Work To Resource Availability

Reject work until enough resources become available to accept it. It is the more general case of _Load Shedding_.

A sophisticated system monitors the availability of resources like CPU, queue depth, memory, and network bandwidth and shapes work loads accordingly.

A good example of this principle is polling with a _Data Grid_, which is a framework for the scalable transfer of large data sets through a network.

A Data Grid framework polls data and brings it into the node that is doing the processing. By only polling when the node projects to have available CPU, we know the work will be able to be processed once it is one the node.

The general idea here is to not move data around if it won't be used in a timely manner. The advantage is while data is not being polled, it is still sitting on the source, being aggregated, which is what we want, it means less unnecessary work to do over the entire system.

## Reference Counting

Sharing objects via reference counting can greatly reduce memory usage because only one copy of an object is maintained at any one time.

*   Reference counting means you have to be very careful about taking a large enough scale lock to protect the object.
*   Reference counting means you don't have to copy an object to put it in multiple lists.
*   Reference counting means you can aggregate changes while an object is on a list.
*   Reference counting means you can spring a memory leak if you are not very very careful.

An example is a change notification that must go out to multiple consumers. The change notification can be reference counted and put on each outbound queue. The only memory used is for the channel data structures because the same object is shared amongst all.

We do not copy objects into each queue. Objects are not snapshotted. While an object is in a queue it is still aggregating, saving resources as the world changes around it.

## End to End Application Level Flow Control

Put back pressure on a sender when the receiver has reached a resource limit. The general idea is to constrain senders by the receivers ability to handle the load.

TCP, for example, has flow control mechanisms to block senders until data has been sent. But developers tend to forget TCP != applications. Just because data has been sent across to another node doesn't mean jack. Until an application does something with the data and successfully returns a reply, nothing useful has happened.

Applications tend to be stupid and retry their way out failures. When a sender has no feedback from a receiver the naive approach is to just keep sending messages, hoping one would go through. If the timeout period is short, as often happens, a lot of useless messages flow through the system.

This failed approach allocates more memory, fills up queues, causes drops,  causes retransmits, uses more CPU, causes the network layer go crazy, causes nearly impossible to find deadlock bugs to decloak, and generally uses more resources, which makes a bad problem worse.

Eventually the receiver will just be flooded with duplicate work, which is in effect a DDOS attack on your own system, and in a naive system the problems start all over again.

There's a life lesson there I think.

Sticking a lot of stuff in an outbound queue is not getting work done. Think about each application not sending another request to another application until a previous request has completed. With batching you get more work done than you think.

This lets requests aggregate in your own queues so the world may have completely changed by the time a request needs to be executed.

Separate out your node status, from your link level status, from your application level status. Do something intelligent when any of these things fail or have degraded service. If you don't have this kind of infrastructure then you need to build it.

All messages from applications on a node to another node travel over the same link. When the node is down or that link is down then all applications should have higher level policies in place that know how to resolve the situation. They should stop accepting work involving downed or compromised resources. Push back on senders telling them that you are down for this class of work so don't send it. When your communication layer detects a link is back up or node as back up then reverse the process.

The same idea applies to individual applications. A node can bee seen as a collection of applications, each application having their own resource limits, including available CPU processing time and priority. When applications fail or degrade then the system must react intelligently to reduce the workload in the entire system.

## Remove Round Trips

Give a client all the information it needs rather than have the client come back down to the server to get the information. By giving the client the state information it needs we remove a round trip which reduces the load on the server because clients do not have to come back to the server. During event storms this can be a large load, especially if events are not being aggregated. 

With UIs, for example, the GUI requests will often connect to a proxy which will perform dozens of operations on the server side, aggregate the operations on the server, and then return a single responses.

Local caching is another way to prevent round trips.

## Remove Retries 

Removing sources of retries in your system makes your system more scalable.

For example, when you are hitting a web site and it doesn't accept your connection you are forced to retry. This causes more load in the system which inturn causes more retries. And because the timeouts for http connections are so long, what the user ends up seeing is very poor service.

To reduce the number of retries and to provide better perceived service, it would be better to accept the http connections as fast as possible. This allows the connections to be setup. Even if later portions of processing are slower, the user will get a better experience and you will reduce the load caused by excessive retries.

[SEDA](http://www.eecs.harvard.edu/~mdw/proj/seda/) uses a pipelining approach, which is a good way to condition work within an application.

## Remove Single Points of Serialization

A point of serialization is anywhere where everything must be looked at before moving on to the next stage of processing. You can't scale effectiviely with single points of serialization. 

Some techniques for removing single points of serialization are:

*   [Data Parallel Algorithms](http://en.wikipedia.org/wiki/Data_parallelism)
*   Load Balancing
*   [Hash Based Node Selection](http://en.wikipedia.org/wiki/Distributed_hash_table)

## Load Balancing

Allocate work between multiple servers.

*   DNS servers can allocate requests amongst multiple machines
*   Hardware routers can allocate requests amongst multiple machines
*   Clients can keep a map of servers to select servers from the client.

There is often a large infrastructure in place to replicate state so that requests in the same session can access state from any server.

Clearly mostly read only applications can make the best use of load balancing because the write update consistency problems are not present.

Often L4-L7 switches used to load balance servers at line rate.

Or you can load balance without the assist of hardware using [Hash Based Node Selection](http://en.wikipedia.org/wiki/Distributed_hash_table).

## Separate Control and Data Planes

The idea is that high priority control messages should never block behind data or lower priority control traffic. Control traffic are simply commands like show me what's happening there, reboot this server, kill this process, change that route, block this user, bring up a server, move to another datacenter, kill this connection, anything that you use to recover from problems. Structure your system to prevent useless work being done while making sure high priority work gets done when it needs to get done. 

*   Reduce chatter. Chatty programs pump out endless low priority control and data traffic that keeps your system busy doing nothing useful at all.
*   Create a separate network for control and data so control messages always go through. This may seem extreme, but if all your messaging goes over the same network you can't guarantee you'll have an open communications channel when you really need it most.
*   Use intelligent retry policies so useless work doesn't pile up in queues.
*   Delete obsoleted messages from queues. When the newest save the day control message is available current work is stopped so the higher priority item can be processed and it's messages will go out immediately as apposed to sitting back and waiting.

## Create Idempotent Services

The term _idempotent_ refers to something which has the same effect if used multiple times as it does if used only once. Idempotency helps handle unreliable communication channels.

Stateless protocols can be simulated by introducing distributed state and by making operations idempotent. Stateless Servers like NFS often use this strategy.

### HTTP Example

In HTTP, some methods (such as GET) are idempotent, while other methods (such as POST) are not. 

### Fuel Rod Example

In interesting example is moving a fuel rod in a nuclear power plant. Let's take two versions of the move command:

*   move_absolute_from_the_top(inches)
*   move_relative(inches)

The absolute version is idempotent because no matter how many times you call it the fuel rod is in the same place.

In the relative version if there are command drops and resends the fuel rod will keep moving the number of inches for every application of the command. This means the fuel rod may not be where expected when a command returns. Oh oh!

### NFS Example

A request to write 5 bytes at offset 165 in a file is idempotent; a request to write 5 bytes at the current end-of-file is not.

NFS employs idempotent operations wherever possible. Certain operations are inherently not idempotent, for example, deleting a file, so NFS server implementations will normally include mechanisms to attempt to detect duplicate requests and furnish the appropriate results. 

## Congestion Control

Good explanation from [http://www.eventhelix.com/RealtimeMantra/CongestionControl](http://www.eventhelix.com/RealtimeMantra/CongestionControl/):

> A congestion control system typically monitors various factors like CPU occupancy, link occupancy and messaging delay. Based on these factors it takes a decision if the system is overloaded. If the system is overloaded, it initiates actions to reduce the load by asking front end processors to reject traffic. The throttling of traffic will reduce the load but it there will be a certain time delay before which the monitored variables like CPU and Link occupancy show downward trend. Congestion control systems are designed to take this into account by spacing out congestion control actions. If the system continues to be overloaded, subsequent congestion control actions can further increase the traffic throttling. If the traffic load is just right, the system maintains current traffic throttling actions. If the system gets under loaded, the traffic throttling is reduced.

This is the kind of monitoring you need to build into your communication layer and distribute amongst all the nodes in your system.

[Gossip protocols](http://highscalability.com/blog/2011/11/14/using-gossip-protocols-for-failure-detection-monitoring-mess.html) are one way to distribute this information though the system so each entity can take proper action. [Zookeeper](http://highscalability.com/zookeeper-reliable-scalable-distributed-coordination-system) is something else to look into as a way of distributing state.

    