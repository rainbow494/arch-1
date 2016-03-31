## [HappyPancake: a Retrospective on Building a Simple and Scalable Foundation](/blog/2015/2/23/happypancake-a-retrospective-on-building-a-simple-and-scalab.html)

    

    

![](https://farm8.staticflickr.com/7428/16409833089_9b2fb87c4d_m.jpg)

_This is a guest repost by [Rinat Abdullin](http://abdullin.com/about-me/), who worked on _[HappyPancake](http://www.happypancake.com/),_ the largest free dating site in Sweden. Initially written in ASP.NET and MS SQL Database server, it eventually became overly complex and expensive to scale. This is the last post in a nearly two year long series of engaging articles on the evolution of the project. For the complete list please see the end of this article._

Our project at HappyPancake completed this week. We delivered a simple and scalable foundation for the **next version of largest free dating web site in Sweden** (with presence in Norway and Finland).

## Journey

Below is the short map of that journey. It lists technologies and approaches that we evaluated for this project. Yellow regions highlight items which made their way into the final design.

        ![](https://farm8.staticflickr.com/7327/16408255448_c70d47bcba_b.jpg?__SQUARESPACE_CACHEVERSION=1424475098086)        

## Project Deliverables

**Project deliverables** included:

*   Deployable full-stack application with major features implemented.
*   Domain model captured in software design (back-end and front-end) and a set of declarative use-cases (acting as living documentation, system outline and behavior test suite).
*   Configured environments for development and continuous integration (docker container).
*   Strategies for further evolution and scaling of the system.
*   Code for migrating existing production deployments to a new version of software.

Final high-level **architecture is simple to reason about and scale**. It was designed to be that way.

        ![](https://farm8.staticflickr.com/7381/16594300251_e6cf0ef8ec_z.jpg?__SQUARESPACE_CACHEVERSION=1424475159331)        

Logically the entire solution consists from backend modules (represented by golang packages) and elements of Facebook Flux architecture (grouped together in namespaces by naming conventions).

Such structure helps to maintain the project as it grows in size and complexity.

        ![](https://farm9.staticflickr.com/8598/16594306631_2fa4cd8460_o.jpg?__SQUARESPACE_CACHEVERSION=1424475207796)        

This design also helps to scale the deployment to handle higher loads.

We can **scale backend** by:

*   moving individual modules to bigger servers;
*   launching multiple instances of a single module;
*   switching storage of an individual module to clustered solution, moving it to bigger servers or even pushing to the cloud.

We can **scale frontend** by simply launching new instances behind the load balancer.

        ![](https://farm8.staticflickr.com/7289/16569507946_122a378ee4_z.jpg?__SQUARESPACE_CACHEVERSION=1424475269173)        

Solution structure also provides a natural way to split the work between the developers. Given the established published language (contracts of API and events), we can also bring in more developers, assigning them to work on individual backend modules or frontend namespaces.

## Lessons Learned

*   Picking the right technology can reduce the development effort.
*   In my next project I'll try to focus even more on divide and conquer approach - isolate a small part first and then evolve it, limiting the amount of work in progress.
*   It is crucial to establish feedback loop as early as possible, involving all stake-holders. This builds trust and helps to avoid surprises.

## Related Articles

*   [On Hacker News](https://news.ycombinator.com/item?id=9101133)
*   The complete HappyPancake article series. Just the titles give you an indication of how the project zig zagged over time. [Introduction](http://abdullin.com/happypancake/intro/); [New Team](http://abdullin.com/happypancake/2013-12-17/); [Language is an Implementation Detail](http://abdullin.com/happypancake/2013-12-23/); [Moving Forward with Golang](http://abdullin.com/happypancake/2014-01-18/); [Starting with FoundationDB](http://abdullin.com/happypancake/2014-02-02/); [Evolving the Stack and learning Nanomsg](http://abdullin.com/happypancake/2014-02-08/); [Designing for Throughput and Low Latency](http://abdullin.com/happypancake/2014-02-17/); [Containers, virtualization and clusters](http://abdullin.com/happypancake/2014-02-24/); [Benchmarking and tuning the stack](http://abdullin.com/happypancake/2014-03-19/); [Change of Plans](http://abdullin.com/happypancake/2014-04-07/); [Back to Basics](http://abdullin.com/happypancake/2014-04-14/); [Messaging - Heart of a Social Site](http://abdullin.com/happypancake/2014-04-21/); [Event-driven week](http://abdullin.com/happypancake/2014-04-28/); [Reactive Prototype](http://abdullin.com/happypancake/2014-05-05/); [Tactical DDD](http://abdullin.com/happypancake/2014-05-12/); [Emergend Design Faces Reality](http://abdullin.com/happypancake/2014-05-24/); [Minimal Viable Product](http://abdullin.com/happypancake/2014-06-01/); [Almost Demo](http://abdullin.com/happypancake/2014-06-09/); [Our First Demo](http://abdullin.com/happypancake/2014-06-13/); [Scala, Modular Design and RabbitMQ](http://abdullin.com/happypancake/2014-06-30/), [Distributing Work](http://abdullin.com/happypancake/2014-07-06/); [Smarter Development](http://abdullin.com/happypancake/2014-07-21/); [Delivering Features and Tests](http://abdullin.com/happypancake/2014-07-29/); [Data, Use Cases And New Module](http://abdullin.com/happypancake/2014-08-02/); [Back from the Vacation](http://abdullin.com/happypancake/2014-08-16/); [Native Performance](http://abdullin.com/happypancake/2014-08-25/); [Features, Use Cases, Rendr](http://abdullin.com/happypancake/2014-09-07/); [Getting started with Node.js, Lazojs](http://abdullin.com/happypancake/2014-09-15/); [Web development, the good parts](http://abdullin.com/happypancake/2014-09-23/); [Reactive User Experience](http://abdullin.com/happypancake/2014-09-29/); [Feeds, Chat, Online list and CSS](http://abdullin.com/happypancake/2014-10-07/); [To ReactJS and Facebook Flux](http://abdullin.com/happypancake/2014-10-27/); [Project Complete](http://abdullin.com/happypancake/2014-11-06/).

    