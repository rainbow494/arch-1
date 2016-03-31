## [Low Level Scalability Solutions - The Aggregation Collection](/blog/2013/3/6/low-level-scalability-solutions-the-aggregation-collection.html)

    

    

![](http://farm9.staticflickr.com/8251/8534554068_6f95503f2f_o.jpg)

What good are problems without solutions? In [42 Monster Problems That Attack As Loads Increase](http://highscalability.com/blog/2013/2/27/42-monster-problems-that-attack-as-loads-increase.html) we talked about problems. In this first post (OK, there was an earlier post, but I'm doing some reorganizing), we'll cover what I call _aggregation_ strategies.

Keep in mind these are low level architecture type suggestions of how to structure the components of your code and how they interact. We're not talking about massive scale-out clusters here, but of what your applications might like like internally, way below the service level interface level. There's a lot more to the world than evented architectures.

Aggregation simply means we aren't using stupid queues. Our queues will be smart. We are deeply aware of queues as containers of work that eventually dictate how the entire system performs. As work containers we know intimately what requests and data sit in our queues and we can use that intelligence to our great advantage.

## Prioritize Work

The key idea to it all is an almost mindful approach to design that has programmers consider as a first class concept the priority of what works gets done, why it gets done, and when it gets done, in every aspect of their creation.

### **Preventing Cascading Failures**

The most treacherous example of why prioritizing work is important is in** preventing cascading failures**. Naive systems, without an idea of priority, will let useless control or data plane traffic elbow out essential control traffic when failures occur.

If you need to send a request to a switch to reprogram a route to failover traffic and there's head-of-line blocking on a low priority work item, then you'll never be able to get control of your system without shutting the whole thing down. Chatty programs that don't know what's going on will pump out endless low priority control and data traffic that keeps your system busy doing nothing useful at all. It's like junk food for code.

A priority aware system will try to prevent useless work being done while making sure high priority work gets done when it needs to get done. You'll have a separate network for control and data so control messages always go through. You'll have intelligent retry policies so useless work doesn't pile up in queues. Obsolete messages will be delete from queues. You'll look at buffering throughout your network to make sure you aren't seeing an old version of the world. When the newest save the day control message is available current work is stopped so the higher priority item can be processed and it's messages will go out immediately as apposed to sitting back and waiting. Lots of cool stuff can be done to robustify systems.

### Handling Infinite Workloads

Priority is a key idea in Handling Infinite Workloads, which was discussed in [42 Monster Problems That Attack As Loads Increase](http://highscalability.com/blog/2013/2/27/42-monster-problems-that-attack-as-loads-increase.html). Traditionally we have considered 100% CPU usage a bad sign. As compensation we create complicated infrastructures to load balance work, replicate state, and cluster machines. CPUs don't get tired so we can use the CPU as much as possible. When we talk about using priority we are also talking about how a system can react smartly and predictably under fully loaded conditions. That's not a problem if the proper conditioning is in place, it's only a problem for naively architected software. 

### Conscious Control

Prioritizing Work says developers should have conscious control over:

*   what work is processed or dropped
*   the order in which work is handled
*   the task priority at which work is processed
*   the amount of resources given to work:
    *   CPU time
    *   memory
    *   queue space
    *   disk
    *   network
    *   locking

You may note that your typical programming environment gives you very little control over most of these. That must change.

### Priority Arises

Prioritizing Work is a common theme across many of the other scalability solutions. Prioritizing takes a deep understanding of what is happening in your system and what you want have happen in your system. Priority may depend on:

*   the customer
*   the type of request
*   trying to provide fair service
*   the number of requests
*   the amount of resources already used
*   the amount of resources needed
*   contractual deadlines

### SLAs

In addition to the prevention of cascading failures, another key reason to embrace priority as a primary concept is to ensure SLAs.

You have a high priority client that has paid for better service than the rabble. For them to achieve their SLA all the components of your system must be conditioned to understand priority while still not starving out others and giving good service.

There's a lot of commonality here to an OS scheduling tasks or a network scheduling traffic. It's the same idea, but developers need to apply the ideas system wide.

## Merge Aggregation

In merge aggregation separate pieces of data and/or commands are merged into one piece. The idea is to _Use Resources Proportional To Fixed Limit_.

For example, let's say an object has the following command sequence:

*   create
*   update
*   update

These three separate requests can be merged into one request instead of three. If this cycle is repeated 100 times then we will still always have one request in the queue.

Another example is for attribute change events. Individual changes can be merged into one change event.

Think how powerful this is. Queues would never grow larger than the number of objects no matter how many events happened.

Your communication subsystem needs to cooperate with the applications to make this happen. 

## Delete Aggregation

In delete aggregation work is deleted whenever possible. For example, take the following operations:

*   create
*   delete 

This would cause the create and delete operations to be deleted before messages were ever sent out. In a storm of reroutes or flapping inputs where many creates and deletes are happening, an immense amounts of resources can be saved. 

## Batch Aggregation

From timing analysis batching together multiple pieces of data greatly improves performance. Better the one than the many. Sending operations one by one is much slower than sending all operations in larger batches.

Ideally applications should not be in the business of having to batch operations manually. A framework, for example, should do it for you.

## Change Aggregation

In this strategy many changes are collapsed into one change. This is a different than Merge Aggregation. In change aggregation we are keeping the state that something changed so no matter how many times it has changed we only note that it has changed once.

We don't keep a record of each change. Instead, we can give the value to a client and tell them that is changed.

In a large system we won't be able to keep a history of everyting that has changed in the system.

## Integration Aggregation

In this strategy an event only exists if it has passed a period of time before being cancelled.

The most common example is an alarm, which is only truly set when an integration period has passed. Otherwise we can create alarm storms when hardware is defective or wave after wave of a condition hits.

Let events sit around for awhile before telling the rest of the system something has happened. A lot of useless work can be prevented.

## Signal Integration

We'll talk more about this later, but the idea is to consider that your system has it's own intelligence network spying on everything your system does. All those well placed assets report back their intelligence to HQ so coordinated action can be taken to respond to any developments.

This intelligence system produces a constant stream of signals that can be used to control how other parts of the system behave. Aggregation policies are very sensitive to what is happening out in the world. So you need to setup your own intelligence operation, conduct your own ops, even do your own counter intelligence. All sensors lie. 

    