## [Leveraging Cloud Computing at Yelp - 102 Million Monthly Vistors and 39 Million Reviews](/blog/2013/6/26/leveraging-cloud-computing-at-yelp-102-million-monthly-visto.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, June 26, 2013 at 8:59AM</span></div>

<div class="body">

![](http://farm4.staticflickr.com/3695/9069678566_490be13289_q.jpg)

_<span>This is a guest post by <span>Yelp's</span> </span>[<span>Jim <span>Blomo</span></span>](https://twitter.com/jimblomo)<span>. Jim manages a growing data mining team that uses <span>Hadoop</span>, <span>mrjob</span>, and <span>oddjob</span> to process <span>TBs</span> of data. Before Yelp, he built infrastructure for startups and Amazon. </span>__Check out his upcoming talk at [<span><span>OSCON</span> 2013 on Building a Cloud Culture at Yelp</span>](http://www.oscon.com/oscon2013/public/schedule/detail/29387)._

In Q1 2013, Yelp had **102 million unique visitors** <span>(source: Google Analytics) including approximately 10 million unique mobile devices using the Yelp app on a monthly average basis. <span>Yelpers</span> ha<span>ve</span> written more than</span> **39 million rich, local reviews**, making Yelp the leading local guide on everything from boutiques and mechanics to restaurants and dentists. With respect to data, one of the most unique things about Yelp is the variety of data: reviews, user profiles, business descriptions, menus, check-ins, food photos... the list goes on.  We have many ways to deal data, but today I’ll focus on how we handle offline data processing and analytics.

<span>In late 2009, Yelp investigated using Amazon’s Elastic <span>MapReduce</span> (<span>EMR</span>) as an alternati<span>ve</span> to an in-house cluster built from spare computers.  By mid 2010, we had moved production processing completely to <span>EMR</span> and turned off our <span>Hadoop</span> cluster.  Today we run over</span> **500 jobs a day**<span>, from integration tests to advertising metrics.  We’<span>ve</span> learned a few lessons along the way that can hopefully benefit you as we<span>ll</span>.</span>

## Job Flow Pooling

One of EMR’s biggest advantages is instant scalability: every job flow can be configured with as many instances as needed for the task. But the scalability does not come for free. Themain drawbacks are 1) spinning up a cluster can take 5-20 minutes, and2) you are billed per hour **or fraction thereof**. This means if your job finishes in 2 hours 10 minutes, you are charged for the full three hours.

<span class="full-image-block ssNonEditable"><span>![](http://farm8.staticflickr.com/7384/9067232753_cc042d8f1f.jpg?__SQUARESPACE_CACHEVERSION=1371488974715)</span></span>

<span>This may not seem like a big deal, until you start running hundreds of jobs and the wasted time at the end of a job flow starts adding up. To decrease the amount of wasted billing hours, <span>mrjob</span> implements “job flow pooling.” Instead of shutting down a job flow at the end of a job, <span>mrjob</span> keeps the flow ali<span>ve</span> in case another job wants to use it. If another job comes along that has similar cluster requirements, <span>mrjob</span> wi<span>ll</span> reuse the job flow.</span>

<span class="full-image-block ssNonEditable"><span>![](http://farm4.staticflickr.com/3696/9067317383_62b31e2d38.jpg?__SQUARESPACE_CACHEVERSION=1371489153336)</span></span>

There are a few subtleties in implementing this: 1) what does it mean to have “similar cluster requirements”, 2) how to avoid race conditions between multiple jobs, and 3) when is the cluster finally shut down?

Similar job flows are defined meeting the criteria:

*   Same Amazon Machine Image (AMI) version
*   <span>Same <span>Hadoop</span> version</span>
*   <span>Same <span>mrjob</span> version</span>
*   <span>Same bootstrap steps (bootstrap steps can set <span>Hadoop</span> or cluster options)</span>
*   <span>Same or greater RAM and Compute Units for each node type (<span>eg</span>. <span>Hadoop</span> master <span>vs</span> workers)</span>
*   Accepts new jobs (job flows handle a max of 256 steps)

Race conditions are avoided by using locking. Locking is implemented using S3 **in the regions that support consistency**, US West by default. Jobs write to a specific S3 key with information about the job name and cluster type. Locks may timeout in the case of failures, which allow other jobs or the job terminator to reclaim the job flow.

<span>Job flow termination is handled by a <span>cron</span> job which runs every 5 minutes, checks for idle job flows that are about to</span> **hit their hourly charge, and terminates them**. This ensures that the worst case, never sharing job flows, is no more costly than the default.

<span>To further impro<span>ve</span> utilization of job flows, jobs can wait a predetermined amount of time to find a job flow it can re-use. For example, for development jobs, <span>mrjobs</span> wi<span>ll</span> try to find free job flows for 30 seconds before starting a new one. In production, jobs that don’t ha<span>ve</span> strict deadlines could set the wait time to several hours.</span>

We estimate around a **10% cost savings from using job flow pooling**<span>. From a developer’s perspecti<span>ve</span>, this cost saving has come almost for free: by setting a few <span>config</span> settings, these changes went into effect without any action from developers. In fact, we saw a side benefit: iterati<span>ve</span> development of jobs was sped up significantly since subsequent runs of a modified <span>MapReduce</span> job could reuse a cluster and the cluster startup time was eliminated.</span>

## Reserved Instances

<span>While <span>AWS</span> charges per machine hour by default, it offers a few other purchasing options that can reduce cost.</span> [Reserved instances](http://aws.amazon.com/ec2/reserved-instances/) <span>are one of the more straightforward options: pay money <span>upfront</span> to recei<span>ve</span> a lower per-hour cost. When comparing <span>AWS</span> prices to buying servers, I encourage people to investigate this option: it is a more fair comparison to the capital costs and commitments of buying servers.</span>

<span class="full-image-block ssNonEditable"><span>![](http://farm8.staticflickr.com/7309/9067325071_cde0cc3887.jpg?__SQUARESPACE_CACHEVERSION=1371489219527)</span></span>

<span>When is it cheaper overa<span>ll</span> to buy a reserved instance? It depends on how many hours an instance is used in a year. The plot abo<span>ve</span> shows the cost of running a large standard instance with the different reser<span>ve</span> instance pricing options: light, medium, or heavy usage. You can pay the on demand price, $0.26/hour, but after around 3000 hours (4 months) it becomes cheaper to cough up the $243 reser<span>ve</span> price and only pay $0.17/hour for light usage of a reserved instance. Using more than 3000 hours per year? Then its time to investigate the increased usage plans, with “heavy” being the largest <span>upfront</span>, but lowest per-hour plan.</span>

How many reserved instances should your company purchase? It depends on your usage. Rather than try to predict how much we **<span>wi<span>ll</span></span>** be using, we wrote a tool that analyzes **past** usage and recommends a purchasing plan that **<span>would ha<span>ve</span></span>** <span>saved us the most money. The assumption is that our future usage wi<span>ll</span> look similar to our past usage, and that the extra work and risk of forecasting was not worth the relatively sma<span>ll</span> amount of money it would sa<span>ve</span>. The tool is ca<span>ll</span></span> [<span><span>EMRio</span></span>](https://github.com/Yelp/EMRio) <span>and we open <span>sourced</span> it last year. It analyzes usage of <span>EMR</span> and recommends how many reserved instances to buy, as we<span>ll</span> as generates some pretty graphs.</span>

<span class="full-image-block ssNonEditable"><span>![](http://farm4.staticflickr.com/3759/9069555160_7c9dedc7a5.jpg?__SQUARESPACE_CACHEVERSION=1371489282122)</span></span>

It’s important to note that reserved instance pricing is a billing construct. That is, you are not physically reserving a machine. At the end of the month, Amazon simply looks at how many instance hours you’ve used and applies the reserved instance rates to any instances that were running, up to the number of instances purchased.

## Lessons Learned

**Understand the trade-offs when moving to a cloud solution**<span>. For Yelp, the primary benefit of using <span>AWS</span> has been multiplying developer productivity by decreasing coordination costs and feature latency. Coordination costs come from requiring product teams to forecast and request resources from the system team. Purchasing resources, be it racks of servers or network capacity, can take weeks and increases the latency of feature launches. Latency has its own associated costs of decreased moral (great developers lo<span>ve</span> shipping products) and context switching between projects. The dollar cost of <span>AWS</span> may be higher than a fully utilized, customized in-house solution, but the idea is you’<span>ve</span> bought much more productivity.</span>

**Focus on big wins**<span>: Incremental adoption of cloud technology is possible -- we’re doing it! Yelp started with <span>EMR</span> because it was our biggest win. Offline processing has spiky load characteristics, often does not require coordination between teams, and makes developers much more producti<span>ve</span> by providing leverage to their experiments. To best use the cloud, focus on solving your worst bottlenecks one at a time.</span>

**Build on abstractions**<span>: Don’t spin everyone up on a<span>ll</span> the details of a cloud service, just like you don’t spin everyone up on the details of your <span>datacenter</span>. Remember your trade-offs: the goal is to make developers much more producti<span>ve</span>, not be buzzword compliant. Having a scalable, adapti<span>ve</span> infrastructure doesn’t matter if developers can’t use it as easily as they would a local script. Our favorite abstraction is</span> [<span><span>mrjob</span></span>](https://github.com/Yelp/mrjob)<span>, which lets us write and run <span>MapReduce</span> jobs in Python. Running a job on a local machine <span>vs</span> an <span>EMR</span> cluster is a matter of changing two command line arguments.</span>

**Establish policies and integration plans**<span>: Spinning up a single instance is easy, but when do you spin down a machine? Processing a day’s worth of logs is <span>straightfoward</span>, but how do you reliably transfer logs to S3? Ha<span>ve</span> a plan for a<span>ll</span> of the supporting engineering that goes into making a system work: data integration, testing, backups, monitoring, and alerting. Yelp has policies around <span>PII</span>, separates production environments from development, and uses tools from the <span>mrjob</span> package to watch for run-away clusters.</span>

**Optimize after stability**<span>. There are many ways you can cut costs, but most of them require some complexity and future inflexibility. Make sure you ha<span>ve</span> a working, abstracted solution before pursuing them so you can evaluate <span>ROI</span>. Yelp wrote <span>EMRio</span> after we had several months of data on <span>EMR</span> usage with <span>mrjob</span>. Trying to optimize before seeing how we actually used <span>EMR</span> would ha<span>ve</span> been shooting in the dark.</span>

**<span>Evaluating <span>ROI</span></span>**: with some optimizations, cost evaluation is straightforward: how much would we have saved if we both reserved instances 2 months ago? Some are more difficult: what are the bottlenecks in the development process and can a cloud solution remove them? Easy or difficult, though, it is important to evaluate them before going into action. Profile code before optimizing it I assume you meant? If not I'd love to check out this profiler you're using :)

## Future Directions

As Yelp grows its service oriented architecture, we are running into similar bottlenecks encountered for offline batch processing: coordination of resources, testing ideas, forecasting usage before launching new features. Therefore we’re again looking at the cloud to provide big wins for services like ad selection, search, and data ingestion. [Look forward to future posts](http://engineeringblog.yelp.com/) on the Yelp engineering blog about leveraging [<span><span>Asgard</span></span>](http://netflix.github.io/asgard/) <span>to build and deploy services, evaluating Python frameworks to ser<span>ve</span> <span>RESTful</span> <span>APIs</span>, and building abstractions to make it a<span>ll</span> easy to use. Of course, if you’d like to help build this future,</span> [let us know!](http://www.yelp.com/careers)

</div>