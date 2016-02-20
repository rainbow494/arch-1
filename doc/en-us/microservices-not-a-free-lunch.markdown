## [Microservices - Not a free lunch!](/blog/2014/4/8/microservices-not-a-free-lunch.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, April 8, 2014 at 8:54AM</span></div>

<div class="body">

![](https://farm4.staticflickr.com/3697/13651792713_817bfe2010_m.jpg)

_This is a guest post by [Benjamin Wootton](https://twitter.com/benjaminwootton), CTO of [Contino](http://contino.co.uk/), a London based consultancy specialising in applying DevOps and Continuous Delivery to software delivery projects. _

Microservices are a style of software architecture that involves delivering systems as a set of very small, granular, independent collaborating services.

Though they aren't a particularly new idea, Microservices seem to have exploded in popularity this year, with articles, conference tracks, and Twitter streams waxing lyrical about the benefits of building software systems in this style.

This popularity is partly off the back of trends such as Cloud, DevOps and Continuous Delivery coming together as enablers for this kind of approach, and partly off the back of great work at companies such as Netflix who have very visibly applied the pattern to great effect.

Let me say up front that I am a fan of the approach. Microservices architectures have lots of very real and significant benefits:

*   The services themselves are very simple, focussing on doing one thing well;
*   Each service can be built using the best and most appropriate tool for the job;
*   Systems built in this way are inherently loosely coupled;
*   Multiple developers and teams can deliver _relatively_ independently of each other under this model;
*   They are a great enabler for continuous delivery, allowing frequent releases whilst keeping the rest of the system available and stable.

This said, Microservices are not a free lunch!

I am currently involved in architecting a system based around Microservices, and whilst the individual services are very simple, a lot of complexity exists at a higher level level in terms of managing these services and orchestrating business processes throughout them.

Microservices one of these ideas that are nice in practice, but all manner of complexity comes out when it meets reality. For this reason, I wanted to write this article to capture some of these and redress the balance.

# Significant Operations Overhead

A Microservices architecture brings a lot of operations overhead.

Where a monolithic application might have been deployed to a small application server cluster, you now have tens of separate services to build, test, deploy and run, potentially in polyglot languages and environments.

All of these services potentially need clustering for failover and resilience, turning your single monolithic system into, say, 20 services consisting of 40-60 processes after we've added resilience.

Throw in load balancers and messaging layers for plumbing between the services and the estate starts to become pretty large when compared to that single monolithic application that delivered the equivalent business functionality!

Productionising all of this needs high quality monitoring and operations infrastructure. Keeping an application server running can be a full time job, but we now have to ensure that tens or even hundreds of processes stay up, don't run out of disk space, don't deadlock, stay performant. It's a daunting task.

Physically shipping this plethora of Microservices through your pipeline and into production also needs a very high degree of robust and release and deployment automation.

Currently, there is not much in terms of frameworks and open source tooling to support this from an operational perspective. It's likely therefore that a team rolling out Microservices will need to make significant investment in custom scripting or development to manage these processes before they write a line of code that delivers business value.

Operations is the most obvious and commonly held objection towards the model, though it is too easily brushed aside by proponents of this architecture.

## Substantial DevOps Skills Required

Where a development team might have been able to bring up, say, a Tomcat cluster and keep it available, the operations challenges of keeping Microservices up and available mean you definitely need high quality DevOps and release automation skills embedded within your _development_ team.

You simply can't throw applications built in this style over the wall to an operations team. The development team need to be very operationally focussed and production aware, as a Microservices based application is very tightly integrated into it's environmental context.

Idiomatic use of this architecture also means that many of the services will also need their own data stores. Of course, these could also be polyglot (the right tool for the job!), which means that the venerable DBA now needs to be replaced with developers who have a good understanding of how to deploy, run, optimise and support a handful of NoSQL products.

Developers with a strong DevOps profile like this are hard to find, so your hiring challenge just became an order of magnitude more difficult if you go down this path.

## Implicit Interfaces

As soon as you break a system into collaborating components, you are introducing interfaces between them. Interfaces act as contracts, with both sides needing to exchange the same message formats and having the same semantic understand of those messages.

Change syntax or semantics on one side of the contract and all other services need to understand that change. In a Microservices environment, this might mean that simple cross cutting changes end up requiring changes to many different components, all needing to be released in co-ordinated ways.

Sure, we can avoid some of these changes with backwards compatibility approaches, but you often find that a business driven requirements prohibit staged releases anyway. Releasing a new product line or an externally mandated regulatory change for instance can force our hand to release lots of services together. This represents additional release risk over the alternative monolithic application due to the integration points.

If we let collaborating services move ahead and become out of sync, perhaps in a canary releasing style, the effects of changing message formats can become very hard to visualise.

Again, bckwards compatibility is not a panacea here to the degree that Microservices evangelists claim.

## Duplication Of Effort

Imagine that there is a new business requirement to calculate tax differently for a certain product line. We have a few choices in how to deliver this.

We could introduce a new service and allow the other services to call into this where needed. That does however introduce more potentially synchronous coupling into the system, so is not a decision we would take lightly.

We could duplicate the effort, adding the tax calculation into all of the services that need it. Besides the duplicated development effort, repeating ourselves in this way is generally considered a bad idea as every instance of the code will need to be tested and maintained going forward.

The final option is to share resources such as a tax calculating library between the services. This can be useful, but it won't always work in a polyglot environment and introduces coupling which may mean that services have to be released in parallel to maintain the implicit interface between them. This coupling essentially mitigates a lot of the benefits of Microservices approaches.

It seems to me that all three of these options are sub-optimal as opposed to writing the piece of code once and making it available throughout the monolithic application. The teams I have seen working in this style tend towards option 2, duplicating of business logic, which goes against many principles of good software engineering. And yes, this even takes place in well decomposed and designed systems - it's not always a sign of bad service boundaries.

## Distributed System Complexity

Microservices imply a distributed system. Where before we might have had a method call acting as a subsystem boundary, we now introduce lots of remote procedure calls, REST APIs or messaging to glue components together across different processes and servers.

Once we have distributed a system, we have to consider a whole host of concerns that we didn't before. Network latency, fault tolerance, message serialisation, unreliable networks, asynchronicity, versioning, varying loads within our application tiers etc.

Coding for some of these is a good thing. Backwards compatibility and graceful degradation are nice properties to have that we might not have implemented within the monolithic alternative, helping keep the system up and more highly available than the monolithic application would be.

The cost of this however is that the application developer has to think about all of these things that they didn't have to before. Distributed systems are an order of magnitude more difficult to develop and test against, so again the bar is raised vs building that unsexy monolithic application.

## Asynchronicity Is Difficult!

Related to the abovei point, systems built in the Microservices style are likely to be much more asynchronous than monolithic applications, leaning on messaging and parallelism to deliver their functionality.

Asynchronous systems are great when we can decompose work into genuinely separate independent tasks which can happen out of order at different times.

However, when things have to happen synchronously or transactionally in an inherently Asynchronous architecture, things get complex with us needing to manage correlation IDs and distributed transactions to tie various actions together.

## Testability Challenges

With so many services all evolving at different paces and different services rolling out canary releases internally, it can be difficult to recreate environments in a consistent way for either manual or automated testing. When we add in asynchronicity and dynamic message loads, it becomes much harder to test systems built in this style and gain confidence in the set of services that we are about to release into production.

We can test the individual service, but in this dynamic environment, very subtle behaviours can emerge from the interactions of the services which are hard to visualise and speculate on, let alone comprehensively test for.

Idiomatic Microservices involves placing less emphasis on testing and more on monitoring so we can spot anomalies in production and quickly roll back or take appropriate action. I am a big believer in this approach - lowing the barriers to release and leaning continuous delivery in order to speed up lean delivery. However, as someone who has also spent years applying test automation to gain confidence prior to release, anything that reduces this capability feels like a high price to pay, especially in risk averse regulated environments where bugs can have significant repercussions.

## To Conclude

These are some of the difficulties I am seeing during the early phases of building and running a Microservices based system.

I am still a fan of the approach and believe that on the right project with the right team it is a wonderful architecture to adopt where the benefits outweigh the costs above.

However, when considering Microservice like architectures, it's really important to not be attracted to the hype on this one as the challenges and costs are as real as the benefits.

## Related Articles

*   [On Hacker News](https://news.ycombinator.com/item?id=8805260)

</div>