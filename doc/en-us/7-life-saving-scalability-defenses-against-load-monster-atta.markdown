## [7 Life Saving Scalability Defenses Against Load Monster Attacks](/blog/2013/3/4/7-life-saving-scalability-defenses-against-load-monster-atta.html)

    

    

![](http://farm9.staticflickr.com/8511/8519485352_0fa79b99f8_m.jpg)We talked about [42 Monster Problems That Attack As Loads Increase](http://highscalability.com/blog/2013/2/27/42-monster-problems-that-attack-as-loads-increase.html). Here are a few ways you can defend yourself, secrets revealed by scaling masters across the ages. Note that these are low level programming level moves, not large architecture type strategies.

### Use Resources Proportional To a Fixed Limit

This is probably the most important rule for achieving scalability within an application. What it means:

*   Find the resource that has a fixed limit that you know you can support. For example, a guarantee to handle a certain number of objects in memory. So if we always use resources proportional to the number of objects it is likely we can prevent resource exhaustion.
*   Devise ways of tying what you need to do to the individual resources.

Some examples:

*   Keep a list of purchase orders with line items over $20 (or whatever). Do not keep a list of the line items because the number of items can be much larger than the number of purchase orders. You have kept the resource usage proportional to the number purchase orders and you know how many purchases orders you can support.
*   Merging, via aggregation, all operations for an object into one request instead of having a separate request for each operation. A create and update, for example could be one request.

### Merge Aggregation

In merge aggregation separate pieces of data and/or commands are merged into one piece. The idea is to Use Resources Proportional To Fixed Limit.

For example, let's say an object has the following command sequence:

*   create
*   update
*   update

These three separate requests can be merged into one request instead of 3\. If this cycle is repeated 100 times then we will still always have one request in the queue.

Another example is for attribute change events. Individual changes can be merged into one change event.

Think how powerful this is. Queues would never grow larger than the number of objects no matter how many events happened.

Your communication subsystem needs to not be stupid and it must cooperate with the application to make this happen. 

### Delete Aggregation

In delete aggregation data and/or requests are deleted when possible. For example, take the following operations:

*   create
*   delete 

This would cause the create and delete operations to be deleted. In a storm of reroutes where many creates and deletes are happening immense amounts of resources can be saved. 

### Batch Aggregation

From timing analysis batching together multiple pieces of data greatly improves performance. Sending operations one by one is much slower than sending all operations in larger batches.

Ideally applications should not be in the business of having to batch operations manually. A framework, for example, should do it for you.

### Change Aggregation

In Change Aggregation many changes are collapsed into one change. This is a different than Merge Aggregation. In change aggregation we are keeping the state that something changed so no matter how many times it has changed we only note that it has changed once.

We don't keep a record of each change. Instead, we can give the value to a client and tell them that is changed.

In a large system we won't be able to keep a history of everyting that has changed in the system.

### Integration Aggregation

In Integration Aggregation an event only exists if it has passed a period of time before being cancelled.

The most common example. An alarm is only set when an integration period has passed. Otherwise we can create alarm storms when hardware is defective or wave after wave of a condition hits.

### Load Shedding

Load Shedding is a scalability solution where work is rejected until enough resources become available to accept it.

For example, in a call processing system, the number of calls would be limited. Any calls beyond that limit would be rejected. This would give time for the current calls to be processed.

In some systems it is not possible to shed load because you are required to handle all requests. This puts a lot more emphasis on other facets of the scalability solution. Some examples of load shedding:

*   limit the number of ftp sessions a node can support.
*   a web server redirecting to another server when it is too busy
*   the public telephone system saying all calls are busy which prevents new calls from being accepted

There are many more strategies and we'll see more of them in later posts.

    