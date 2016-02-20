## [How Shopify Scales to Handle Flash Sales from Kanye West and the Superbowl](/blog/2015/11/2/how-shopify-scales-to-handle-flash-sales-from-kanye-west-and.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, November 2, 2015 at 8:56AM</span></div>

<div class="body">

![](https://c2.staticflickr.com/6/5701/22686657356_45fa0ef0d2_o.jpg)

_This is a guest [repost](https://scaleyourcode.com/blog/article/23) by [Christophe Limpalair](https://twitter.com/christophelimp), creator of [Scale Your Code](https://scaleyourcode.com)._

In this article, we take a look at methods used by Shopify to make their platform resilient. Not only is this interesting to read about, but it can also be practical and help you with your own applications.

## Shopify's Scaling Challenges

Shopify, an ecommerce solution, handles about **300 million unique visitors** a month, but as you'll see, these 300M people don't show up in an evenly distributed fashion.  

One of their **biggest challenge is what they call "flash sales"**. These flash sales are when tremendously popular stores sell something at a specific time.  

For example, **Kanye West** might sell new shoes. Combined with **Kim Kardashian**, they have **a following of 50 million people on Twitter alone**.  

They also have customers who advertise on the Superbowl. Because of this, they have no idea how much traffic to expect. It **could be 200,000 people showing up at 3:00** for a special sale that ends within a few hours.  

**How does Shopify scale to these sudden increases in traffic?** Even if they can't scale that well for a particular sale, how can they make sure it doesn't affect other stores? This is what we will discuss in the next sections, after briefly explaining Shopify's architecture for context.

## Shopify's Architecture

Even though Shopify **moved to Docker** last year, they have **not moved away from a Monolithic application**. I asked Simon why, and he told me that they do not take the overhead of [Microservices](http://martinfowler.com/articles/microservices.html "Microservices explanation") lightly. However, this move has positioned them for an easier transition in the future if they ever decide to go that route.  

Anyway, their architecture looks something like this:  

A r**equest** goes to **Nginx** and then to a **cluster of servers running Docker with the Rails app**.  

For the **data tier**, they use things like:

*   Memcached
*   Redis
*   ElasticSearch
*   MySQL
*   Kafka
*   ZooKeeper

The majority of this **runs on their own hardware**. They do also run a **few things on AWS**.  

To reduce cost, Shopify **runs a multi-tenancy platform**. This means they have multiple stores on a single server -- shopA.com and shopB.com are served from the same server, for example.  

Even though the move to Docker [was not a smooth experience](https://www.youtube.com/watch?v=Qr0sATj9IVc "Simon Eskildsen on switching to Docker") here are some benefits they eventually got from it:  

Moving to Docker has enabled Shopify to **run Continuous Integration** on hundreds of thousands of lines of Ruby in about 5 minutes (15 minutes before Docker) and deploys to 300-400 nodes across 3 data centers take just 3 minutes (15 minutes before). These are impressive changes.

## How They Handle Spikes in Traffic

Ideally, the platform could handle spikes on its own. This is not completely implemented yet, so they run through a **performance checklist** before large sales.  

In Kanye West's example, they sat down for 2 weeks and did **extensive passive load testing and performance optimizations** by combing through critical parts of the platform.  

To run various tests, they use something called the Resiliency Matrix.  

![Resiliency Matrix used at Shopify](https://c2.staticflickr.com/6/5671/22090778534_a75acbb70b.jpg)  
(Image from [Simon's conference talk](https://speakerdeck.com/sirupsen/dockercon-2015-resilient-routing-and-discovery "Simon Eskildsen on Resilient Routing and Discovery and Dockercon 2015"))  

The **Resiliency Matrix** can help determine what happens to systems when a service goes down.  

Say Redis goes down, and Redis is used as an integral part of checkouts. Do you take the whole site down and put it into maintenance mode? No, you can just sign everyone out and still allow them to go through checkout without customer accounts. Then, once Redis is back up, associate email addresses with the customer account and fill in the gaps.  

Go down your list of systems (like storefronts, admin dashboards, API, etc...) and see what happens when a service goes down -- what other parts of your systems are effected? Try to **remove these dependencies and your application resiliency will drastically improve.** It's like a chain, where your application's strength is only as strong as the weakest link.  

![](https://d1ngwfo98ojxvt.cloudfront.net/images/blog/resiliency/chain.jpg)  

To help with this process, the Shopify team **open sourced Toxiproxy and Semian**.  

[Toxiproxy](https://github.com/Shopify/toxiproxy "Toxiproxy") introduces **controlled latency** in whatever system you choose.  

![Toxiproxy](https://d1ngwfo98ojxvt.cloudfront.net/images/blog/resiliency/toxiproxy.jpg)  

[Semian](https://github.com/Shopify/semian "Semian") is used to verify that you have **no single points of failure**.  

![Semian](https://d1ngwfo98ojxvt.cloudfront.net/images/blog/resiliency/semian.png)  

For more on resiliency, check out one of [Simon's conference talks](https://speakerdeck.com/sirupsen/dockercon-2015-resilient-routing-and-discovery "Simon Eskildsen on Resilient Routing and Discovery and Dockercon 2015") where he gives a lot more detail. It's super interesting.  

On top of this resiliency work, Shopify is also able to **over-provision since they own their own hardware**. This is cheap for them, but it may not be as cheap if you run on the cloud. Take a look at the cost versus benefit, and see if it makes sense for you.  

Another big challenge has been scaling the data store. Since Shopify handles financial transactions, their databases _cannot_ be out of sync. The solution? **Shopify sharded MySQL** about 2 years ago. They shard pretty aggressively and are aiming at having **more, and smaller, shards over time**.  

Simon went on to say that **scaling databases is hard** -- especially sharding. It almost always should be a last resort. You can probably cache a lot before you have to do it. But the good thing about shards is that they help isolate incidents. If something crazy is going on with a customer in a single shard, only a very small subset of the entire platform suffers from it.  

Going back to resiliency testing, Simon reinforces that most of their **database scaling issues have been fixed with a lot of resiliency work and automatic failovers**.

## What Other Improvements Are They Planning?

Going forward, the team is looking at **isolating tier**s that serve the applications from each other. Another big thing they are working on is getting shops to **run out of multiple different data centers on different continents at the same time**. This is big for data locality and also to protect against unexpected events.  

Protecting against unexpected events is also something Netflix has invested a substantial amount of resources in as Jeremy Edberg told me in [his interview](/interviews/interview/11 "Jeremdy").  

In addition to these plans, they are also looking at **making failovers something they could easily perform** multiple times a day. If you are curious about how they perform failover tests between entire data centers, refer to [Simon's interview page](/interviews/interview/14 "Simon Eskildsen interview with ScaleYourCode").  

As it stands, they cannot failover an entire data center without temporarily taking down checkouts. However, they are currently working on a solution to overcome this.

## Actions to Take

<div id="_mcePaste">My goal with this article is to give you easily-digestible knowledge you can take action on. What action(s) can you take now? Well, you probably want to avoid sharding, so can you cache more? You may not be able to over-provision due to cost, but what about checking out the resiliency matrix? Even if you don't have the resources to pull this off right now, building one of these matrices or even just thinking about it can be helpful.</div>

</div>