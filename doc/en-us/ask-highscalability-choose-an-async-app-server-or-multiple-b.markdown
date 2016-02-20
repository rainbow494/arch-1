## [Ask HighScalability: Choose an Async App Server or Multiple Blocking Servers?](/blog/2015/8/24/ask-highscalability-choose-an-async-app-server-or-multiple-b.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, August 24, 2015 at 8:56AM</span></div>

<div class="body">

![](https://farm1.staticflickr.com/696/20630118899_f811690958_m.jpg)

[Jonathan Willis](https://twitter.com/j_e_willis), software developer by day and superhero by night, asked an interesting question [via Twitter](https://twitter.com/j_e_willis/status/635228654516105216) [on StackOverflow](http://stackoverflow.com/questions/32108945/async-app-server-versus-multiple-blocking-servers): 

> tl;dr Many Rails apps or one Vertx/Play! app?
> 
>   
> I've been having discussions with other members of my team on the pros and cons of using an async app server such as the Play! Framework (built on Netty) versus spinning up multiple instances of a Rails app server. I know that Netty is asynchronous/non-blocking, meaning during a database query, network request, or something similar an async call will allow the event loop thread to switch from the blocked request to another request ready to be processed/served. This will keep the CPUs busy instead of blocking and waiting.
> 
> I'm arguing in favor or using something such as the Play! Framework or Vertx.io, something that is non-blocking... Scalable. My team members, on the other hand, are saying that you can get the same benefit by using multiple instances of a Rails app, which out of the box only comes with one thread and doesn't have true concurrency as do apps on the JVM. They are saying just use enough app instances to match the performance of one Play! application (or however many Play! apps we use), and when a Rails app blocks the OS will switch processes to a different Rails app. In the end, they are saying that the CPUs will be doing the same amount of work and we will get the same performance.

What do you think? The marketplace has seemingly moved, in the form of node.js, Golang, Akka, and even Java, to the async server model. Does that mean it's the only right way?

Here's my attempt at a response:

Both approaches can and have worked. So if switching would incur a high development cost and/or schedule hit then it's probably not worth the effort...yet. Make the switch when the costs become unacceptably high. Think of using microservices as a gradual switching strategy.

If you are early on in your development cycle then making the switch early may make sense. Rewriting is a pain.

Or perhaps you'll never have to switch and rails will work for your use case like a charm. And you've been so successful at making your customers happy that the cash is just rolling in.

Some of the downsides of a blocking single server approach:

*   **Increased memory usage**. Sources: multiple processes, memory leaks, lack of shared datastructures (which increases communication costs and brings up consistency issues).
*   **Lack of parallelism**. This has two consequences: **more boxes and more latency**. You'll need potentially a much larger box count to handle the same load. So if you need to scale and have money concerns then this can be a problem. If it isn't a concern then it doesn't matter. In the server it means increased latency, the sort of latency which can't be improved by multiplying processes, which may be a killer argument depending on your app.

Some examples of those who had made such a switch from rails to node.js and golang: 

*   [LinkedIn Moved From Rails To Node: 27 Servers Cut And Up To 20x Faster](http://highscalability.com/blog/2012/10/4/linkedin-moved-from-rails-to-node-27-servers-cut-and-up-to-2.html) 
*   [Why Timehop Chose Go to Replace Our Rails App](https://medium.com/building-timehop/why-timehop-chose-go-to-replace-our-rails-app-2855ea1912d)
*   [How We Moved Our API From Ruby to Go and Saved Our Sanity](http://blog.parse.com/learn/how-we-moved-our-api-from-ruby-to-go-and-saved-our-sanity/)
*   [How We Went from 30 Servers to 2: Go](http://www.iron.io/blog/2013/03/how-we-went-from-30-servers-to-2-go.html)

These posts represent arguments that are probably illustrative of what your group is going through. The decision is unfortunately not an obvious one.

It depends on the nature of what you are building, the nature of your team, the nature of resources, the nature of your skills, the nature of your goals and how you value all the different tradeoffs.

**Would costs really drop? Isn't the same amount of computation done no matter the number of servers?**

Depends on the type and scale of the work being done. Typically web services are IO bound, waiting on responses from other services like databases, caches, etc.

If you are using a single threaded server the process is blocked on IO a lot so it is doing nothing a lot. In contrast the nonblocking server will be able to handle many many requests while the single threaded server is blocked. You can keep adding processes, but there are only so many processes a single machine can run. A nonblocking server can have the same number of processes while keeping the CPU busy as possible handling requests. It's often possible to handle higher loads on smaller cheaper machines when using nonblocking servers.

If your expected request rate can be handled by an acceptable number of boxes and you don't expect huge spikes then you would be fine with single threaded servers. Nonblocking servers are great at soaking up load spikes without necessarily having to add machines.

If your work is such that response latencies don't really matter then you can get by with fewer nodes.

If your workload is CPU bound then you'll need more boxes anyway because there won't be the same opportunity for parallelism because the servers won't be blocking on IO.

</div>