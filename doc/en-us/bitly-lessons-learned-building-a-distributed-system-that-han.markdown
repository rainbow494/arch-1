## [Bitly: Lessons Learned Building a Distributed System that Handles 6 Billion Clicks a Month](/blog/2014/7/14/bitly-lessons-learned-building-a-distributed-system-that-han.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, July 14, 2014 at 9:00AM</span></div>

<div class="body">

![](https://farm4.staticflickr.com/3876/14642827891_279c2ac916_m.jpg)

<span>Have you ever wondered how</span> [<span>bitly</span>](http://bitly.is/1g3AhR6) <span>makes money? A</span> [<span>URL shortener</span>](http://en.wikipedia.org/wiki/URL_shortening) <span>can’t be that hard to write, right?</span> [<span>Sean O'Connor</span>](http://linkd.in/1nt6KTN)<span>, Lead Application Developer at bitly, answers the how can bitly possibly make money question immediately in a</span> [<span>talk he gave on bitly</span>](http://bit.ly/1iuEAlb) <span>at the</span> [<span>Bacon conference</span>](http://devslovebacon.com/)<span>.</span>

<span>Writing a URL shortner that works is easy, says Sean, writing one that scales and is highly available, is not so easy.</span>

<span>Bitly doesn’t make money with a Shortening as a Service service, bitly makes money on an analytics product that mashes URL click data with with data they crawl from the web to help customers understand what people are paying attention to on the web.</span><span style="font-size: 12px;"> </span>

<span>Analytics products began as a backend service that crawled web server logs. Logs contained data from annotated links along with cookie data to indicate where on a page a link was clicked, who clicked it, what the link was, etc. But the links all went back to the domain of the web site. The idea of making links go to a different domain than your own so that a 3rd party can do the analytics is a scary proposition, but it’s also kind of genius.</span>

<span>While this talk is not on bitly’s architecture, it is a thoughtful exploration on the nature of distributed systems and how you can solve bigger than one box problems with them.</span>

<span>Perhaps my favorite lesson from his talk is this one (my gloss):</span>

<span>**SOA + queues + async messaging is really powerful**</span><span>. This approach isolates components, lets work happen concurrently, lets boxes fail independently, while still having components be easy to reason about.</span>

<span>I also really like his explanation for why event style messages are better than command style messages. I’ve never heard it put that way before.</span>

<span>Sean talks from a place of authentic experience. If you are trying to make a jump from a single box mindset to a multibox way of thinking, this talk is well worth watching.</span><span style="font-size: 12px;"> </span>

<span>So let’s see what Sean has to say about distributed systems...</span>

## <span>Stats</span>

*   <span>6 billion clicks a month</span>

*   <span>600 million shortens a month</span>

*   <span>50 people in the company and approximately 20 engineers</span>

*   <span>400 servers. Not all 400 servers are handling redirects.  ~30 servers handle all incoming traffic from the outside world including shortens, redirects, api requests, web ui, etc.  The rest of the 400 are dedicated to various services either storing and organizing user data or providing various forms of processing and analysis (databases, metrics, web crawling, text analysis, etc).</span>

*   <span>Crawls a hundred million web pages a month.</span>

## <span>Source</span>

*   <span>[LESSONS LEARNED BUILDING DISTRIBUTED SYSTEMS AT BITLY](http://devslovebacon.com/conferences/bacon-2014/talks/lessons-learned-building-distributed-systems-at-bitly)</span>

## <span>Platform</span><span style="font-size: 12px;"> </span>

<span>Note, that these were just what was mentioned in the talk, it’s not a comprehensive list.</span>

*   <span>HDFS</span>

*   <span>S3</span>

*   <span>Nagios</span>

*   <span>The real time distributed messaging system bitly uses is</span> [<span>Nsq</span>](https://github.com/bitly/nsq)<span>.</span>

*   <span>Bitly uses</span> [<span>hostpool</span>](https://github.com/bitly/go-hostpool) <span>to manage a pool of hosts.</span>

*   <span>The end of the talk is cut off, but it’s mentioned bitly uses quite a few different databases.</span><span style="font-size: 12px;"> </span>

## <span>On the Nature of Distributed Systems</span><span style="font-size: 12px;"> </span>

*   <span>Anything that is truly highly available will be inherently distributed. To achieve certain levels of availability you must have geographic diversity. By definition if you have independently operating systems in different regions they have to be distributed.</span>

*   <span>Old way of creating distributed systems. Create abstractions that let you pretend things weren’t actually distributed.</span>

    *   <span>NetApp, for example, creates the illusion of an infinite disk.</span>

    *   <span>Oracle, is a highly available database that let’s you pretend the world is different than it actually it is.</span>

    *   <span>Both solve real problems, but both are expensive, so do something different.</span>

*   <span>New way. Approach problems by accepting the inherent characteristics of distributed systems and build those into the abstractions they provide as tools.</span>

    *   <span>Significant shift in how you think about things. Since your abstractions are more closely aligned with the realities of what you are building, you can make much more powerful tradeoffs and therefore do things more efficiently.</span>

*   <span>4 boxes with 1x RAM will always be cheaper than 1 box with 4X RAM. The implication is distributed systems are the way to scale and achieve high availability.</span>

*   <span>Problems with distributed systems occur because you are going from one machine to N machines.</span>

    *   <span>**Concurrency of components**</span><span>. Machine A does work at the same time as machine B. This is how you get horizontal scaling. Powerful but the cost is the need for coordination between machines. When locking data, for example, making boxes wait on each other,  you are no longer concurrent.</span>

    *   <span>**Lack of a global clock**</span><span>. Each box has an imperfect clock. With more than one machines each machine will have their own sense of time. Which means events happening on different machines can’t be ordered based on time. For bitly if events are 1 or 2 seconds apart, they don’t know which ones happened first.</span>

    *   <span>**Independent failure**</span><span>. If box A fails then box B should keep on working.</span>

*   <span>Some problems, like analytics are too big to handle on one machine. Or at least such a machine wouldn’t be on budget.</span>

## <span>Strategies for Implementing Distributed Systems</span>

### <span>Service Oriented Architecture</span>

<span>There’s no one monolithic bitly application. There are dozens of small services that talk to each other over the network.</span>

*   <span>It’s an abstraction much like functions are for code, but at the system level.</span>

*   <span>Don’t have to keep all the details of system inside your head. Can just pay attention to API boundaries and contracts.</span>

*   <span>Powerful for development and operations.</span>

*   <span>Bitly is a 6 year old company and has a lot of code. You don’t need to look at every line of code. You just use the service.</span>

*   <span>Well designed services are just a few hundred lines of code.</span>

*   <span>Operationally it’s easy to determine which system is having a problem. Then you can dig into that system to find the problem.</span>

*   <span>Failure now means reduced functionality instead of going down. Generally each service is designed to answer a particular question. Since they are completely isolated with their own persistence layers if one of them fails then the only thing that is lost is the ability to answer that question, but overall the services is still up. A metric system going down should never impact URL shortening requests.</span><span style="font-size: 12px;"> </span>

### <span>Asynchronous Messaging</span>

<span></span>

*   <span>Send a message without waiting for a reply.</span>

*   <span>Really easy to drop a queue in between components. If box A is sending messages to box B and box B is having issues, the messages will be queued up and can processed when box B recovers.</span>

*   <span>More options in dealing with errors. If a box goes bad the message will just be processed later. Box A didn’t have to know or care about box B’s troubles.</span>

*   <span>The downside is when the operation performed is more naturally modeled as a synchronous request.</span>

*   <span>A URL shortner request is a completely synchronous process because:</span>

    *   **Speed**. Want to return the reply back as fast as possible. Don’t want to deal with queue backups, for example.

    *   **Consistency**. Don’t want to give out the same shortened URL to multiple users. Rather give an error than a link that doesn’t work.

*   <span>Metrics systems is completely async. When a bitly link is clicked on it’s translated and returned via HTTP. Data about the conversion is popped onto a queue for the analytics and other downstream systems. If there’s a little delay in processing the metrics it’s not the end of the world.</span>

*   <span>Messages can be thought of as commands or events.</span>

    *   <span>A command says “do X.”</span>

    *   <span>An event says “X happened.”</span>

*   <span>Events turned out to be a much more useful abstraction than commands.</span>

    *   **Allows for better isolation between systems**. A command implies there’s knowledge of who receiving the command or it will be meaningless. Saying something happened means you don’t have to care who is consuming the message. Just advertise that X happened and other services can increment stats or do whatever.

    *   **Events naturally map to having multiple consumers process messages**. When a bitly URL is decoded into an HTTP redirect a message is sent to multiple services: an archive services that saves it to HDFS and S3,  a real-time analytics service, longer term history analytics, an annotation service. The decode service just has to publish the event without knowing anything about the downstream services. And the downstream services just get an event, they don’t care how or who sent it.

    *   **Adding new consumers is easy**. A new service can be created and it can register for events and the producer doesn’t know or care. Changes in how a service processes events is likewise not a concern of the producer. Producers and consumers are isolated and connect only over the contract specified by the event.

## <span>Making Services Play Nice</span>

*   **Use backpressure between services**. If a service busy it should tell other requesting services to throttle their requests, thus reducing their load on the troubled service. For example, exponential backoff. Helps prevent cascading errors through the system. Also helps a system recover faster. For example, a service that relies on caching needs a warmup period, if requesting services don’t back-off during warmup then the database can get crushed.

*   **Route around failures**. For example, load balance between services. Bitly uses [<span>hostpool</span>](https://github.com/bitly/go-hostpool) to manage a pool of hosts. A client asks for a host for a service. The client then tells hostpool on each request if the requests failed. Based on this feedback hostpool can manage host allocations to route to healthier hosts.

## <span>Monitoring</span><span style="font-size: 12px;"> </span>

*   <span>With one or two servers it’s not that hard to know when one is broken, but when you have hundreds of servers you need help.</span>

*   **Use a tool like Nagios to check server health**. Check stats like "is the box swapping?"

*   **Run integrity checks**. Example, the service is responding but the data it is returning is corrupted.

*   **Centralized logging**. The reason this is important is so you can detect failures across multiple hosts. If one user is causing all your errors then that will be hard to detect by hopping from machine to machine. Centralized logs make it easier to detect holistic problems, like all errors are coming from one ip address.

*   **Human interfaces**. How do you get the right information to the right people at the right time. How can you expose information from tools?

    *   <span>Nsq, for example, has a nice admin interface that gives feedback on how queues are behaving. So you can see a queue is backing up but it’s because one host.</span>

    *   <span>Web UI for deployment system. Makes it easy to deploy and see the deploy history. If Nagios is going crazy and someone just deployed they are probably related.</span>

## <span>Lessons Learned</span><span style="font-size: 12px;"> </span>

*   <span>**Knowledge is power**</span><span>. The more you understand the properties of the things you are working with the better decisions you can make and the more efficiently you can work. Efficiency means you can make bigger, faster systems at a much lower cost.</span>

*   <span>**Build stuff that deals with leaky abstractions**</span><span>. If you are using a layer of abstraction that hides the underlying distributed nature of the system it will eventually bubble through. Your code has to understand and deal with any leaks.</span>

*   <span>**Put it all on one box if you can**</span><span>. If you don’t need a distributed system then don’t build one. They are complicated and expensive to build.</span>

*   **In a Service Oriented Architecture failure means reduced functionality instead of going down**.

*   <span>**SOA + queues + async messaging is really powerful**</span><span>. This approach isolates components, lets work happen concurrently, lets boxes fail independently, while still having components be easy to reason about.</span>

*   <span>**Use synchronous requests when speed and consistency are paramount**</span><span>. Give users an error rather than a slow or wrong answer.</span>

*   <span>**Event style messages are better than command style messages**</span><span>. They allow for better isolation between systems and naturally support multiple consumers. Helps keep services focussed and not worried about anything beyond what the service is supposed to do.</span>

*   <span>**Annotate rather than filter**</span><span>. Filters at the producer level bake in assumptions about what the downstream services care about. Example is public vs private links. Filtering private links out of the stream means that services interested in private links won’t get the links they need. Instead, annotate an event with the fact that a link is either private or public and let services operate only on the kind of events they care about.</span>

*   <span>**Services should play nice with each other**</span><span>. Use back pressure to prevent a service from being overloaded and route around failed services.</span>

*   <span>**If there isn’t a Nagios check on something it’s almost certainly broken**</span><span>. You just don’t know it yet.</span>

*   <span>**Tools should expose information to humans**</span><span>. Get the right information to the right people at the right time.</span>

## Related Articles

*   [On Hacker News](https://news.ycombinator.com/item?id=8031606)

</div>