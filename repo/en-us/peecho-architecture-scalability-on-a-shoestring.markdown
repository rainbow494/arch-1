## [Peecho Architecture - scalability on a shoestring](/blog/2011/8/1/peecho-architecture-scalability-on-a-shoestring.html)

    

    

_This is a guest post by [Marcel Panse](http://www.linkedin.com/in/marcelpanse "Marcel Panse") and [Sander Nagtegaal](http://www.linkedin.com/in/centrical "Sander Nagtegaal") from [Peecho](http://www.peecho.com)._

Although architecture descriptions are an interesting read, the problems that start-ups face are hardly ever addressed. We would like to change that, so here is our architecture story.

## Introducing a start-up

![Peecho](http://www.peecho.com/storage/images/penguins/100/penguin.png)

The Amsterdam-based company [Peecho](http://www.peecho.com) offers print-as-a-service. Our embeddable _print button_ allows you to sell your digital content as professionally printed products, like photo books, magazines or canvases - straight from your own website. There is an API, too.

Printcloud is the system that powers the print button. It exists in the cloud only, growing when needed and becoming smaller if it can. The system takes in print orders, magically transforms tough data into print-ready files and routes the orders to the production facility that is closest to the intended recipient.

To preserve the environment, Peecho's philosophy is to facilitate global ordering, but to aim for local production only.

## Expensive stuff does not scale

We are a start-up, so the most important thing that we considered before we started was simply _money_ - or rather, the lack thereof. Although we required some serious firepower, the fully operational system should cost no more than a few hundred bucks a month.

The money issue reached all the way into our technology stack. We could insert something cool here about object-orientation, code compilation or MVC architectures - but in the end, we just needed an extensive development toolkit that wouldn't cost us. So, the computer science background of our developer community members pointed us to the Java platform.

Using cloud computing was a dead give-away, because on demand scaling eliminates expensive overcapacity. Our shortlist contained [Google App Engine](http://code.google.com/appengine/ "GAE") and [Amazon Web Services](http://aws.amazon.com "AWS"). These molochs have way more resources and experience with scalability than we do. That's why we vowed to use as many of their cloud services as possible, rather than building stuff ourselves. However, we needed queueing and relational data storage - exit Google.

So, we chose to build on top of Amazon Web Services, using SimpleDB, S3, SQS, EC2, RDS, IAM, Route 53 and Cloudfront. It is cheap and generally reliable. Just to be sure, our entire system runs simultaneously in two AWS availability zones to guarantee maximum uptime. If we go down, half of the world will be there with us.

## If you are slow, you can't grow

However, technology is not everything. Scalability for small companies is largely obtained by flexibility. Be fast. We try to keep our code simple, so refactoring is easy. As a result, we can afford to only build the bare minimum that is needed without thinking much about the future needs. We can fix those later.

If you really want to be fast, short iterations are important. On average, we release new software to our production environment about once a week. To keep up with that pace, we organize a monthly dinner. That's when demos are shown and everybody cheers and applauds for the new stuff.

There is a lot of affordable help available to get you up to speed. For example, we develop according to [Scrum](http://en.wikipedia.org/wiki/Scrum_(development) "Scrum") and [Test Driven Development](http://en.wikipedia.org/wiki/Test-driven_development "TDD"). We use [Jira](http://www.atlassian.com/software/jira/ "Jira") with [Greenhopper](http://www.atlassian.com/software/greenhopper/ "Greenhopper") for requirements management, [Bamboo](http://www.atlassian.com/software/bamboo/ "Bamboo") for continuous integration, [Sonar](http://www.sonarsource.org/ "Sonar") for code quality monitoring and [Mercurial](http://mercurial.selenic.com/ "Mercurial") for distributed versioning.

By the way, the large number of Atlassian products is partly explained by their _cheap start-up licenses_ - but it is good stuff anyway.

## Sure, but what does the architecture look like?

The print button and its checkout exist as a separate application on top of our platform. This separation of concerns is meant to minimize the interaction points - to achieve high cohesion and low coupling. The app runs on an EC2 auto-scaling group with a load balancer on top. It stores order data in SimpleDB, an AWS No-SQL data store that can take a beating. Everything static is run through the Cloudfront CDN, to avoid server hits.

![Peecho Printcloud architecture: print button](http://www.peecho.com/storage/images/architecture/1-architecture-3.png)

After payment has been confirmed, the print button checkout submits every order to a REST API. This is the same API that our premium users access from their apps, so we only have to maintain a single interface. It is load-balanced to multiple nodes that can scale out automatically. Using the relational database service RDS, order data is kept in two availability zones.

![Peecho Printcloud architecture: order intake](http://www.peecho.com/storage/images/architecture/2-architecture-3.png)

Now, this is where the magic happens. The order intake machine writes a ticket to the processing queue. Whenever there are enough tickets available, a new processing machine wakes up, gets a ticket and starts crunching that awful data. We use large [spot instances](http://aws.amazon.com/ec2/spot-instances/ "spot instances") that we rent by bidding on unused EC2 capacity. When those big guns are done, they store the result in S3 and then kill themselves.

![Peecho Printcloud architecture: processing](http://www.peecho.com/storage/images/architecture/3-architecture-3.png)

So, using queue metrics, we scale the amount of processing power up and down as the number of items in the queue varies. This saves money and prepares us for unexpected outages. Before you do this yourself, keep in mind that each component or module should be responsible for a specific feature or functionality only. A component or object should not know about internal details of other components or objects.

Subsequently, the order must be routed to the right production facility by adding a production ticket to the right print facility queue. Using either our Adobe AIR Printclient desktop application or a direct integration, the facility retrieves its jobs from the queue and the product files from S3 file storage. There are no servers involved, so this system is practically bullet-proof.

![Peecho Printcloud architecture: Printclient](http://www.peecho.com/storage/images/architecture/4-architecture-3.png)

In return, the facility can update job statuses in Printcloud by posting messages to the central order status queue. Asynchronously, we read that queue to update our customers about their orders. If all goes well, the product is shipped and delivered pretty soon.

After a certain period of time, the product files will be removed from S3 to save storage costs - and everybody lives happily ever after.

## Doing the math

Let's give you a quick insight into our monthly AWS bill. There are a couple of things that you should know.

*   Amazon offers free tiers for SimpleDB and other services.
*   We use the European region cluster, which is more expensive than those in the United States.
*   The EC2 instances are [reserved for a year](http://aws.amazon.com/ec2/reserved-instances/ "reserved EC2 instances"), making them cheaper.
*   Spot instances are generally a bargain and we assume a continuous 50% workload.
*   Just like the number of concurrent EC2 nodes, required bandwidth is largely determined by the amount of orders coming in.

<table>

<thead>

<tr>

<th>Task</th>

<th>Service</th>

<th>Cost estimate</th>

</tr>

</thead>

<tbody>

<tr>

<td>Print button</td>

<td>Multi-AZ EC2 small instances</td>

<td>$ 96</td>

</tr>

<tr>

<td>Order intake</td>

<td>Multi-AZ EC2 small instances</td>

<td>$ 96</td>

</tr>

<tr>

<td>Processing</td>

<td>EC2 large spot instances</td>

<td>$ 84</td>

</tr>

<tr>

<td>Database</td>

<td>Multi-AZ RDS</td>

<td>$ 160</td>

</tr>

<tr>

<td>The rest</td>

<td>Cloudfront, S3, SimpleDB, SQS, etc.</td>

<td>$ 50</td>

</tr>

</tbody>

</table>

That makes a total of _486 dollars_ a month for a fully elastic, heavy duty system that has the power of a minimum of 5 servers, 2 database servers, a No-SQL data store and external flat file storage.

Not bad, we think.

## We wish you cheap apps

Peecho proves that it is possible to create scalable architectures on a shoestring. We hope this information was useful to you and that it will help you to create great apps for next to nothing, too. Preferably with a print button, of course.

    