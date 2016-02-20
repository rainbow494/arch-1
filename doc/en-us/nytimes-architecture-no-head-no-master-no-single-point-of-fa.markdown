## [NYTimes Architecture: No Head, No Master, No Single Point of Failure](/blog/2014/1/13/nytimes-architecture-no-head-no-master-no-single-point-of-fa.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, January 13, 2014 at 8:54AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7435/11844140264_1dd88baca3_m.jpg)

Michael Laing, a Systems Architect at NYTimes, gave this [great decription](http://lists.rabbitmq.com/pipermail/rabbitmq-discuss/2014-January/032920.html) of their use of [RabbitMQ](http://www.rabbitmq.com/) and their overall architecture on the RabbitMQ mailing list. The closing sentiment marks this as definitely an architecture to learn from:

> Although it may seem complex, <span>Fabrik</span> has simple components and is mostly principles and plumbing. The key point to grasp is that there is no head, no master, no single point of failure. As I write this I can see components failing (not RabbitMQ), and we are fixing them so they are more reliable. But the system doesn't fail, users can connect, and messages are delivered, regardless - all within design parameters.

Since it's short, to the point, and I couldn't say it better, I'll just reproduce several original sources here:

> Just a quick note and thank you to the RabbitMQ team for a great product.
> 
> Our premier online offering www.nytimes.com has a new look and new underpinnings, now including a messaging architecture implemented using RabbitMQ.
> 
> This architecture - <span>Fabrik</span> - has dozens of RabbitMQ instances spread across 6 AWS zones in Oregon and Dublin. The instances are organized into "wholesale" and "retail" layers. Connection to clients is via websockets/sockjs.
> 
> Upon launch today, the system autoscaled to ~500,000 users. Connection times remained flat at ~200ms.
> 
> Fabrik provides subscription services for breaking news, video feeds, etc. and will add more event based services. It also supports individual messaging related to subscription status for registered users.
> 
> This system would not have been possible without RabbitMQ. It was the one component, used everywhere, that never faltered or failed.
> 
> We are using: a single Amazon Linux AMI, RabbitMQ, Cassandra 2, python 2
> 
> We use pika with tornado and libev for the nyt⨍aбrik wholesale and retail pieces; our internal clients use Java and PHP.
> 
> We use Rabbit MQ as our message passing system. Right now, the messages we handle are things like Breaking News Alerts and Live Video alerts. Our internal clients send the fabrik these messages over AMQP. We then send them around our stack, ensuring they are delivered.
> 
> We have Rabbit in all layers of our stack, with shovels connecting them. Our own internal code helps route the messages based on there services level. Some messages, like Breaking News, must go out as quickly as possible. So we spread these out over out clusters AND shovel them to clusters in other regions for processing. From there the messages get send to the front end for delivery.
> 
> We also use Rabbit for individual messages. If you are a registered NYTimes users, we can send you personally a message. Things like credit card expiring.
> 
> In production we have a RabbitMQ client 3-cluster and a core 3-cluster in each region on c1-xlarges. A proxy cluster of c1-mediums in Virginia connects clients to the client clusters. All services are parallelized so we can add more cores and clients.
> 
> The retail layer autoscales and use c1-mediums with a single rabbit shovel-connected to one of the core rabbits. Each python websocket/sockjs gateway supports up to 100K clients.
> 
> We autodeploy into subnets within Virtual Private Clouds in AWS. Clients are routed via least latency to the fastest healthy region.
> 
> Of the technical components, the gateway is the most complex. We will be moving it into open source in pieces and the first piece is likely to be the python websocket/sockjs libraries which, frankly, beat the crap out of most other stuff out there and fully conform with the relevant standards. It can be loosely thought of as a C co-process managed by python, and as such, may be possible to reuse in other languages/environments.
> 
> We have a 12-node Cassandra cluster across the 2 regions / 6 zones. It is used for persistence of messages and as cache. We do not use persistence in RabbitMQ. Our services are idempotent and important messages may be replicated multiple times creating intentional race conditions in which the fastest wins.
> 
> Although it may seem complex, <span>Fabrik</span> has simple components and is mostly principles and plumbing. The key point to grasp is that there is no head, no master, no single point of failure. As I write this I can see components failing (not RabbitMQ), and we are fixing them so they are more reliable. But the system doesn't fail, users can connect, and messages are delivered, regardless - all within design parameters.

## Related Articles

*   [On Reddit](http://www.reddit.com/r/programming/comments/1v4gzj/nytimes_architecture_system_doesnt_fail_users_can/)

</div>