## [Beyond Threads and Callbacks - Application Architecture Pros and Cons](/blog/2013/3/18/beyond-threads-and-callbacks-application-architecture-pros-a.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, March 18, 2013 at 12:37PM</span></div>

<div class="body">

![](http://farm9.staticflickr.com/8251/8534554068_6f95503f2f_o.jpg)

There's not a lot  of talk about application architectures at the process level. You have your threads, pools of threads, and you have your callback models. That's about it. Languages/frameworks making a virtue out of simple models, like Go and Erlang, do so at the price of control. It's difficult to make a low latency well conditioned application when a power full tool, like work scheduling, is taken out of the hands of the programmer.

But that's not all there is my friend. We'll dive into different ways an application can be composed across threads of control.

Your favorite language may not give you access to all the capabilities we are going to talk about, but lately there has been a sort of revival in considering performance important, especially for controlling [latency variance](http://highscalability.com/blog/2012/6/18/google-on-latency-tolerant-systems-making-a-predictable-whol.html), so I think it's time to talk about these kind of issues. When it was do everything in the thread of a web server thread pool none of these issues really mattered. But now that developers are creating sophisticated networks of services, how you structure you application really does matter.

In the spirit of [Level Scalability Solutions - The Conditioning Collection](http://highscalability.com/blog/2013/3/11/low-level-scalability-solutions-the-conditioning-collection.html), we are going to talk about the pros and cons of some application architectures with an eye to making sure high priority works gets done with low enough latency, while dealing with deadlock and priority inheritance, while making the application developer's life as simple as possible.

## Forces to Balance

Here are some of the forces we are trying to harmonize when selecting an application architecture. 

<div id="_mcePaste">

*   **Scheduling High Priority Work** - If high priority work comes in while lower priority work has the CPU, how do we make sure the high priority work gets handled in a bounded period of time?
*   **Priority Inheritance** - is a way of getting out of priority inversion problems by assuring that a task which holds a resource will execute at the priority of the highest priority task blocked on that resource. In practice it means code you thought wasn't important can prevent other higher priority work from getting serviced.
*   **Dead Lock** - What you get when you use locks to protect data and you have a cycle in your dependencies. This happens when all these conditions are true: <span style="color: #000000; font-family: sans-serif; font-size: 13px; line-height: 19.049999237060547px;">Blocking shared resources -</span> a shared resource is protected by some mechanism, typically a lock; No pre-emption - a process or thread cannot force another process or thread to drop a shared resource it is holding; Acquiring while holding - resources are acquired while holding locks; Circular wait - A cycle (schedule) exists such that the above three conditions all hold for a set of processes or threads accessing a shared resource.
*   **OS Latency -** The gap between when a time-sensitive task needs to run and when the OS actually schedules it. Some factors: Timer Resolution, Scheduling Jitter, Non-<span><span>Preemptable</span></span> Portions. 
*   **Shared Locks** - When there's a shared lock anywhere we are back to potentially poor scheduling because the high priority task must wait on the low priority task. This requires anyone who shares the lock to know there is high priority work and never to take more time than the worst case performance needed for the high priority work. Of course, this changes dynamically as code changes and with different load scenarios.
*   **Ease of Programming Model** - Programmers shouldn't be burdened with complicated hard to program schemes. Which I realize may invalidate everything we are talking about.
*   **Robustness of Programming Model** - It should be hard for programmers to break the system through dead locks, poor performance, and other problems.
*   **Minimizing Lock Scope** - Locks over operations that take an unexpectedly long time can really kill the performance of an entire system as many threads will be blocked or not get enough CPU. They'll stop making progress on their work, which is not what we want. 
*   **Flow Control** - The general process of keeping message chatter to a minimum through back pressure on senders, removing retries, and other techniques.
*   **Reasonable Use of Memory** - Memory management is a hidden killer in almost every long lived program. Over time memory becomes fragmented with lots of allocations and <span><span>deallocations</span></span> and everything becomes slower because of it. Garbage collection doesn't save you here. Message serialization/<span><span>deserialization</span></span> is often a big culprit. And calls to memory allocation will drop you into shared locks, which is a killer when components are cycling through a lot of memory.
*   **Reasonable Use of Tasking** - Lots of threads, depending on the scheduler OS, can cause high scheduling overhead as naive scheduling algorithms don't perform well as the number of threads increase. Lots threads can also mean a lot of memory usage because each thread has allocated to it the maximum stack space. This is where [languages like Go](http://stackoverflow.com/questions/4226964/how-come-go-doesnt-have-stackoverflows) have a big lead over their C based cousins as it can minimize stack space while keeping many threads of control.
*   **Time Slice** - The period of time for which a process is allowed to run uninterrupted in a pre-emptive multitasking operating system. Generally you want your program to use it's entire time slice and not do anything that gives up control of the CPU while you have it.
*   **Starvation** -Priority is a harsh mistress. At first blush it's easy to think priority solves all your problems, but like money, mo' priority, more problems. This biggest problem is the surprise you feel when you see work in your system not getting done. You suffer from dead lock or priority inheritance or large lock scope, but after bugs are accounted for, it may simply be that you have higher priority tasks that are using up all the CPU time, so even medium priority tasks in your system end up not getting serviced. Higher priority tasks may have endless streams of work to do because of poor flow control and other methods of poor messaging hygiene, so you use techniques like batching to reduce CPU usage, but sometimes there's just a lot to do. Lower priority tasks need some CPU time so their state machines can make progress. Otherwise cascading failures can start as timeouts pile up. Another problem can be fault windows widen as it takes longer to process requests. 

</div>

So you look at all these issues and think, well, there's a good reason not to expose all this complexity to programmers, and that's true. Playing it safe can be a good thing, but if you want to make full use of all your system resources, playing it safe won't work. 

## On Threading

Threads are somewhat of a religious issue. A lot of people dislike threads because multithreaded programming is difficult. Which is true.

So why do we use threads at all? 

*   **latency**. In latency sensitive systems some work must be performed with very low latency. In a run to completion approach where code runs as long as it wants, latency can not be guaranteed. Yield, like hope, is not a plan.
*   **priority**. At a system level we want more important work to be performed over lower priority work. Threads provide this ability because they can be preempted.
*   **fairness**. Preemption allows multiple threads to make progress on work at the same time. One thread doesn't starve everyone at the same priority.
*   **policy domain**. A thread can be a domain for policies that you may or may not want applied to different applications. Examples are: priority, floating point safety, stack space, thread local storage, blocking, memory allocation. 

The key to safe thread usage is not using shared data structures protected by locks. It is lock protected shared data structures that cause latency issues, deadlock, and priority inheritance. It is nearly impossible for a human to make a correct system when any thread can access shared state at any time. 

Threading can also be bad when thread scheduling is slow. On real-time systems and modern Linux systems, threads are usually very efficiently scheduled, so it is not a huge concern.  Where 10,000 threads would have been unthinkable in the past, it's not a bad approach anymore.

## Are Locks OK?

If locking is so dangerous, should locks be used at all? An application that uses multiple threads and has a lock usable just within the application may not, if care is taken, suffer from the same problems as when locks are arbitrarily used from different application threads.

A single application can:

*   guarantee the amount of time spent blocked on the lock
*   prevent deadlock because it probably won't do work in different threads
*   prevent priority inheritance

When different components share a process space nothing can can be guaranteed. Even bringing in code from other groups can ruin everything. A process has many hidden variables that different code components communicate through indirectly, even if they don't have and obvious direct dependencies. Think any shared resource like memory management, timers, socket buffers, kernel locks, and available CPU time at different priorities. It gets real complex real fast.

## <span><span>Evented</span></span> Model

This is "good enough" concurrency. Common examples are node.<span><span>js</span></span> and your browser. In an <span><span>Evented</span></span> architecture applications share a single threaded container where work runs to completion. Thus it's a cooperative tasking model, which makes for an asynchronous application architecture. Your application is essentially a collection of functions that get called when events happen. Locks and shared state aren't a problem as your code can't be preempted. It's a simple approach and can perform very well if applications are well behaved, especially if as in most web applications they spend a lot of time blocking on IO.

Erlang, for example, supports a preemptive model where work in one process can be suspended and another process can start working. Internally Erlang uses <span><span>async</span></span> IO and its calls are non-blocking so the Erlang kernel doesn't interfere with process scheduling at the application level.

Go is like Erlang in that it has lightweight processes, but it's like node.<span><span>js</span></span> in that preemptive scheduling is not supported. A <span><span>goroutine</span></span> runs to completion, so it never has to give up the CPU and if on a single CPU it's expected application code will insert yields to give the scheduler a chance to run. 

A downside of the <span><span>evented</span></span> model, and even Erlang, is there's no concept of priority. When a high priority input comes in there's no way to bound the response latency. 

Without preemption, CPU heavy workloads, as is common in a service architecture, can starve other work from making progress. This increases latency variance which is how [long tails](http://highscalability.com/blog/2012/6/18/google-on-latency-tolerant-systems-making-a-predictable-whol.html) are created at the architecture level. Bounding latency variance requires a more sophisticated application architecture.

## Thread Model 

In this model applications always operate out of  others threads. For example, a web request is served out of a thread of a web server thread pool. Your application state is shared amongst all these threads and there's no predictable order of when threads run and what work they are doing. Locks are a must and shared state is the rule. Usually all threads run at the same priority.

Dead lock in this model is nearly impossible to prevent as application complexity increases. You can never be sure what code will access what locks and what threads are involved in these locks.

Latency can't be guaranteed because you don't know what operations are being performed in your execution path. There's no preemption. Priority inheritance becomes a big problem. Simply cranking up the number of threads for better performance can lead to very unpredictable behavior.

## Actor Model  (1 - 1)

In the Actor model all work for an application is executed in a single thread context. One application maps to one thread, thus it is 1-1\. Each Actor has a queue and blocks on a semaphore waiting for messages. When a message is posted on the queue this causes the Actor unblock and be scheduled to run at its priority level. The Actor then processes messages off the queue until it runs out of messages, gives up the processor by blocking, or is preempted.

There's no shared state in this model so it eliminates locks which eliminates priority inheritance and deadlock issues. But we still have the latency for high priority work issue. As an Actor is processing a message it can't be interrupted if a higher priority message arrives. 

We can expect an Actor to be asynchronous so it doesn't block the next message from processing for long.

Or we can introduce the idea of cooperation by having a priority associated with each message. If a higher priority message comes in on the queue then a thread variable "<span><span>GiveUpWork</span></span>" is set. Code in a sensitive a place would check this flag a particular points in its code and stop what it is doing so the higher priority work can be scheduled. An implication is to change the Actor's priority accordingly with the highest priority work in its queue. This is another form cooperative multitasking. And its ugly. So we usually don't do it this way.

## Actor With Thread Pool (1-N)

Like the Actor model but requests are served by a thread pool instead of just one thread. Useful in cases where work items are independent of each other. Locks are required if there's any shared state, but as the lock is completely under control  of the application the chances for deadlock are contained.

## Actor Arbitrary Thread Model

A hybrid is where an application has an Actor, so it has a thread, but other threads access state in the Actor and even <span><span>cals</span></span> Actor methods directly. Shared locks are necessary to protect data. It's easy in practice for this architecture to evolve because not everyone is disciplined about using just message passing between Actors.

This is in some ways the worse of all possible worlds. The Actor model is supposed to be a safe programming model, but instead it degenerates into a thread and lock nightmare. 

## Multiple Actor Model

Work is routed to multiple Actors. Parallelism is added by adding an Actor per some sort of category of work. For example, low priority work can go to one Actor. High priority work can go to another. With this approach work happens at the expected priority and another queue can preempt a lower priority queue. There's no attempt at fairness however and a work item in each queue must cooperate and give up the processor as fast as possible.

A lock is still necessary because an application shares state across Actors, but the lock is controlled by the application and is totally responsible for locking behaviour. 

The disadvantage is this architecture is it more complex for applications to create. Message order issues also arise as parallel work is spread across multiple Actors. 

## Parallel State Machine Model

In this model work is represented by events. Events are queued up to one or more threads. Applications don't have their own thread. All application work is queued up in event threads. Applications are usually asynchronous. The events know what application behavior to execute. In the Actor model an Actor knows what behavior to execute based on the message type and the current state. 

Deadlock and locking issues still apply. One thread is safer, but then we have priority and latency issues.

This model is attractive when most of the work is of approximately equal and small size. This model has been used very well in state machine oriented computation (communicating finite state machines) where state transitions require deterministic and small times. Breaking down your system to make it suitable for this model is a big job. <span><span>Paybacks</span></span> are reduced complexity of implementation. Event priorities can be used in this model. This model is usually based on a run to completion kind of scheduling.

If latency is an important issue then this model can't be used, at least everywhere as you'll never be able to guarantee latency. Attempts to guarantee latency basically require writing another level of scheduling.

## Actor + Parallel State Machine Model

If synchronous logic is used then an Actor can not process high priority messages when a low priority message is being processed.

One alternative is to use a state machine model in the processing of a request. A request is sent and the reply is expected on a specific message queue. Now the Actor begins the processing of the high priority message. Next time it picks up a message it can look for the reply message and resume processing of the low priority message. Having a separate message queue for replies simply makes it easy to find replies.

In short, having multiple message queues for different priority messages combined with state machine driven message processing when required can potentially give, in many situations, low latency processing of high priority messages.

For example, msg1 comes in at a high priority, then msg2 comes in at medium priority, and then comes msg3 at the low priory. The assignment of these priorities to different requests is part of application protocol design. The heuristics used by an application could be as follows. Process msg1 first. If more than 90 ms is spent processing msg1 and msg2 is starving, process one msg2 for 10 ms and then go back to msg1 processing.

If a msg3 is pending and it has not been processed for the last 1 minute, process msg3\. Note that in this example each message is processed to completion. The heuristic kicks in to avoid starvation when large number of requests comes in.

The state machine is parallel in terms of the number of requests it can take. For each request processed you create a context for the request and the context is driven by the state machine. You might, for example, get a flood of msg1s. For each msg1 being processed, there will be a context block for storing an id (for context identification purpose), current state, and various associated data. As hundreds of msg1s are processed, hundreds of these contexts are created. One of the state these contexts go through is communication to another node. There is a timer associated with each context in case reply times out. As replies come in or a timer fires, the associated context makes its state transition.

Many applications can be implemented in a straight forward fashion in this model to achieve high throughput and low latency for high priority work.

## <span><span>Dataflow</span></span> Model

A technique used in functional languages. It allows users to dynamically create any number of sequential threads. The threads are <span><span>dataflow</span></span> threads in the sense that a thread executing an operation will suspend until all operands needed have a well-defined value.

## Erlang Model

[Erlang](http://www.erlang.org/) has a process-based model of concurrency with asynchronous message passing. The processes are light-weight, i.e require little memory; creating and deleting processes and message passing require little computational effort.

No shared memory, all interaction between processes is by asynchronous message passing. The distribution is location transparent. The program does not have to consider whether the recipient of a message is a local process or located on a remote Erlang virtual machine.

The deficiency of <span><span>Erlang's</span></span> approach is that there's no way to control latency through priority. If certain events need to be serviced before others or if certain events need to be serviced quickly, Erlang doesn't give you the necessary control needed to shape message processing.

## <span><span>SEDA</span></span> Model

<div>

<div>[<span><span>SEDA</span></span>](http://www.eecs.harvard.edu/~mdw/proj/seda/) is an acronym for staged event-driven architecture, and decomposes a complex, event-driven application into a set of stages connected by queues. Each queue has a thread pool to execute work from the queue.</div>

<div>This design avoids the high overhead associated with thread-based concurrency models, and decouples event and thread scheduling from application logic. By performing admission control on each event queue, the service can be well-conditioned to load, preventing resources from being <span><span>overcommitted</span></span> when demand exceeds service capacity. <span><span>SEDA</span></span> employs dynamic control to automatically tune runtime parameters (such as the scheduling parameters of each stage), as well as to manage load, for example, by performing adaptive load shedding. Decomposing services into a set of stages also enables modularity and code reuse, as well as the development of debugging tools for complex event-driven applications.</div>

</div>

## Multiple Application Thread Pool Model (M - N)

In this model multiple applications are multiplexed over a common thread pool and a scheduling system on top of the thread scheduling system is imposed. The pool may have only one thread or it could have many. This could be considered a container for a parallel state machine model. Execution times for individual work items are tracked so work can be scheduled at a low level of granularity. Priority and fairness are built into the scheduler and follow application specific rules.

The basic idea is to represent an application so that behaviour is separated from threading. Then one or more applications can then be mapped to a single thread or a thread pool. If you want an event architecture then all applications can share a single thread. If you want N applications to share one thread then you can instantiate N applications, install them, and they should all multiplex properly over the single thread. If you want N applications to share M threads then that would work as well. Locking is application specific. An application has to know its architecture and lock appropriately.

## Where is the state machine?

Every application can be described by an explicit or implicit [state machine](http://en.wikipedia.org/wiki/Finite-state_machine). In the Actor model the state machine is often implicit on the stack and in the state of the Actor. Clearly an Actor can use a state machine, but it is often easier for developers to not use a formal state machine.

In Parallel State Machine model state machines are usually made explicit because responses are asynchronous which means a response needs to be matched up with the original request. A state machine is a natural way to do this. And applications built around state machines tend to be more robust.

These are some of the issues to keep in mind when figuring out how to structure work flows within an application. All of the options can be used depending on the situation. It's not easy at all, but if you create the right architecture for your application you can keep your application both responsive and busy, which is a pretty good result.

## Related Articles

*   [The Pillars of Concurrency](http://www.drdobbs.com/parallel/the-pillars-of-concurrency/200001985) by Herb Sutter
*   [Why Events Are A Bad Idea](http://www.stanford.edu/class/cs240/readings/vonbehren.pdf) by Rob von <span><span>Behren</span></span>, Jeremy <span><span>Condit</span></span> and Eric Brewer
*   [A Note on Distributed Computing](http://labs.oracle.com/techrep/1994/smli_tr-94-29.pdf) by Jim Waldo, Geoff <span><span>Wyant</span></span>, Ann <span><span>Wollrath</span></span>, Sam Kendall
*   [How to use priority inheritance](http://www.embedded.com/design/configurable-systems/4024970/How-to-use-priority-inheritance) by Kyle <span><span>Renwick</span></span> and Bill <span><span>Renwick</span></span>
*   [Mission Planning and Execution Within the Mission Data System](http://www-aig.jpl.nasa.gov/public/planning/papers/barrett_iwpss2004_missionplanning.pdf)
*   [<span><span>CLEaR</span></span>: Closed Loop Execution and Recovery High-Level Onboard Autonomy for Rover Operations](http://www-aig.jpl.nasa.gov/public/planning/papers/fy01_clear_demo_handout-short_version.pdf)
*   [42 Monster Problems That Attack As Loads Increase](http://highscalability.com/blog/2013/2/27/42-monster-problems-that-attack-as-loads-increase.html)
*   [Low Level Scalability Solutions - The Aggregation Collection](http://highscalability.com/blog/2013/3/6/low-level-scalability-solutions-the-aggregation-collection.html)
*   [Low Level Scalability Solutions - The Conditioning Collection](http://highscalability.com/blog/2013/3/11/low-level-scalability-solutions-the-conditioning-collection.html) 

</div>