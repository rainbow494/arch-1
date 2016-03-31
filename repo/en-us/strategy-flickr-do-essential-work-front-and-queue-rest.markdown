## [Strategy: Flickr - Do the Essential Work Up-front and Queue the Rest ](/blog/2008/10/8/strategy-flickr-do-the-essential-work-up-front-and-queue-the.html)

    

    This strategy is stated perfectly by Flickr's Myles Grant: _The Flickr engineering team is obsessed with making pages load as quickly as possible. To that end, we’re refactoring large amounts of our code to do only the essential work up front, and rely on our queuing system to do the rest._ Flickr uses a queuing system to process 11 million tasks a day. Leslie Michael Orchard also does a great job explaining the queuing meme in his excellent post [Queue everything and delight everyone](http://decafbad.com/blog/2008/07/04/queue-everything-and-delight-everyone). Asynchronous work queues are how you scalably solve problems that are too big to handle in real-time.  

The process:  
*   **Identify the minimum feedback the client (UI, API) needs to know an operation succeeded**. It's enough, for example, to update a client's view when a posting a message to a microblogging service. The client probably isn't aware of all the other steps that happen when a message is added and doesn't really care when they happen as long as the obvious cases happen in an appropariate period of time.  
    *   **Queue all work not on the critical path to a job queueing system so the critical path remains unblocked**. Work is then load balanced across a cluster and completed as resources permit. The more [sharded](http://highscalability.com/tags/shard) your architecture is the more work can be done in parallel which minimizes total throughput time.  

    This approach makes it much easier to bound response latencies as features scale.  

    ## Queues Give You Lots of New Knobs to Play With

    As features are added data consumers multiply, so throwing a new task into a sequential process has a good chance of blowing latencies. Queueing gives much more control and flexibility over the performance of a system. With queues some advanced strategies you have at your disposal are:  
    *   **Horizontal scaling**. Add more processing resources to do more work in parallel.  
    *   **Priority order processing**. Paying customers, can be processed first, for example. Take measures to avoid starvation.  
    *   **Aggregation**. Work sitting on the same queue for the same user can be aggregated together so it can be processed as a batch.  
    *   **Work canceling**. A request later in the queue can cancel work earlier in the queue. These can just be dropped.  
    *   **CPU limitting**. When jobs have unbounded CPU time it destroys the latency for other jobs sitting in the queue. Bounding CPU limits on jobs evens out latency for everyone.  
    *   **Low priority work dropping**. Under load low priority jobs can be dropped. Just make you have background sweep processes that catch work that should have been done and redoes it.  
    *   **Admission control**. Under load clients can be told about when to retry. This is the best form of flow control, end-to-end flow with the client. We want to push back on work as high up the stack as we can. Stop the client from pushing work to you and you've accomplished something. Just having blind retries and timeouts puts immense pressure on the whole system.  

    These ideas have been employed in embedded real-time systems forever and now it seems they'll move into web services as well.  

    ## What Can You do with Your Queue?

    The options are endless, but here are some uses I found out in the wild:  

    *   Backfill jobs. Backfill is what Flickr calls asynchronous job that: alter database tables in preparation for a new feature; fix existing features; or other operation that touch a lot of accounts, photos, or groups. For example, a sharding approach means related data is spread through many different shards. To delete a user account would require visiting each shard to delete that users data. Each of those deletes would be queued to they could be done in parallel. Now lets say a bug prevented some of the user data from deleting. After the bug was fixed the user data for all the impacted user accounts would have to be scheduled to be deleted again.  
    *   Low latency funciton call router.  
    *   Scatter/gather calls in paralellel.  
    *   Defer expensive library calls.  
    *   Parellize database queries.  
    *   Job queue system for a cluster. Efficiently use all your pool of CPU power.  
    *   Sending scheduled mail merged emails.  
    *   Creating guest hosts  
    *   Put heavy code on backend instead of the web server.  
    *   Call a cron script to update topic hits and popular article hits.  
    *   Clean useless data from database because it's outdated.  
    *   Resize photos.  
    *   Run daily reports.  
    *   Update search indexes.  
    *   Speed up batch jobs by running them in parallel.  
    *   SpamAssassin spamtraps.  

    ## Queuing Implies an Event Driven State Machine Based Client Architecture

    Moving to queuing has architecture implications. The client and server are nolonger connected in a direct request-response sort of way. Instead, the server continually sends events to clients. The client is event driven instead of request-response driven. Internally clients often simulates the reqest-response model even though Ajax is asynchronous. It might be better to drop the request-response illusion and just make the client an event driven state machine. An event can come from a request, or from asynchronous jobs, or events can be generated by others performing activities that a client should see. Each client has an event channel that the system puts events on for a client to consume. The client is responspible for making sense of the event in its current context and is capable of handling any event regardless of its original source.  

    ## Queuing Systems

    If you are in the market for a queuing system take a look at:  
    *   [Gearman - Open Source Message Queuing System](http://highscalability.com/product-gearman-open-source-message-queuing-system/)  
    *   [Amazon's SQS](http://aws.amazon.com/sqs/). The latencies for this service tend to be high and variable so it may not be appropriate for all tasks.  
    *   [beanstalkd](http://xph.us/software/beanstalkd/).  
    *   Apache [ActiveMQ](http://activemq.apache.org/).  
    *   [Spread Queue](http://search.cpan.org/~jmay/Spread-Queue-0.4/)  
    *   [Rabbit MQ](http://www.rabbitmq.com/)  
    *   [Open AMQ](http://www.openamq.org/)  
    *   [The Schwartz](http://search.cpan.org/~bradfitz/TheSchwartz-1.07/lib/TheSchwartz.pm)  
    *   [Starling](http://rubyforge.org/projects/starling/)  
    *   [Simple MQ](http://sourceforge.net/projects/simpleMQ)  
    *   Roll your own.  

    ## Related Articles

    *   [Flick Engineers Do it Offline](http://code.flickr.com/blog/2008/09/26/flickr-engineers-do-it-offline/) by Myles Grant  
    *   [Queue everything and delight everyone](http://decafbad.com/blog/2008/07/04/queue-everything-and-delight-everyone) by Leslie Michael Orchard.  
    *   [Gearman - Open Source Message Queuing System](http://highscalability.com/product-gearman-open-source-message-queuing-system/)  
    *   [GridGain: One Compute Grid, Many Data Grids](http://highscalability.com/gridgain-one-compute-grid-many-data-grids)  
        