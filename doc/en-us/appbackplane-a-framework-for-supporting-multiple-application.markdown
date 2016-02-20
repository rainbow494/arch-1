## [AppBackplane - A Framework for Supporting Multiple Application Architectures](/blog/2013/3/25/appbackplane-a-framework-for-supporting-multiple-application.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, March 25, 2013 at 10:36AM</span></div>

<div class="body">

![](http://upload.wikimedia.org/wikipedia/commons/thumb/c/c1/PICMG-Backplane-Details.jpg/300px-PICMG-Backplane-Details.jpg)

Hidden in every computer is a hardware backplane for moving signals around. Hidden in every application are ways of moving messages around and giving code CPU time to process them. Unhiding those capabilities and making them first class facilities for the programmer to control is the idea behind AppBackplane.

This goes directly against the trend of hiding everything from the programmer and doing it all automagically. Which is great, until it doesn't work. Then it sucks. And the approach of giving the programmer all the power also sucks, until it's tuned to work together and performance is incredible even under increasing loads. Then it's great.

These are two different curves going in opposite directions. You need to decide for your application which curve you need to be on.

AppBackplane is an example framework supporting the multiple application architectures we talked about in [Beyond Threads And Callbacks](http://highscalability.com/blog/2013/3/18/beyond-threads-and-callbacks-application-architecture-pros-a.html). It provides a scheduling system that supports continuous and high loads, meets critical path timing requirements, supports fair scheduling amongst priorities; is relatively easy to program; and supports higher degrees of parallelism than can be supported with a pure tasking model.

It's a bit much for simple applications. But if you are looking to go beyond a basic thread per request model and think of an application as a container where a diverse set of components must somehow all share limited resources to accomplish work, then some of the ideas may prove useful.

In case you are still wondering where the name AppBackplane comes from, it's something I made up a while ago as a takeoff of [computer backplane](http://en.wikipedia.org/wiki/Backplane):

> A group of electrical connectors in parallel with each other, so that each pin of each connector is linked to the same relative pin of all the other connectors forming a computer bus. It is used as a backbone to connect several printed circuit boards together to make up a complete computer system.

## Frameworks Encourage Poor Threading Models

Frameworks often force an application architecture. Your application architecture shouldn't be determined by a servlet or a database or anything but the needs of your application.

Sure, a single threaded approach may work fine for a stateless web back end. But what if you are doing a real application on the backend like handling air traffic control or a manufacturing process? Or what if creating a reply to a REST request requires integrating the results of dozens of different requests, each with their own state machine?

In these cases a single threaded approach makes no sense because a web page is just one of a thousand different events an application will be handling. All events are not created equal. Threads, queues, priorities, CPU limits, batching, etc are all tools you can use to handle it all.

It has a lot to do with viewing your application performance as a whole, instead of a vertical slice in time. With a multi threaded approach you can create the idea of quality of service. You can have certain work done at higher priorities. You can aggregate work together even though it came in at different times. You can green light high priority traffic. You can reschedule lower priority traffic. You can drop duplicate work. You can limit the CPU usage for work items so you don't starve other work. You can do lots of things, none of which you can do with single task that runs until completion.

Programmers commonly talk about upping the number of threads in a thread pool or tuning garbage collection or tuning memory limits as their primary scalability tactics at the application level, but they don't talk about priorities, deadlock, queuing, latencies, and a lot of other issues to consider when structuring applications.

For example, on an incoming request from a browser you want to setup the TCP/IP connection immediately so that the browser doesn't have to retry. <span>Retries add load and make the user experience horrible. Instead, </span><span>what you would like to do is set up the connection, then queue up the request </span><span>and satisfy it later after having immediately setup the connection. But if </span><span>each request is handled by a single thread then you can't implement </span><span>this sort of architecture and your responsiveness will appear </span><span>horrible as you run out of threads or threads run slow, or threads block other threads </span><span>on locks, when in fact you have tons of CPU resources available.</span>  

<span>Based on the source of the request you could assign the work a specific </span><span>priority or drop it immediately.</span>  

<span>You can also do things like assign priorities to different phases of a process. If </span><span>one phase hits the disk you know that takes a lot of time relative </span><span>to other in memory operations. As your application scales you </span><span>decide if that phase of the work is more important and give it </span><span>a higher priority or you can possible drop work elsewhere because </span><span>you don't want to a request to fail once it has reached a certain </span><span>processing point.</span>  

<span>Consider if an application must process a UI request, servlet work, and </span><span>database work. The work could be organized by priority. Maybe you want </span><span>to handle the UI work first so it queues ahead. But if you keep getting</span>  
<span>UI work it will starve the other clients so you give other clients a </span><span>chance to process work. </span>

<span>With your typical application architecture you don't have much control over any of this. That's what we'll address next, giving you control.</span>

## AppBackplane

As a note, in this post Hermes refers to a general messaging infrastructure that implements subscribe and publish functionality and remote method invocation. The target environment is C++ so the major compositional structure is a framework based on interfaces and virtual functions. Other languages would no doubt express the same ideas in their own way.

AppBackplane is an application framework that:

*   Supports multiple application architectures. Applications can map to one or a pool of threads, they can be event oriented, or they can be synchronous, or they can use a SEDA pipeline type model. Multiple applications can multiplex over the same queues and threads. Applications can have multiple pieces of work outstanding at the same time.
*   Supports large scale parallelism so peer-to-peer protocols can be used without radical increases in thread counts.
*   Supports static registration of routes and other application services. One goal is for all communication routes to be available after application modules are are constructed. In a complicated system software components, or modules, can't just come up in any order because modules are interdependent. The idea is to create a dependency graph of modules and drive them through a state machine until they reach the state appropriate for the role the application is assigned (start, initializing, primary, secondary, down). If one module depends on another module then it will only be told to move to another state when the modules it is dependent on have reached a desired state. This way all components in a system can come up in harmony. This model can be extended across clusters as well.
*   Uniformly supports Hermes, peer-to-peer, timer, and other frameworks. If you specify a one shot or repeated pattern of timer expirations, for example, these events should be treated no differently than any other event your application consumes. Timer events should always be handled in the appropriate thread context and never the thread of the timer service. And so it should be with every service/framework.
*   Provide a scheduling system that:
    *   Supports continuous and high loads.
    *   Performs work in priority order.
    *   Meets critical path deadlines.
    *   Does not cause watch dog timeouts.
    *   Throttles by CPU usage. Once a certain amount of CPU is used the application will give up the processor.
    *   Has a configurable number priority queues.
    *   Has a quota of operations per queue.
*   Enable request throttling total number of requests per application.
*   Ensures operations on an object happen in order.
*   Has work for an object be performed at the highest priority of outstanding requests.
*   Ensures work on different objects can happen at the same time.
*   Ensures one work request for an object is completed before another work request.
*   Support objects modeled as state machines.
*   Data structures are simple so they can be in the critical path.
*   Keep various statistics on operations.

We want to make it easy for applications to select an architecture . It should be relatively simple to switch between 1-1, 1-N, M-N, event, and parallel state machine architectures. 

## Conceptual Model

In general the AppBackplane approach combines the event scheduling with parallel state machines using a thread pool (1 or more threads).

*   An application derives from an AppComponent.
*   AppComponents plug into and share an AppBackplane. This means one or more applications can transparently work over the same AppBackplane. This is how multiple architectures are supported.
*   Work is generally asynchronous so that multiple AppComponents can multiplex over the same AppBackplane. It is not required that work be performed asynchronously, but if it is not then it is impossible to guarantee latency. In the future it would be great of applications could have more control over pre-emptive scheduling without having to reply on the OS.
*   AppBackplane has a configurable number of priority queues, called ready queues, for scheduling objects with work to do. Applications must agree on the meanings of the priorities.
*   Each ready queue has a quota of the number of items that can be serviced at a time before the next priority queue must be serviced.
*   AppBackplane keeps a wait queue for an object waiting for a reply. Once the reply comes in the object is moved to the ready queue.
*   AppBackplane contains one or more threads blocked on a semaphore waiting for work to do. Threads can be added or deleted.
*   AppBackplane is the event scheduler.
*   Each AppObject maintains a queue of work it must perform. Operations are queued to and serviced by objects. This is how we make sure operations on an object are performed in order.
*   An application can work on more than one request simultaneously without creating more threads.
*   All operations are in the form of Actions. This allows operations from different sources to be treated generically.
*   Work is serviced in priority order. It is the object that has the priority.
*   Work for an object is processed in order.
*   Hermes and other infrastructure components are abstracted in such a way that work can be converted to Actions and that registration specifications are complete at construction.
*   Hermes registration and deregistration will happen automatically, similarly for other services.
*   AppComponent and AppBackplane are modules so they can be used in the system state machine that is in charge of bringing up the entire system in dependency order.
*   AppBackplane is a AppComponent so it can have its own operations.
*   AppBackplane is a policy domain. All AppComponents sharing an AppBackplane must agree to abide by the same rules regarding latency, etc.
*   AppBackplane can be a system module and move all its contained components through the system state machine.
*   AppBackplane can load balance between available objects implementing the same operations.
*   Each event is timed to gauge how long it took. This data are used to calculate the percentage of CPU used per second which is used to limit the amount of CPU used by the backplane in a second.
*   Events are mapped to objects which are queued to ready queues in the thread of the caller. If an event can not be dispatched quickly, by message type or other data in the message, then event should be queued to a '''digester''' object at a high priority so the event can be correctly prioritized based on more complicated calculations.
*   The scheduling algorithm supports multiple priorities so high priority work gets process first, quotas at each priority for fairness, and CPU limitation.
*   A warning alert is issued if any event takes longer than a specified threshold to process. This can be picked up in the logs or a dashboard so programmers can take a look as to why an operation was taking longer than expected.
*   Work is scheduled by the backplane so we can keep a lot of statistics on operations.
*   Many parallel activities can happen at the same time without a correspondingly large increase in thread counts.
*   When the AppObject is in the wait queue waiting for reply, more requests can be queued to that AppObject.

The basic idea is to represent an application by a class that separates application behaviour from threading. Then one or more applications can then be mapped to a single thread or a thread pool.

If you want an event architecture then all applications can share a single thread.

If you want N applications to share one thread then you can instantiate N applications, install them into an AppBackplane, and they should all multiplex properly over the single thread.

If you want N applications to share M threads then you can create an AppBackplane for that as well.

Locking is application specific. An application has to know its architecture and lock appropriately.

## Increased Parallelism Without Large Thread Count Increases

One of the key motivations behind AppBackplane is to be able to support higher degrees of parallelism without radical increases in thread counts. This allows us to do is have more peer-to-peer protocols without worrying about how they impact threading.

For example, if multiple applications on a node want to talk with 30 other nodes using a peer-to-peer approach, then 30 threads per application will be used. This adds up to a lot of threads in the end.

Using the backplane all the protocols can be managed with far fewer threads which means we can expand the use of peer-to-peer protocols.

## Bounding Process Times

If an application is using an event or parallel state machine model it is responsible for creating bounded processing times.

This means that whoever is doing work must give up the CPU so as to satisfy the latency requirements of the backplane. If, for example, an application expects a max scheduling latency of 5ms then you should not do a synchronous download of 1000 records from the database.

## How Modules are Supported

A Module is a way of looking at a slice of software as a unit that can have dependencies on other modules and that the modules must be brought up in some sort of dependency order. The state machine and complexity of the dependencies depends on your application. 

Whatever your approach, the backplane needs to work within the module infrastructure. Applications need to be dependent on each other yet still work over the same backplane.

We need AppComponents installed into the backplane to be driven by the backplane in accordance with the same state machine that brings up the entire application.

## Dispatching Work to Ready Queues

Requests like Hermes messages come in from a thread different than the thread in the backplane that will execute the command. A dispatch step is required to dispatch the work to an object and then to a ready queue. Dispatch happens in the thread of the caller.

Dispatching in the caller's thread saves an intermediate queue and context switch.

If the dispatch can happen quickly then all is right with the world. Dispatching can be fast if it is based on the message type or data in the request.

Sometimes dispatching is a much more complicated process which could block the caller too long.

The strategy for getting around blocking the client is to queue the operation to a high priority '''digester'' object.

The digester object runs in the backplane and can take its time figuring out how to dispatch the work.

## Objects Considered State Machines

Objects may or may not be implement as FSMs. An object's operations may all be stateless in which case it doesn't care. Using a state machine model makes it easier to deal with asynchronous logic.

## Parallelism

Parallelism is by object and is made possible by asynchronous operation. This means you can have as many parallel operations as there are objects (subject to flow control limits). The idea is:

*   Operations are queued to objects.
*   Any blocking operation is implemented asynchronously allowing other operations to be performed. Latency is dependent on cooperative multitasking.
*   The reply to an operation drives the state machine for the object forward.

An application can have N objects capable of performing the same operation. The AppBackplane can load balance between available objects.

Or if an operation is stateless then one object can handle requests and replies in any order so the object does not need to be multiplied.

It may still be necessary to be able to interrupt low priority work with a flag when high priority work comes in. It is not easy to decompose iterators to an async model. It may be easier to interrupt a loop in the middle.

## Per Object Work Queue

Without a per object queue we won't be able to guarantee that work performed on an object is performed in order.

The Actor queue fails because eventually you get to requests in the queue for an object that is already working on a request. Where do the requests go then?

In an async system a reply is just another message that needs to be fed to the FSM. There are also other requests for the same object in the queue. If the reply is just appended to the queue then that probably screws up the state machine because it's not expecting a new request. So the reply must be handled so that it is delivered before any new requests. This isn't that obvious how this should be done.

## All Operations are Actions

The universal model for operations becomes the Action. The AppBackplane framework derives a base class called AppAction from Action for use in the framework.

All operations must be converted to AppActions by some means.

The reasoning is:

*   We need a generic way to queue operations and for the work scheduler to invoke operations.
*   Through the use of a base class an Action can represent any operation because all the details can be stuck in the derived class.
*   Actions can be put in a memory pool so they don't have to be dynamically allocated.

Requests are converted to actions in the thread queueing the request. The actions are added to the object that is supposed to handle them. The Action/conversion approach means that the type of a message only has to be recovered once. From then on out the correct type is being used. Parallelism can be implemented by the application having more than one object for a particular service.

## Ready and Wait Queues

The ready and wait queues take the place of the single Actor queue. AppBackplane is the event scheduler in that it decides what work to next. AppComponents are multiplexed over the AppBackplane.

*   An object cannot be deleted if it is the wait queue.
*   Aggregation is not performed in the queues. The correct place for aggregation is in the senders. Most every request expects a reply, so aggregation usually won't work.
*   The total number of requests is counted so that we can maintain a limit.
*   The number of concurrent requests is limited.

## Handling Replies

An object can have only one outstanding reply so we don't need dynamic memory to keep track of an arbitrary amount of outstanding requests.

If an object is expecting reply the object is put on the wait queue until the reply comes in. The reply is queued first in the object's work queue. The object is then scheduled like any other object with work to do.

## Client Constraints

Clients can not send multiple simultaneous requests and expect any order. Order is maintained by a client sending one request to a service at a time and not duplicating requests.

## Steps to Using AppBackplane

*   Create a backplane to host components that agree to share the same policies. Policies include:
    *   Number of priorities.
    *   Quotas per priority.
    *   CPU limit.
    *   Latency policies. What is the longest an application should process work?
    *   Lock policies between components.
    *   Number of threads in the thread pool. This addresses lock policies.
*   For each application create a class derived from AppComponent and install it into the backplane.
*   Determine how your application can decomposed asynchronously.
*   Install the backplane and/or components in the system modules if necessary.
*   In each component:
    *   Implement virtual methods.
    *   Create AppActions for implementing all operations.
    *   Decide which objects implement which operations.
    *   Decide which operations are implement in the AppAction or in the object.
    *   Decide if objects represent state machines.

## Class Structure

<pre>   

            +-- ISA ----[Module]
            |
[AppBackplane]--HAS N ----[AppComponent]
            |
            +-- HAS 1 ----[AppScheduler]

[AppScheduler]-- HAS N ----[AppWorkerThread]
          | |
          | +-- HAS N ----[WorkQueue]
          |
          +-- HAS 1 ----[WorkQueue]

            +-- ISA ----[Module]
            |
[AppComponent]-- HAS N ----[AppObject]
            |
            +-- HAS N ----[AppEndpoint]

[AppWorkerThread]-- ISA ----[Task]

[AppObject]-- HAS N ----[AppAction]

[AppAction]-- ISA ----[Action]

[AppEndpoint]

[MsgHandlerEndpoint]-- ISA ----[AppEndpoint,MsgHandler]

[HermesImplement]-- ISA ----[MsgHandlerEndpoint]

[DbAdd]-- ISA ----[HermesImplement]    

</pre>

## Hermes Request Processing Example

This is an example how application requests over Hermes are handled using AppBackplane. Note, there's a lot of adapter cruft to integrate an external messaging layer into AppBackplane. But it's quite common to use a third party messaging library, so we have to have a way to take a blob of bytes over the wire and turn it into a typed message and then determine where the message should be processed. With a messaging layer that knows about AppBackplane it can be much cleaner.

*   An application has created its AppComponent. The routes it supports are registered during construction.
*   Routes derive from MsgHandlerEndpoint which is a MsgHandler. It supplies the namespace and in what system states the operations should be registered.
*   At the appropriate system state the backplane will register with Hermes.
*   Hermes requests for the operation are routed by Hermes.
*   MsgHandlerEndpoint calls CreateImplementationAction so the derived MsgHandlerEndpoint class can create an action that knows how to implement the operation. 
*   During the CreateImplementationAction process the MsgHandlerEndpoint must have figured out which object the action should be queue to. It will do this using information from the request or it will have predetermined objects already in mind. The AppComponent and AppBackplane are available.
*   Objects capable of processing AppActions must derive from AppObject.
*   The backplane is asked to schedule the AppAction.
*   The AppAction is queued to the AppObject specified in the AppAction.
*   The AppObject is put on the priority queue specified by the AppObject.
*   An AppWorkerThread is allocated from the idle pool.
*   The AppWorkerThread thread priority is set to that indicated by the AppObject.
*   The AppObject is given to the AppWorkerThread when it gets a chance to run.
*   If a worker is not available the work will be scheduled later.
*   The AppWorkerThread is put on the active pool.
*   The first AppAction in the queue is dequeued and the action's Doit method is invoked. The Doit method is the action's way of saying do whatever you are supposed to do.
*   How the action accomplishes its task is up to the action. All the logic for carrying out the operation may be in the action. The action may invoke an event on a state machine and let the object take over from there. The operation may:
    *   do its work and reply immediately
    *   do some work and block waiting on an asynchronous reply
    *   perform a synchronous call
*   If the object needs to send out more requests it will send the request and then call DoneWaitForReply so it will be put in the wait queue waiting for a reply.
*   If the object is done it calls Hermes's Reply to send the Reply back to the sender. Then it calls DoneWaitForMoreWork to schedule it for more work.

### Scheduling Algorithm

AppBackplane implements a scheduling layer on top of OS thread scheduling. An extra scheduling layer is necessary to handle fairness, low latency for high priority work, and throttling in a high continuous load environment.

The scheduling algorithm is based on:

*   **integration period** - a period, 1 second by default, over which CPU utilization is calculated. Quotas are replenished. When this timer fires we call it a "tick."
*   **priority queues** - work is performed in priority order. Priorities are 0..N where 0 is the highest priority. The number of queues is configurable.
*   **thread pool** - more than one thread can be active at a time. The thread is scheduled and is run at the priority set by the work being done. The number of threads in the pool is configurable. Thread can be dynamically added and deleted.
*   **CPU limitation** - each backplane can use only so much of the CPU in an integration period. If the period is exceeded work is stopped. Work is started again on the next tick.
*   **quotas** - only so much work at a priority is processed at a time. Quotas are settable by the application. By default quotas are assigned using the following rules:
    *   The highest priority has an unlimited quota.
    *   The next highest priority has a quota of 100\.
    *   Every priority then has 1/2 have the previous higher priority until a limit of 12 is reached.
*   **priority check** - after each event is processed the scheduler starts again at the highest priority that has not used its entire quota. The idea is work may have come in for higher priority work while lower priority work was being processed. A priority may run out of work before its quota has been used so there may be more work that can be done at a priority.
*   **virtual tick** - If all the CPU hasn't been used and all quotas at all priorities have been filled, then a virtual integration period is triggered that resets all quotas. This allows work to be done until the CPU usage limit is reached. Otherwise the backplane could stop processing work when it had most of its CPU budget remaining. The CPU usage is not reset on a virtual tick, it is only reset on the real tick.

## Backplanes are Not Organized by Priority

It is tempting to think of backplanes as being organized by thread priority. This is not really the case. Any application has a mix of work that can all run at different thread priorities.

You probably don't wan't to splat an application across different backplanes, though technically it could be done.

The reason is backplanes are more of a shared data domain where latency contracts are expected to be followed. Different backplanes won't share the same latency policies, lock policies, or ready queue priorities and quotas.

Thus backplanes can't be thought of as able to cooperated with each other.

## CPU Throttling

CPU throttling is handled by the scheduler.

## Request Throttling

Requests are throttled by having a per component operation limit. When a component reaches its limit and a new operation comes in then the component is asked to make room for new work, if the component can't make room then the request is failed with an exception.

The client can use the exception as a form of back pressure so that it knows to wait a while before trying the request again.

## Priority Inheritance for Low Priority Work

Low priority work should have its priority raised to that of the highest work outstanding when the low priority work is blocking higher priority work that could execute. The reasoning is...

Work is assigned to threads to execute at a particular priority.

Thread priorities are global. We must assign thread priorities on a system wide basis. There doesn't seem to be a way around this.

Each backplane has priority based ready queues that are quota controlled. High priority work that hasn't met its quota is executed. By priority we are talking ready queue priority, not thread priority. Thread priority is determined by the work.

Ready queue priorities and quotas are primarily determined by the backplane. They are ultimately constrained by a CPU limit on the backplane.

Lower priority work is scheduled when there is no higher priority work or higher priority work has met its quota.

When lower priority work has been assigned to a thread higher priority work can come in.

The lower priority work's task may not be scheduled because a higher priority task is executing elsewhere in the system. This causes the lower priority work not the be scheduled and to not get a chance to run. In fact, it can be starved.

The lower priority work is causing the higher priority work not to execute, even if the higher priority work would run at a task priority higher than the currently running task that is blocking the lower priority work from running.

By upping the task priority of the lower priority work's task to that of the higher priority work's task priority we give the lower priority work a chance to run so it can complete and let the higher priority work run.

The lower priority work can run to completion or it can use global flags to know if it should exit its function as soon as possible so the higher priority work can run.

The priority inheritance works across backplanes so high priority work in any backplane will not block on lower priority work in any other backplane. The higher priority work is present flag is global.

Even if the lower priority work doesn't not give up the critical section immediately, just giving it a chance to run will make it so the higher priority work can get scheduled sooner.

## Backplane Doesn't Mean No Mutexes

Mutexes can still be used within a backplane, but they should not be shared with threads outside the backplane because there can be no guarantees about how they are used.

Applications inside a backplane should be able to share mutexes and behave according to a shared expectation of how long they will be taken and when they will be taken.

Fair sharing is implemented by a max CPU usage being assigned to each backplane.

## Yep, it's complicated.

A lot of the complication has to do with mapping different work input sources (messages, timers) into a common framework that can execute in an AppBackplane. Some form of this has to be done unless you really want have multiple threads running through your code without any discipline other than locks. That's a disaster waiting to happen.

Other complications are the typical complexity you have with decoding and routing messages to handlers. This is always necessary however.

There's complication around thinking about dependencies between components and the different states your application can be in, like start, initializing, primary, secondary, down. A lot of this is usually hidden in an application with lots of hacks to get around the weird dependencies that grow over time. I think it's better to make dependencies a first class component of your application architecture so you can gain some power out of it instead of it being a constant source of bugs.

And then there's the actual scheduling, which is complicated, but it does allow you to tune what work gets done when, which is the point of at all as you start trying to handle  more and more load while minimizing latencies and guaranteeing SLAs. 

It's something to think about anyway.

</div>