## [22 Recommendations for Building Effective High Traffic Web Software](/blog/2013/12/16/22-recommendations-for-building-effective-high-traffic-web-s.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, December 16, 2013 at 8:54AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7308/11359657305_60cbab5826_m.jpg)

_This is a guest post by Ashwanth Fernando, Software Engineer from the trenches at large scale internet companies._

Inspired by the book "Effective Java" by Joshua Bloch, I wanted to share my holistic recommendations on building high traffic web software (i.e. web applications/services that serve high traffic loads). Some of these items may not be just about software design but also around surrounding areas such as the engineering organization, culture etc.

Two disclaimers up front:

1) This is my opinion.  
2) There will be real world situations where the below principles will be wrong as in all things "software". Please use common sense all the time.

**  
Consider using more than one datacenter**

There have been numerous horror stories about businesses, ahem going out of business because they just had a single datacenter. Its really important to have more than one data center if you want to protect yourself from natural disasters or electrical supply failures. Run all your datacenters in active-active configuration. It may cost extra money, but its well worth it rather than having an active passive configuration and then finding out at the end that for some pieces of data, your passive hardware was not consistent with the active one.

**Consider a sparse datacenter deployment**

Make sure that your operations team or your PaaS (if you are running a private cloud) really deploys your software cluster across racks that span multiple electrical power supply phases within a data center. The switch to a rack may fail or failures may occur that may cause an entire rack to go offline, you do not want to find out at that time that all instances of your software was deployed on the same rack in a datacenter.

**Consider running a private cloud**

The IaaS open source software OpenStack et al. are not mature yet and require a huge operations team to run it because many many errors and failures occur during provisioning, so do not consider building a private cloud unless you really have the budget for it, but a private cloud offers many advantages. It first allows you to customize many aspects of your deployment much more than what AWS or Rackspace can offer such as choosing racks that are close by for consensus seeking software such as Zookeeper. Second it allows you to do bare metal provisioning. Oracle is way more faster on bare metal than in a paravirtualized environment.

**Consider using a PaaS**

The days of huge teams sitting on a deployment call for a software release are nearing their end. These days organizations want agility and lesser time to market. Having a PaaS in house allows your developers (with some reasonable level of authorization and control) to perform deployments themselves quickly. It allows features to go to production quickly and quickly you will find that your developers are also very satisfied these days. That's right, giving developers self service tools to deploy their software and maintain it is a huge increase in work satisfaction. A growing number of developers these days, do not even join organizations that do not have an automatic software deployment system. The lesser the bureaucracy and ceremonial processes, the better.

**If possible use Oracle or MySQL for nothing other than looking up records based on primary keys** 

Oracle performs its best at extreme traffic when there are not too many database artifacts in the RAC. Try as much as possible not to have referential integrity, triggers, materialized views, views, stored procedures and other Oracle artifacts. Move all of these to the application tier. Every ORM gives you referential integrity. Triggers can be done from the data access layer itself. Stored procedures can be completely moved onto the application tier. Use the database only for storing data and nothing else. For searching records based on fields other than the primary key, index the table or tables into a search indexer such as Lucene and expose a search service that allows searching of records by other fields which will then return the primary key of the record. This primary key can be further used to fetch the record.

**Consider sharding Oracle or MySQL**

Oracle's scalability is limited when it comes to a monumental schema. Thus it is recommended to shard your schema functionally (i.e. orders, product catalog, promotions, customer.. etc) as well as key shards for highly populated tables.  

Use consistent hashing for key shards so that when a new RAC is added that contributes to the total shard set of RACs, you don't have to go through all keys of the all RACs to find out which are the keys that needs to be moved to the key shard.

**Consider using Data Guard and Golden Gate, if the RDBMS is Oracle**

Using these two technologies can greatly simplify operations life for Oracle. Data Guard allows a near instant read replica which is passive (no clients connect to it) and Golden Gate allows a near instant active read-write replica.

One of the recommended deployment topologies is to have 1 dataguarded node for every shard in the same data center.  

Use Golden Gate to replicate every shard of Oracle in a data center across the remaining data centers.

**Note:** Golden Gate is near realtime and not real time.

**Consider a data access layer for Oracle or MySQL**

Lets assume you have an Oracle RAC which can accept 500 connections. Now consider that you have 25 jBoss instances talking to this Oracle RAC. For each jBoss instance the configuration of the database connection pool is 10 min and 50 max.

When the jBoss cluster starts up, the number of open connections to Oracle is 250 (25 x 10). Everything works perfectly fine. Imagine whats going to happen as traffic peaks up on the jBoss cluster. After a point, Oracle is going to start refusing connections.

Hence it is recommended to multiplex application server connections through a multiplexer layer. This can be a simple netty app that runs in a cluster where each netty node can only make 25 connections to Oracle but can have as many inbound connections. It uses a event loop to multiplex the inbound connections to Oracle, but does not exceed the 25 connections to Oracle. It uses the Oracle JDBC drivers to talk to Oracle.

At the jBoss application tier level an SPI implementation can be written for JDBC that allows jBoss to talk to the netty multiplexer layer and treat it as if it were a database.

**Do not have transactions that span multiple data centers**

This is probably motherhood and apple pie now, but very useful piece of advice for anything, including Oracle. Do not have an XA adapter do a database transaction across Oracle RAC nodes in two different data centers. This will result in application threads blocking for considerable amounts of time until the ceremony of the two phase commit is completed thus bringing your application services, services and all upstream synchronous services to their knees and eventually bringing down your application during to increased number of threads during a Black Friday traffic situation.

**Consider using a distributed caching framework**

Memcached, Couchbase are some of the usual suspects. But really, offload relatively non volatile data into a central cache cluster, there is really no need for the same copy in each JVM. But do set a small amount of JVM heap as a MRU cache of the distributed cache. This will result in very few network calls to the cache cluster itself.

Most distributed caches support the concept of a local cache on the JVM which stores the most recently used objects.

The GC pause time of the JVM is also minimized because it has now fewer objects to traverse in the object graph while doing its marking phase.

Consider using a warmup process that is automatically triggered by the PaaS or by your operations team which hits HTTP urls in your application server cluster that warm up the distributed cache. Do this warmup at night or at a time when the customer traffic on those application servers are low.

**Consider splitting your web application into services**

God forbid, if you are in charge of a web application that is more than half a million lines of code and is still deployed as a single deployable, then its time to break it out into services, services that are split by function and assigned and operationalized under different sub-organizations/teams.

Breaking down a monolithic web application into services gives a lot of advantages:

1) Debugging production issues are easier.

2) Scaling and making individual subsystems perform better are easier.

3) Its easily to visualize and see what's going on in a production context when the subsystems are split up and easily understandable.

4) Adding new features is now faster.

**Do not use session stickiness**

This is absolutely the work of the devil. Session stickiness guarantees that one will not be able to scale under extreme loads. Your client should be able to call ANY application server and have its query answered. One way to do this is by making services stateless, also called as RestFUL services. The id that maintains state is passed to and from the client for every request and the data that represents a client's session is stored in the database or the distributed cache across multiple requests.

If for some reason a majority of your web-site is built around HttpServlets and HttpSession attributes instead of restful services, use the below way to achieve independence from session stickiness:

A servlet filter facades every service and takes the id for every request and makes a call to the distributed cache to fill up the session attributes which will be helpful for processing the request. Thus any server inside the datacenter can respond to the client's request because session state is maintained in memcached.

Not using session stickiness also allows a rolling restart of your application server cluster thus allowing achieving a 100% uptime.

**Do terminate SSL on the reverse proxy**

The reverse proxy is pretty good at SSL handshakes (some of which take 8 round trips) and keeping the underlying TCP connection active.

Set an explicit TCP keep alive timer on the reverse proxy. nGinx and many other http servers allow this and this allows the TCP connection to be reused multiple times. The cost of the TCP handshake is 3 network calls which you can avoid across many requests. 

Hence from the reverse proxy to the application servers, it is typically over RAW http. Hence enable TCP keep alive on the upstream connections as well.

**Use sticky load balancing for GSLB type load balancers**

For load balancing across datacenters, it is recommended to use session stickiness. This is because the databases - Oracle or Cassandra have only eventual consistency technologies in replicating across datacenters. Hence a non-sticky cross data center load balancer will make your clients never use your web site ever again.

Hence always use a GSLB. Your CDN mostly will have a locality based GSLB solution for datacenter load balancing.

**Reduce CNAME lookups on the home page.**

Try to reduce the number of CNAME lookups on the home page. Some websites (popular ones) have 10 or more CNAME lookups just for the home page alone. Even though your client's DNS lookup is probably being answered by their ISP's recursive caches, that is still not a the best that can be done.

www.amazon.com has zero CNAME lookups.

dig www.amazon.com 

;; QUESTION SECTION:

;www.amazon.com. <span></span> <span></span><span></span>IN <span></span> A

;; ANSWER SECTION:

www.amazon.com. <span></span> <span></span>28 <span></span> IN <span></span> A <span></span> 205.251.242.54

**Embrace all things "reactor"**

The reactor based pattern has proven itself time and again, a force to reckon for high traffic software systems. A flurry of frameworks have been created that implement the reactor pattern. The places where the reactor can be used are:

_As a reverse proxy: nGinx_

_As a application server: node.js_

_For parallel processing: Scala's actor model_

Unless your business logic is highly CPU bound, consider using the reactor pattern or the event loop based software.  

If that is not possible, consider a reactive programming model using frameworks like RxJava. 

**Implement Call Cancel**

Inspired from one of Siddharth Anand's conferences, in a call graph of service calls, first implement timeouts in decreasing number from the top to the bottom.

Next for every invocation of a call graph of services, create a UUID and set a flag for the UUID in a distributed cache:

UUID: true

If any service in the call graph times out, set the flag for the UUID to false.

Now implement a servlet filter for all services that checks this flag all the time and continues processing only if the flag is true. 

If the flag is false, the service in question would just return with a empty response.

This allows unnecessary calls from lighting up the call graph during times of high traffic

**Implement the GC scout protocol**

Again, inspired by the same guy - Make all services also expose a TCP port through Netty that returns a empty string. Before calling a service, call this TCP port and have a timeout of 2 ms - 5 ms for the call. If the call times out, that means the java process in question is doing a "stop the world" garbage collection. 

Have the client immediately switch over to another instance of the service and try the same procedure again. If the call succeeds, then call the actual service on the instance.

**Note:** Client side configuration of ip addresses (i.e. client side load balancing) is required to implement the GC scout protocol.

**Make business logic and I/O access asynchronous as much as possible**

Making business logic asynchronous allows your application to be shielded from excessive thread creation during heavy bursts of traffic. Push events out into a load balanced cluster of queues that are subscribed by processes which perform the business logic instead of trying to perform the same within the http request/response lifecycle thread.

**Bias on eventual consistency databases**

Especially when you are running cross datacenter applications. Unless your use case is transactional (i.e. order placement) etc. bias on using eventual consistency databases like Cassandra and use as little ACID semantics type databases as possible.

**Do use a CDN to serve static content**

Use a CDN to serve static content - javascript, images, css etc. CDNs are effective in replicating static content to a locality near the customer and hence many http requests to these static content end up really crossing hundred miles at the most.

**Bundle and minify javascript into one file**

Self explanatory. Also minimize inline javascript. 

Note. Do not do this in pre-prod environments, one would require the ability to debug javascript using a debugger.

What are the recommendations that you have? Please share your valuable insights.

</div>