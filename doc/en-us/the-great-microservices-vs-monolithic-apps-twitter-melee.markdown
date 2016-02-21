## [The Great Microservices vs Monolithic Apps Twitter Melee ](/blog/2014/7/28/the-great-microservices-vs-monolithic-apps-twitter-melee.html)

    

    

![](https://farm4.staticflickr.com/3926/14568159437_fe66e0bc3e_n.jpg) Once upon a time a great [Twitter melee](https://twitter.com/adrianco/status/441169921863860225) was fought for the coveted title of _Consensus Best Way to Structure Systems._ The competition was between Microservices and Monolithic Apps. 

Flying the the logo of Microservices, from a distant cloud covered land, is the Kingdom of Netflix, whose champion was Sir [Adrian Cockcroft](https://www.linkedin.com/in/adriancockcroft) (who has pledged fealty to another). And for the Kingdom of ThoughtWorks we have Sir [Sam Newman](https://twitter.com/samnewman) as champion.

Flying the logo of the Monolithic App is champion Sir J[ohn Allspaw](https://www.linkedin.com/in/jallspaw), from the fair Kingdom of Etsy.

    Knights from the Kingdom of Digital Ocean and several independent realms filled out the list.    

    To the winner goes a great prize: developer mindshare and the favor of that most fickle of ladies, Lady Luck.    

    May the best paradigm win.    

    The opening blow was wielded by the highly ranked Sir Cockcroft, a veteran of many tournaments:    

>     adrian cockcroft ‏@adrianco  Mar 5    
> 
>     Etsy at #qconlondon make it clear to me why monolithic apps are a dead end. Use microservices for continuous scalable deployments    
> 
>     Neil Bartlett ‏@nbartlett  Mar 5    
> 
>     @adrianco Yes, +1000 for this. Though reasonable people can disagree about how those microservices are to be implemented /cc @AnneWoof             
> 
>     adrian cockcroft ‏@adrianco  Mar 5    
> 
>     @nbartlett @AnneWoof monolithic apps force everyone into the same decisions. Microservice freedom to innovate and optimize as needed    
> 
>     John Allspaw ‏@allspaw  Mar 5    
> 
>     ‏@adrianco your critical thinking is lacking on this one, because you're unable to imagine all benefits.    
> 
>     Sam Newman ‏@samnewman  Mar 5    
> 
>     @allspaw @adrianco generically, microservices give you more options about how to get things done             
> 
>     John Allspaw ‏@allspaw  Mar 5    
> 
>     @samnewman and more/different constraints. @adrianco             
> 
>     John Allspaw ‏@allspaw  Mar 5    
> 
>     @samnewman sometimes, less options can open opportunities. A few number of well-understood tools and patterns brings advantages @adrianco    
> 
>     John Allspaw ‏@allspaw  Mar 5    
> 
>     @samnewman and microservices can also be an alias for "I want to get my way without having to debate the merits of my decisions." @adrianco    
> 
>     Sam Newman ‏@samnewman  Mar 5    
> 
>     @allspaw @adrianco it can give more freedom - it actually shifts where you standardise, and where you allow for free choice    
> 
>     Sam Newman ‏@samnewman  Mar 5    
> 
>     @allspaw @adrianco standardisation between services is vital (monitoring, integration). Inside the box? Give the team autonomy    
> 
>     Sam Newman ‏@samnewman  Mar 5    
> 
>     @allspaw @adrianco all this relies on having a good description of what a ‘good citizen’ is for your organisation    
> 
>     Mark Burgess ‏@markburgess_osl  Mar 5    
> 
>     @samnewman @allspaw @adrianco The key is: can you measure diffs between these approaches in user/provider experience fitness for purpose etc    
> 
>     Mark Burgess ‏@markburgess_osl  Mar 5    
> 
>     @samnewman @allspaw @adrianco Once you've identified who benefits from what, there is a policy weighted optimization to consider.    
> 
>     Mark Burgess ‏@markburgess_osl  Mar 5    
> 
>     @samnewman @allspaw @adrianco This was an interesting (and very polarized) discussion!             
> 
>     adrian cockcroft ‏@adrianco  Mar 6    
> 
>     @allspaw Netflix was monolithic app for the first 3 years I worked there. As the team scaled beyond 100 engineers concerns were separated    
> 
>     adrian cockcroft ‏@adrianco  Mar 6    
> 
>     @samnewman @allspaw centrally planned and coordinated things don't scale and don't innovate as fast as high trust freedom and responsibility    
> 
>         
> 
>     John Allspaw ‏@allspaw  Mar 6    
> 
>     @adrianco We have high trust, freedom responsibility at Etsy with our architecture and process. Conway's Law is not a law. /cc @samnewman    
> 
>     John Allspaw ‏@allspaw  Mar 6    
> 
>     @adrianco I'm saying that you are ignoring the possibility that you're wrong in your application of absolutist assertions to Etsy.    
> 
>     John Allspaw ‏@allspaw  Mar 6    
> 
>     @adrianco Who said Etsy was centrally planned? Your mental model of Etsy's development culture is flawed. You don't sound even curious.    
> 
>     adrian cockcroft ‏@adrianco  Mar 6    
> 
>     @allspaw sorry, I'm not sure what assertion you think I'm making. I'm discussing alternatives to scaling a monolith like Etsy or Facebook    
> 
>     John Allspaw ‏@allspaw  Mar 6    
> 
>     @adrianco I didn't hear "alternative", I heard "doesn't work", "dead", and "microservices are in all ways superior". Apologies if incorrect.    
> 
>     adrian cockcroft ‏@adrianco  Mar 6    
> 
>     @allspaw I went to both Etsy talks yesterday. Monolith in php prevents implementation of features in other languages, e.g. clojure.             
> 
>     John Allspaw ‏@allspaw  Mar 6    
> 
>     @adrianco Yep. And microservices in clojure prevents reaping advantages in monolith php. Engineering is trade-offs amongst many influences.             
> 
>     John Allspaw ‏@allspaw  Mar 6    
> 
>     @adrianco and I've been working there for >4 years. I'm telling you you're missing context, that is all. :)             
> 
>     adrian cockcroft ‏@adrianco  Mar 6    
> 
>     @allspaw I guess someone from Etsy should do a talk on the advantages of a monolith, but I don't see how adding indep team prevents innov    
> 
>     Alan ‏@AlanMorrison  Mar 6    
> 
>     @adrianco @allspaw The right level of services should be in between micro and macro, e.g., steps in a process, yes? http://bit.ly/1nha966    
> 
>     John Allspaw ‏@allspaw  Mar 6    
> 
>     @adrianco a fine idea.    
> 
>     Adam Thody ‏@thody  Mar 6    
> 
>     @allspaw @adrianco a fine, and long overdue debate gentlemen. In software and in orgs there's little room for dogmatism.    
> 
>     John Allspaw ‏@allspaw  Mar 6    
> 
>     @adrianco a summing up of my thoughts: https://gist.github.com/anonymous/9388472 … /cc @kellan    
> 
>     1\. Monolithic applications and architectures  can vary in their monolithness. This is an under-specified description.    
> 
>     2\. Microservice applications and architectures can vary in their microness. This is an under-specified description.    
> 
>     3\. Microservices and monolithic architectures have both benefits and disadvantages.    
> 
>     4\. Organizations will exploit those benefits while working around any weaknesses.    
> 
>     5\. Success of the business is a large influence on the exploitation of benefits and implementation and costs of workarounds.    
> 
>     6\. All benefits and work arounds are context-sensitive. Meaning that they are both technically and socially constructed by the organization that navigates them.    
> 
>     7\. Path dependency is a thing. History matters and manifests in these architectural decisions and evolution in an organization.    
> 
>     8\. Patterns exist to inform practice, not dictate it. Zealous adherence to an architectural pattern brings peril when it is to the exclusion of cultural context in actual practice.    
> 
>     9\. Architectural patterns will expand, contract, evolve, and change to fit the trade-offs that an organization perceives it has to make    
> 
>     Kevin Behr ‏@kevinbehr  Mar 6    
> 
>     @allspaw @adrianco great fun. I read this thread and said "I love variation and adaptation" out loud.             
> 
>     Sam Newman ‏@samnewman  Mar 6    
> 
>     @allspaw @adrianco I have found conways law to prove true more than have it prove false! I use it as guidance though, not hard constraint    
> 
>     Sam Newman ‏@samnewman  Mar 6    
> 
>     @allspaw @adrianco trust leading to autonomy are the key - many ways to get there. The right architecture counts!    
> 
>     Sam Newman ‏@samnewman  Mar 6    
> 
>     @allspaw @adrianco where standardisation is required, I try to make it easy/transparent, eg provide tools to do the right thing             
> 
>     adrian cockcroft ‏@adrianco  Mar 6    
> 
>     @allspaw @kellan agree with those, but have some to add. A micro service arch could be seen as a collection of small monoliths.             
> 
>     adrian cockcroft ‏@adrianco  Mar 6    
> 
>     @samnewman @allspaw inverse application of conways law helps influence the architectures that gets built    
> 
>     Steve Smith ‏@AgileSteveSmith  Mar 6    
> 
>     @samnewman I like to see standards gradually emerging as a byproduct of success, and be baseline for future improvement /@allspaw @adrianco    
> 
>     Anthony Elizondo ‏@complex  Mar 6    
> 
>     @adrianco @allspaw @kellan depends on your pov. the provider sees monolith, to consumer it is micro, one of many.    
> 
>     Sam Newman ‏@samnewman  Mar 6    
> 
>     @AgileSteveSmith @allspaw @adrianco I’ve seen this reflected in incremental changes to internal tool chains and service templates             
> 
>     Sam Newman ‏@samnewman  Mar 6    
> 
>     @AgileSteveSmith @allspaw @adrianco generally agree, but some things may have to be decided upfront,eg measures to avoid cascading failures    

    As you can see many blows were landed. And while blood spilled freely onto the field, no wounds proved mortal.    

    Then the herald announced free beer and t-shirts out in the market, so no winner could be declared. The battle would continue another day...perhaps every generation must fight this same fight.    

    As your bard I feel it necessary to explain what point of honor demanded this clanging exchange of blows.    

##     Monolithic Apps are Considered an Anti-Pattern             

    It is written in     [    Development, Deployment and Collaboration at Etsy    ](https://speakerdeck.com/mrtazz/development-deployment-and-collaboration-at-etsy)     that “At Etsy about 150 engineers deploy a single monolithic application more than 60 times a day.” A     [    monolithic application    ](http://cantina.co/monolithic-architecture-doesnt-scale/)     is where “all of the required logic is located within one ‘unit’ (a war, a jar, a single application, one repository).”    

Etsy is [successful](http://www.entrepreneur.com/article/235364) as a company and it is not a small site, as of February 2013: 1.49 billion page views, 4,215,169 items sold, $94.7 million of goods sold, 22+ million members, 800,000+ active shops.

    Etsy also serves as a fine example of how to build a great site and do things right: continuous integration; per developer VMs; push button deployment; good monitoring; developers deploy to the site on the first day; GitHub; Chef; IRC to control releases; dashboards; and they do not use source code control branches, effectively always deploying from the main branch.    

    So how could Etsy still use a monolithic image?    

    The sense of disbelief is because monolithic applications are considered an anti-pattern. Read more about why in     [    Monolithic Architecture Doesn’t Scale!    ](http://cantina.co/monolithic-architecture-doesnt-scale/)     The gist of it is that scale here refers to the ability of  a large group of developers to successfully change, test, and release code, not scale as in how many requests per second a system handles.      

    The old saying applies: Too many cooks in the kitchen spoil the broth.    

    We all know the way to make a great broth is to divide the kitchen into many kitchenettes, each with their own cook, staff, supplies, and equipment, and then have the hungry customer coordinate all the separate cooks in how to make a tasty broth. Now there's a recipe for becoming a Chopped Champion.    

    Which leads us to microservices…    

##     Microservices are Awesomesauce             

    One solution to the monolithic application problem is to break up an application into microservices. Martin Fowler     [    explains    ](http://martinfowler.com/articles/microservices.html)    :    

>     The term "microservice" was discussed at a workshop of software architects near Venice in May, 2011 to describe what the participants saw as a common architectural style that many of them had been recently exploring. In May 2012, the same group decided on "microservices" as the most appropriate name. James presented some of these ideas as a case study in March 2012 at 33rd Degree in Krakow in     [    Microservices - Java, the Unix Way    ](http://2012.33degree.org/talk/show/67)     as did Fred George     [    about the same time    ](http://www.slideshare.net/fredgeorge/micro-service-architecure)    . Adrian Cockcroft at Netflix, describing this approach as "fine grained SOA" was pioneering the style at web scale as were many of the others mentioned in this article - Joe Walnes, Dan North, Evan Botcher and Graham Tackley    

    For a tight explanation of microservices in action listen to this interview with Sir Cockroft at the     [    DevOps Cafe    ](https://www.youtube.com/watch?v=dAOkEnBS498)    . The microservices section starts about 30 minutes in.                 My gloss on the talk is:    

*       Optimizing for speed causes developers to integrate code into a monolithic app. Getting a monolithic app to work is hard. When one feature breaks and must be rolled back that means the other features released at the same time must also be backed out.    

*       Once more than 100 developers work on a project a threshold is reached where building a product gets harder and harder using a monolithic approach. At 10 developers it's OK.    

*       Netflix had problems getting their DVD app out with around 100 people. Then they went to cloud and wanted to break up the app into separate pieces. Which ended up being microservices. Microservices grew from the desire to for speed and agility to have teams deploy independently.    

*       Microservices are different from SOA because they are loosely coupled. If updating one service requires the coordination of other services, then you are not loosely coupled. If pushing your app and would require an update to Google, Facebook, and Foursquare APIs then that’s not loosely coupled.    

*       An app should be deployed independently and the APIs it depends on should have enough stability that everything can be decoupled. Decoupling and separation of concerns is the key to microservices.    

*       If  one service goes down and there’s a ripple effect where dependent services start to fail then that’s a problem. Then you end up building bulkheads, circuit breakers, and other enterprise type patterns. You are building defensive code.    

*       The problem with waterfall is it takes too long and the feedback loops are too disconnected. Problems are externalized to some other group. In a waterfall approach by the time a problem is found the original team is probably already gone.    

*       Continuous delivery, turning around code in days or weeks, running code in production creates an immediate feedback loop and the externalities go away. If a developer writes bads code and the service goes down they get a call immediately.    

*       The blast radius on the boundary of failure must be contained. Decoupled microservices means more availability. If one service fails then it’s easy to identify and contain that failure. Who is involved in that service is easily determined so a problem doesn’t spread out and take out other services.    

*       This is very different than the tightly linked enterprise model where the ops guys have told developers everything will be perfect so you don’t have to code for things going down. The idea that ops can provide a perfect machine for developers to run on is one of the problems in the world. It’s dangerous. Telling developers that anything can break at any time is a different contract with developers. Developers have to do more work, but in the end products get to market quicker and they are more reliable. And because developers are running it themselves they spend less time talking to other groups in handoff meetings so they have more time to program.    

        

    As we might expect, the goals are are all noble. Avoiding defensive code, decoupling, separations of concerns, fast deployment, fault isolation, fast feedback loops, developer responsibility, focused apps that do one thing very well, independent upgrade paths, graceful versioning, separation between teams...all check.    

The question is: are microservices the only or even the best way to achieve these goals?

Take the first point, of independent features in a release requiring a general rollback of all features if one fails. That's an artifact of branching and release granularity policies. Etsy presumablyly gets around this issue by not buffering features in branches or in time to be released at once. Continually releasing many times a day means small sets of changes are released, which are independently revertible, so the problem doesn't happen. [Feature flags](http://stackoverflow.com/questions/7707383/what-is-a-feature-flag) are another, perhaps less graceful way, of dealing with misbehaving features in production.

Though microservices are one solution to the rollback problem, they are not the only solution, or even the most elegant solution, which is a pattern we'll see repeated in the melee.

##     Services are Nothing New    

    Take a look at the materials in the         _Related Articles_         section and it’s pretty clear microservices have won the thought leader space. Is there another side to the story?     

    I agree with others in that microservices is more a rebranding/marketecture spin on the very old notion of services. Do we really need a new word? I think it makes communication clearer, so yes. What has changed from the old days of services is the context. And a new binding requires a new word. It's like a linguistic method of versioning. 10 years ago all the words around that eras version of microservices would be different, so we need a new word now to reflect a new world.    

    What rankles a bit is the comparison of microservices with enterprise services. It's a strawman argument. Enterprise anything is like a mounted knight after the age of gunpowder. An easy target.    

The real comparison is with Unix and our old friend [/etc/services](file:///etc/services), that had a large number of independent services like nfs, ntp, smtp, whois, echo, time, ftp, quote, and hostnames. All of varying complexity. Some quite simple, some quite complex. But they are independent. And get this, they [    all ran    ](http://en.wikipedia.org/wiki/Inetd) on the same machine without the need for a container or a VM. Amazing!

    How were services created? By reading books by     [    W. Richard Stevens    ](http://en.wikipedia.org/wiki/W._Richard_Stevens)     and creating your own message handlers, formats, and protocols using native TCP/IP and threading libraries. Then on top of that you could build your own Actors, state machine processors, message forwarders, publish and subscribe mechanisms, timer handlers, thread pools, works queues, and advanced CPU scheduling algorithms.  Or you could have used a framework like     [    ACE    ](http://www.cs.wustl.edu/~schmidt/ACE.html)     to help you.             

    A bewildering number of services like the above were built and they performed like a bat out of hell. No HTTP, web servers, or any other modern “improvements” required.    

    In the mobile era HTTP has become more of a strategy tax than a help.    

    So services have always worked and none of the reasons why are new.             

##     In Defense of the Monolith - As Above So Below             

    Monoliths have also always worked. A service with any sort of complexity typically also has a complex threading and messaging structure internally as a service is the endpoint for a wide mix of requests/response dialogues. This means services internally can get very complex, requiring large teams working on them, along with stringent latency and high availability requirements.    

    What we see is that complexity is conserved. When it's all your own code complexity can’t be made simple by pushing it behind a wall. The complexity is somewhere. Simple must be intrinsically simple to really be simple, otherwise it’s just an abstraction and that won’t fool anyone for long.    

    All the things we like about decoupling and separation of concerns are possible in a code base. If you can’t make complex code work then look to your code structure, your language choice, your programmers, your source code control policies, your branching policies, your bug fixing policies, your release policies, and your build and deployment system. Don’t pass the buck to the Monolith.    

    Decoupling, for example, has long been accomplished via a mechanism called libraries. I know, this is software engineering and not algorithms, so please condescend with me. If a code base is structured into independent libraries with clean interfaces then hundreds of people from all over the world can work on them independently. Yes, it’s true. Libraries can be put in the source tree as separate products that are included in any project that needs them. They have their own release policies, bug tracking, etc. What must be stay true is the interface. Just like a service interface. If the interface changes then another version of the library can be created. Just like a service. Each library can be tested on its own. Just like a service. Each team can develop using their own process and policies. Just like a service.    

    How do all these different components get along inside a process?    

    Internally code in a process has, or should have, much the same sort of boot sequence a box undergoes for bringing together all library components, instantiating them, and configuring, and them managing them at run time, etc.             

    All things that run code must do this. Microservices must have a way internally to resolve internal and external service references, start services in some sort of dependent order, configure the services on startup, reconfigure the services at runtime, handle failures, handle HA, use metrics for monitoring, and gracefully bring this complex mess back down again. And so on.             

    The OS does this same sort of thing when it comes up. Each process running on the OS does the same thing. And each service does the same thing.         In each case what changes are the mechanisms to accomplish the goal, not the pattern itself. And each case is subject to their own set of difficulties that can't be hidden behind abstraction layers.    

    The one thing a Monolith can’t do is support developing in separate languages because a single image must be built. But that’s a tradeoff to be evaluated, not a hard reason against.    

So, the goals of the microservices movement are all peaches and cream. The mechanisms however for realizing those goals are more flexible than the proponents give credit. Source code can and does become a [Big Ball of Mud](http://en.wikipedia.org/wiki/Big_ball_of_mud). Much of the agile/TDD/extreme programming movement is a way of managing code entropy. Services can and do become their own Big Ball of Mud through exactly the same forces. A Monolithic code base, with proper software engineering, can work and work well. It does for Etsy.

##     Related Articles    

*   [On Reddit](http://www.reddit.com/r/programming/comments/2e3l2b/the_great_microservices_vs_monolithic_apps/) and [on Hacker News](https://news.ycombinator.com/item?id=8203373)

*       [Kelly Sommers](https://twitter.com/kellabyte/status/413002813459812352)        : Modelling fads like micro services are bad because it makes people stop thinking and prematurely decide on boundaries for all things broadly    

*   [    App Platform clouds - from servers, to paas, and back to servers    ](http://michaelneale.blogspot.com/2013/08/app-platform-clouds-from-servers-to.html)     - Servers --> Force.com --> GAE --> Heroku classic/CloudBees/EY --> CloudFoundry/Heroku Procfile --> Docker based clouds    

*   [    Adrian Cockcroft on Microservices and DevOps    ](http://www.infoq.com/interviews/adrian-cockcroft-microservices-devops)

*   [    The Microservice Architecture Sounds Like Service-Oriented Architecture    ](http://www.petrikainulainen.net/software-development/design/the-microservice-architecture-sounds-like-service-oriented-architecture/)

*   [    Velocity and Volume (or Speed Wins) by Adrian Cockcroft    ](https://www.youtube.com/watch?v=wyWI3gLpB8o)

*   [    Microservices - Money for old rope or re-badging SOA for the cool kids    ](http://service-architecture.blogspot.com/2014/03/microservices-money-for-old-rope-or-re.html)

*   [    Services, Microservices, Nanoservices – oh my!    ](http://arnon.me/2014/03/services-microservices-nanoservices/)

*   [    The microservice declaration of independence    ](https://docs.google.com/document/d/1LsaEw790rMeV6ICaVUxZtf5FqHJSVpr8v-N0-IUDTK4/edit)

*   [    CQRS    ](http://martinfowler.com/bliki/CQRS.html)     - Command Query Responsibility Segregation    

*   [    Microservices: It’s not (only) the size that matters, it’s (also) how you use them – part 2    ](http://www.tigerteam.dk/2014/micro-services-its-not-only-the-size-that-matters-its-also-how-you-use-them-part-2/)

*   [    How Micro-Services approach worked out in production    ](http://abdullin.com/post/how-micro-services-approach-worked-out-in-production/)

*   [    Micro Services: Java, the Unix Way    ](http://www.infoq.com/presentations/Micro-Services)        

*   [    Micro Service Architecture    ](http://www.slideshare.net/eduardsi/micro-service-architecture)

*   [    The buzz around microservices    ](http://www.planetgeek.ch/2014/03/18/the-buzz-around-microservices/)

*   [    Micro SOA    ](https://groups.google.com/forum/#!topic/object-composition/e6K7KhKkdnM)     thread on G+    

*   [    Micro and Nano Services    ](http://alexfalkowski.blogspot.com/2013/12/micro-and-nano-services.html)

*       Adrian Cockcroft     [    Tweet    ](https://twitter.com/adrianco/status/441883572618948608)    : Twitter microservices map looks just like the Netflix one. We called this the "Death Star" diagram    

*   [    Micro Services Style Data Work Flow    ](http://www.markhneedham.com/blog/2013/02/18/micro-services-style-data-work-flow/)

*   [    Scaling Gilt    ](http://www.slideshare.net/slideshow/embed_code/30059475#)     - From Monolithic Ruby App to Scala Distributed Micro-Services    

*   [    What is Service Choreography?    ](http://nic.ferrier.me.uk/blog/2013_12/what-is-service-choreography)

*   [    Practical microservices - YOW 2013    ](http://www.slideshare.net/spnewman/practical-microservices-yow-2013)

*   [    Micro Service Architectures with James Lewis and Matt Collinge    ](http://m.dotnetrocks.com/show.aspx?showNum=917)

*   [    Building Clojure Services at Scale    ](http://blog.josephwilk.net/clojure/building-clojure-services-at-scale.html)

*   [    Micro-services - When should you use micro-services?    ](http://davidmorgantini.blogspot.co.uk/2013/08/micro-services-when-should-you-use.html?utm_source=buffer&utm_campaign=Buffer&utm_content=buffer933a0&utm_medium=twitter)

*   [    Aligning software architecture and code    ](http://www.codingthearchitecture.com/2013/07/03/aligning_software_architecture_and_code.html)     - Monolithic architecture, micro-services or something in-between?    

*   [    Microservices - a brief presentation    ](http://developer-blog.cloudbees.com/2013/10/microservices-brief-presentation.html)

*   [    Micro-Service Architecture    ](http://vimeo.com/55042628)     by Fred George    

*   [    Why Puppet, Chef, Ansible aren't good enough    ](https://news.ycombinator.com/item?id=7378764)        

*   [    Stevey's Google Platforms Rant    ](http://pastebin.com/FBbSkp2i)

*   [    Netflix: How 'Separation of Concerns' Can Benefit Your API Design    ](http://www.programmableweb.com/news/netflix-how-separation-concerns-can-benefit-your-api-design/analysis/2014/03/04)

*   [    Optimizing the Netflix API    ](http://techblog.netflix.com/2013/01/optimizing-netflix-api.html)

    .    

    