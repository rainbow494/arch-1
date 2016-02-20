## [How Autodesk Implemented Scalable Eventing over Mesos](/blog/2015/8/17/how-autodesk-implemented-scalable-eventing-over-mesos.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, August 17, 2015 at 8:56AM</span></div>

<div class="body">

![](https://farm1.staticflickr.com/700/20605921095_b31535e553_m.jpg)

_This is a guest post by [Olivier Paugam](https://www.linkedin.com/pub/olivier-paugam/2/214/475), SW Architect for the [Autodesk](http://www.autodesk.com/) Cloud. I really like this post because it shows how bits of infrastructure--Mesos, Kafka, RabbitMQ, Akka, Splunk, Librato, EC2--can be combined together to solve real problems. It's truly amazing how much can get done these days by a small team._

I was tasked a few months ago to come up with a _central eventing system_, something that would allow our various backends to communicate with each other. We are talking about activity streaming backends, rendering, data translation, BIM, identity, log reporting, analytics, etc.  So something really generic with varying load, usage patterns and scaling profile.  And oh, also something that our engineering teams could interface with easily.  Of course every piece of the system should be able to _scale on its own_.

I obviously didn't have time to write too much code and picked up [Kafka](http://kafka.apache.org/) as our storage core as it's stable, widely used and works okay (please note I'm not bound to using it and could switch over to something else).  Now I of course could not expose it directly and had to front-end it with some API. Without thinking much I also rejected the idea of having my backend manage the offsets as it places too much constraint on how one deals with failures for instance.

So what did I end up with?  Two separate tiers: **first an API tier that handle incoming traffic and then a backend tier hosting long-lived, stateful streaming processes** talking to Kafka (e.g implementing producers & consumers).  Both tiers can scale independently and just require consistent routing between them to ensure clients keep talking to the same backend streaming process.

Those tiers are implemented 100% in **Scala** and use the [Play! framework](https://www.playframework.com/).  They heavily rely on the **Akka actor system** as well (each node typically runs a few hundreds actors).  The backend tier implements a custom set of Kafka producers & consumers and uses a dedicated set of actors to manage read-ahead & write buffers.  Everything is implemented as nested finite-state machines (I love that concept).  Analytics go to **Splunk** while metrics fly off to **Librato** (_collectd_ running in-container).

[![1-4](http://cloudengineering.autodesk.com/.a/6a01b7c7651b22970b01bb08323d7f970d-800wi "1-4")](http://cloudengineering.autodesk.com/.a/6a01b7c7651b22970b01bb08323d7f970d-pi)_Artistic rendering of what publishing looks like._

So, how do I route between my two tiers?  Simply by using [RabbitMQ](https://www.rabbitmq.com/) which is so durable & resilient it's not even funny. AMQP queues are a great way to implement this simple "phone-switch" pattern. It's trivial to scale it up as well by using some logical sharding (e.g hash over some cookie present in each transaction or something similar) that will associate a fixed set of backend nodes to one RabbitMQ broker.

Why didn't I cluster my RabbitMQ brokers?  Well, mostly because I was lazy and also because it's not really necessary.  **Sharding traffic amongst individual brokers** is actually as efficient and way easier to control in my opinion. The additional work to be done is negligible compared to the benefits.

So in short given some container topology our requests will stick to specific paths depending on what backend node is hosting what streaming session.  **Scaling the whole thing is as trivial as scaling each tier independently given what you need**.  The only practical limitation will come from your virtual network adapter and how much bandwidth can go through.

[![1-3](http://a6.typepad.com/6a01b7c766c713970b01bb083239de970d-500wi "1-3")](http://a6.typepad.com/6a01b7c766c713970b01bb083239de970d-pi)_The dotted line shows the path requests from a given session would stick to._

Now here comes the fun part: **how do we guarantee reliable traffic and avoid byzantine failures?** Mucho easy I say, just go for a simple 2-phase commit-esque protocol and model both your client and your backend as mirrored state-machines (e.g they are always in sync). That can be accomplished by having read and write operations requiring an explicit acknowledgement request. You attempt to read something and if you fail you just re-try until you can place an acknowledgement which then mutates the backend (e.g move the Kafka offset forward or schedule a block of events for publishing). So the traffic between my clients and the backend is really like "allocate session", "read", "ack", "read",  "ack" ... "dispose".

The huge advantage of such a system is that **you effectively render your operations idempotent,** plus you can **encode all that logic in a state-machine** without any annoying declarative statements (_note to myself: i should provide a purely functional implementation just to be cool_).  Any network failure is of course re-tried gracefully. You also get free control-flow and back-pressure by the way.

So this whole thing is exposed as an [Apache Thrift](https://thrift.apache.org/) API (funneled over HTTPS with compression right now with work planned to move it to plain TCP at some point). I have client SDKs in Python, Scala, .NET and Ruby to go with the dazzling variety of technologies we use at Autodesk).  Please note the Kafka offset is managed by the clients (it is opacified though), which renders the backend even simpler to control.

Hang on you say.  **How do you handle cases where a backend node goes down?** Well thanks to the 2-phase protocol we don't really case when reading data: the client gets repeated failures then re-allocate a new streaming session using its current offset.  Worries come when writing data to Kafka since this is asynchronous and potentially subject to downstream back-pressure (say your node is failing and the Kafka broker is having issues too .. urg).  I equipped my backend nodes with a graceful shutdown which will fast-fail any incoming request while waiting for any pending write to go through. In a very last resort we can even flush any pending data to disk (and re-inject it later).

Hang on again you say.  **What if part of the infrastructure explodes?**  Same thing. Any traffic disruption between you and the actual backend node handling your streaming session will slow you down of course but won't have any nasty impact thanks again to the 2-phase protocol.

Oh I forgot to add the **data at rest is automatically encrypted** (AES 256) before landing in the Kafka logs.  No more lame key sharing trying to do the same using vanilla Kafka producers and consumers. On the security topic I can add that our streaming sessions are authenticated via OAUTH2, use a per-request MD5-HMAC challenge and go over TLS down to the backend cluster.

So, how do we deploy all that funky system are you now asking?  Well we run it 100% over a plain [Mesos/Marathon](https://github.com/mesosphere/marathon) cluster (not [DCOS](https://mesosphere.com/) right now but we could switch to it and benefit from their awesome dashboard).  The cluster is **hosted on AWS EC2** at this point and we basically multiplex the whole thing on a handful of c3.2xlarge instances (10 to 20 for a small deployment on a given region is plenty).  Please note we could do the exact same thing on [Kubernetes](http://kubernetes.io/) (either EC2 or GCE).

[![Stack](http://a4.typepad.com/6a01b7c766c713970b01b7c78e36d4970b-500wi "Stack")](http://a4.typepad.com/6a01b7c766c713970b01b7c78e36d4970b-pi)

_A little reminder of how our stack is structured._

Everything is deployed using our [Ochopod](https://github.com/autodesk-cloud/ochopod) technology (self-clustering containers) which is by the way open-sourced as well.  Operations are reduced to the absolute minimum.  For instance a graceful build push for, say, the API tier is just a matter of allocating a few new containers, waiting for them to settle down and then phasing out the old one.  All of this is done via a dedicated Jenkins slave running in the cluster (and being itself an Ochopod container too) !

I actually developed the [Ochothon](https://github.com/autodesk-cloud/ochothon) mini-PaaS just to be able to quickly dev/ops all those containers.

[![Screen Shot 2015-05-21 at 9.00.31 AM](http://cloudengineering.autodesk.com/.a/6a01b7c7651b22970b01b8d117b6df970c-800wi "Screen Shot 2015-05-21 at 9.00.31 AM")](http://cloudengineering.autodesk.com/.a/6a01b7c7651b22970b01b8d117b6df970c-pi)

_Ochothon CLI showing one of my pre-production cluster._

To give you an idea of how useful all that Ocho-* platform is I will just say that **one single person (me) is able to manage 5 deployments of the system over 2 regions including all the replication infrastructur**e ... and still has time to blog stuff and code away!

So overall it was a lot of fun designing and coding the whole thing, plus it is now running in production as a mission critical part of our Cloud infrastructure (which is a nice bonus).  Let us know if you want to learn more about this exotic streaming system!

## Related Articles

*   [Deploying our eventing infrastructure over Mesos/Marathon & Ochothon !](http://cloudengineering.autodesk.com/blog/2015/05/after-a-long-day-of-devops-to-inaugurate-the-latest-ochothon-release-i-though-id-share-quick-what-it-feels-like-deploying-ou.html)
*   [Ochopod + Kubernetes = Ochonetes](http://cloudengineering.autodesk.com/blog/2015/05/ochopod_plus_kubernetes.html)
*   [Return of the finite state machine!](http://cloudengineering.autodesk.com/blog/2015/07/fsm.html)

</div>