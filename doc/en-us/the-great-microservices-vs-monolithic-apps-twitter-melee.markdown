## [The Great Microservices vs Monolithic Apps Twitter Melee ](/blog/2014/7/28/the-great-microservices-vs-monolithic-apps-twitter-melee.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, July 28, 2014 at 8:56AM</span></div>

<div class="body">

![](https://farm4.staticflickr.com/3926/14568159437_fe66e0bc3e_n.jpg) Once upon a time a great [Twitter melee](https://twitter.com/adrianco/status/441169921863860225) was fought for the coveted title of _Consensus Best Way to Structure Systems._ The competition was between Microservices and Monolithic Apps. 

Flying the the logo of Microservices, from a distant cloud covered land, is the Kingdom of Netflix, whose champion was Sir [Adrian Cockcroft](https://www.linkedin.com/in/adriancockcroft) (who has pledged fealty to another). And for the Kingdom of ThoughtWorks we have Sir [Sam Newman](https://twitter.com/samnewman) as champion.

Flying the logo of the Monolithic App is champion Sir J[ohn Allspaw](https://www.linkedin.com/in/jallspaw), from the fair Kingdom of Etsy.

<span>Knights from the Kingdom of Digital Ocean and several independent realms filled out the list.</span>

<span>To the winner goes a great prize: developer mindshare and the favor of that most fickle of ladies, Lady Luck.</span>

<span>May the best paradigm win.</span>

<span>The opening blow was wielded by the highly ranked Sir Cockcroft, a veteran of many tournaments:</span>

> <span>adrian cockcroft ‏@adrianco  Mar 5</span>
> 
> <span>Etsy at #qconlondon make it clear to me why monolithic apps are a dead end. Use microservices for continuous scalable deployments</span>
> 
> <span>Neil Bartlett ‏@nbartlett  Mar 5</span>
> 
> <span>@adrianco Yes, +1000 for this. Though reasonable people can disagree about how those microservices are to be implemented /cc @AnneWoof</span><span style="font-size: 12px;"> </span>
> 
> <span>adrian cockcroft ‏@adrianco  Mar 5</span>
> 
> <span>@nbartlett @AnneWoof monolithic apps force everyone into the same decisions. Microservice freedom to innovate and optimize as needed</span>
> 
> <span>John Allspaw ‏@allspaw  Mar 5</span>
> 
> <span>‏@adrianco your critical thinking is lacking on this one, because you're unable to imagine all benefits.</span>
> 
> <span>Sam Newman ‏@samnewman  Mar 5</span>
> 
> <span>@allspaw @adrianco generically, microservices give you more options about how to get things done</span><span style="font-size: 12px;"> </span>
> 
> <span>John Allspaw ‏@allspaw  Mar 5</span>
> 
> <span>@samnewman and more/different constraints. @adrianco</span><span style="font-size: 12px;"> </span>
> 
> <span>John Allspaw ‏@allspaw  Mar 5</span>
> 
> <span>@samnewman sometimes, less options can open opportunities. A few number of well-understood tools and patterns brings advantages @adrianco</span>
> 
> <span>John Allspaw ‏@allspaw  Mar 5</span>
> 
> <span>@samnewman and microservices can also be an alias for "I want to get my way without having to debate the merits of my decisions." @adrianco</span>
> 
> <span>Sam Newman ‏@samnewman  Mar 5</span>
> 
> <span>@allspaw @adrianco it can give more freedom - it actually shifts where you standardise, and where you allow for free choice</span>
> 
> <span>Sam Newman ‏@samnewman  Mar 5</span>
> 
> <span>@allspaw @adrianco standardisation between services is vital (monitoring, integration). Inside the box? Give the team autonomy</span>
> 
> <span>Sam Newman ‏@samnewman  Mar 5</span>
> 
> <span>@allspaw @adrianco all this relies on having a good description of what a ‘good citizen’ is for your organisation</span>
> 
> <span>Mark Burgess ‏@markburgess_osl  Mar 5</span>
> 
> <span>@samnewman @allspaw @adrianco The key is: can you measure diffs between these approaches in user/provider experience fitness for purpose etc</span>
> 
> <span>Mark Burgess ‏@markburgess_osl  Mar 5</span>
> 
> <span>@samnewman @allspaw @adrianco Once you've identified who benefits from what, there is a policy weighted optimization to consider.</span>
> 
> <span>Mark Burgess ‏@markburgess_osl  Mar 5</span>
> 
> <span>@samnewman @allspaw @adrianco This was an interesting (and very polarized) discussion!</span><span style="font-size: 12px;"> </span>
> 
> <span>adrian cockcroft ‏@adrianco  Mar 6</span>
> 
> <span>@allspaw Netflix was monolithic app for the first 3 years I worked there. As the team scaled beyond 100 engineers concerns were separated</span>
> 
> <span>adrian cockcroft ‏@adrianco  Mar 6</span>
> 
> <span>@samnewman @allspaw centrally planned and coordinated things don't scale and don't innovate as fast as high trust freedom and responsibility</span>
> 
> <span></span>
> 
> <span>John Allspaw ‏@allspaw  Mar 6</span>
> 
> <span>@adrianco We have high trust, freedom responsibility at Etsy with our architecture and process. Conway's Law is not a law. /cc @samnewman</span>
> 
> <span>John Allspaw ‏@allspaw  Mar 6</span>
> 
> <span>@adrianco I'm saying that you are ignoring the possibility that you're wrong in your application of absolutist assertions to Etsy.</span>
> 
> <span>John Allspaw ‏@allspaw  Mar 6</span>
> 
> <span>@adrianco Who said Etsy was centrally planned? Your mental model of Etsy's development culture is flawed. You don't sound even curious.</span>
> 
> <span>adrian cockcroft ‏@adrianco  Mar 6</span>
> 
> <span>@allspaw sorry, I'm not sure what assertion you think I'm making. I'm discussing alternatives to scaling a monolith like Etsy or Facebook</span>
> 
> <span>John Allspaw ‏@allspaw  Mar 6</span>
> 
> <span>@adrianco I didn't hear "alternative", I heard "doesn't work", "dead", and "microservices are in all ways superior". Apologies if incorrect.</span>
> 
> <span>adrian cockcroft ‏@adrianco  Mar 6</span>
> 
> <span>@allspaw I went to both Etsy talks yesterday. Monolith in php prevents implementation of features in other languages, e.g. clojure.</span><span style="font-size: 12px;"> </span>
> 
> <span>John Allspaw ‏@allspaw  Mar 6</span>
> 
> <span>@adrianco Yep. And microservices in clojure prevents reaping advantages in monolith php. Engineering is trade-offs amongst many influences.</span><span style="font-size: 12px;"> </span>
> 
> <span>John Allspaw ‏@allspaw  Mar 6</span>
> 
> <span>@adrianco and I've been working there for >4 years. I'm telling you you're missing context, that is all. :)</span><span style="font-size: 12px;"> </span>
> 
> <span>adrian cockcroft ‏@adrianco  Mar 6</span>
> 
> <span>@allspaw I guess someone from Etsy should do a talk on the advantages of a monolith, but I don't see how adding indep team prevents innov</span>
> 
> <span>Alan ‏@AlanMorrison  Mar 6</span>
> 
> <span>@adrianco @allspaw The right level of services should be in between micro and macro, e.g., steps in a process, yes? http://bit.ly/1nha966</span>
> 
> <span>John Allspaw ‏@allspaw  Mar 6</span>
> 
> <span>@adrianco a fine idea.</span>
> 
> <span>Adam Thody ‏@thody  Mar 6</span>
> 
> <span>@allspaw @adrianco a fine, and long overdue debate gentlemen. In software and in orgs there's little room for dogmatism.</span>
> 
> <span>John Allspaw ‏@allspaw  Mar 6</span>
> 
> <span>@adrianco a summing up of my thoughts: https://gist.github.com/anonymous/9388472 … /cc @kellan</span>
> 
> <span>1\. Monolithic applications and architectures  can vary in their monolithness. This is an under-specified description.</span>
> 
> <span>2\. Microservice applications and architectures can vary in their microness. This is an under-specified description.</span>
> 
> <span>3\. Microservices and monolithic architectures have both benefits and disadvantages.</span>
> 
> <span>4\. Organizations will exploit those benefits while working around any weaknesses.</span>
> 
> <span>5\. Success of the business is a large influence on the exploitation of benefits and implementation and costs of workarounds.</span>
> 
> <span>6\. All benefits and work arounds are context-sensitive. Meaning that they are both technically and socially constructed by the organization that navigates them.</span>
> 
> <span>7\. Path dependency is a thing. History matters and manifests in these architectural decisions and evolution in an organization.</span>
> 
> <span>8\. Patterns exist to inform practice, not dictate it. Zealous adherence to an architectural pattern brings peril when it is to the exclusion of cultural context in actual practice.</span>
> 
> <span>9\. Architectural patterns will expand, contract, evolve, and change to fit the trade-offs that an organization perceives it has to make</span>
> 
> <span>Kevin Behr ‏@kevinbehr  Mar 6</span>
> 
> <span>@allspaw @adrianco great fun. I read this thread and said "I love variation and adaptation" out loud.</span><span style="font-size: 12px;"> </span>
> 
> <span>Sam Newman ‏@samnewman  Mar 6</span>
> 
> <span>@allspaw @adrianco I have found conways law to prove true more than have it prove false! I use it as guidance though, not hard constraint</span>
> 
> <span>Sam Newman ‏@samnewman  Mar 6</span>
> 
> <span>@allspaw @adrianco trust leading to autonomy are the key - many ways to get there. The right architecture counts!</span>
> 
> <span>Sam Newman ‏@samnewman  Mar 6</span>
> 
> <span>@allspaw @adrianco where standardisation is required, I try to make it easy/transparent, eg provide tools to do the right thing</span><span style="font-size: 12px;"> </span>
> 
> <span>adrian cockcroft ‏@adrianco  Mar 6</span>
> 
> <span>@allspaw @kellan agree with those, but have some to add. A micro service arch could be seen as a collection of small monoliths.</span><span style="font-size: 12px;"> </span>
> 
> <span>adrian cockcroft ‏@adrianco  Mar 6</span>
> 
> <span>@samnewman @allspaw inverse application of conways law helps influence the architectures that gets built</span>
> 
> <span>Steve Smith ‏@AgileSteveSmith  Mar 6</span>
> 
> <span>@samnewman I like to see standards gradually emerging as a byproduct of success, and be baseline for future improvement /@allspaw @adrianco</span>
> 
> <span>Anthony Elizondo ‏@complex  Mar 6</span>
> 
> <span>@adrianco @allspaw @kellan depends on your pov. the provider sees monolith, to consumer it is micro, one of many.</span>
> 
> <span>Sam Newman ‏@samnewman  Mar 6</span>
> 
> <span>@AgileSteveSmith @allspaw @adrianco I’ve seen this reflected in incremental changes to internal tool chains and service templates</span><span style="font-size: 12px;"> </span>
> 
> <span>Sam Newman ‏@samnewman  Mar 6</span>
> 
> <span>@AgileSteveSmith @allspaw @adrianco generally agree, but some things may have to be decided upfront,eg measures to avoid cascading failures</span>

<span>As you can see many blows were landed. And while blood spilled freely onto the field, no wounds proved mortal.</span>

<span>Then the herald announced free beer and t-shirts out in the market, so no winner could be declared. The battle would continue another day...perhaps every generation must fight this same fight.</span>

<span>As your bard I feel it necessary to explain what point of honor demanded this clanging exchange of blows.</span>

## <span>Monolithic Apps are Considered an Anti-Pattern</span><span style="font-size: 12px;"> </span>

<span>It is written in</span> [<span>Development, Deployment and Collaboration at Etsy</span>](https://speakerdeck.com/mrtazz/development-deployment-and-collaboration-at-etsy) <span>that “At Etsy about 150 engineers deploy a single monolithic application more than 60 times a day.” A</span> [<span>monolithic application</span>](http://cantina.co/monolithic-architecture-doesnt-scale/) <span>is where “all of the required logic is located within one ‘unit’ (a war, a jar, a single application, one repository).”</span>

Etsy is [successful](http://www.entrepreneur.com/article/235364) as a company and it is not a small site, as of February 2013: 1.49 billion page views, 4,215,169 items sold, $94.7 million of goods sold, 22+ million members, 800,000+ active shops.

<span>Etsy also serves as a fine example of how to build a great site and do things right: continuous integration; per developer VMs; push button deployment; good monitoring; developers deploy to the site on the first day; GitHub; Chef; IRC to control releases; dashboards; and they do not use source code control branches, effectively always deploying from the main branch.</span>

<span>So how could Etsy still use a monolithic image?</span>

<span>The sense of disbelief is because monolithic applications are considered an anti-pattern. Read more about why in</span> [<span>Monolithic Architecture Doesn’t Scale!</span>](http://cantina.co/monolithic-architecture-doesnt-scale/) <span>The gist of it is that scale here refers to the ability of  a large group of developers to successfully change, test, and release code, not scale as in how many requests per second a system handles.  </span>

<span>The old saying applies: Too many cooks in the kitchen spoil the broth.</span>

<span>We all know the way to make a great broth is to divide the kitchen into many kitchenettes, each with their own cook, staff, supplies, and equipment, and then have the hungry customer coordinate all the separate cooks in how to make a tasty broth. Now there's a recipe for becoming a Chopped Champion.</span>

<span>Which leads us to microservices…</span>

## <span>Microservices are Awesomesauce</span><span style="font-size: 12px;"> </span>

<span>One solution to the monolithic application problem is to break up an application into microservices. Martin Fowler</span> [<span>explains</span>](http://martinfowler.com/articles/microservices.html)<span>:</span>

> <span>The term "microservice" was discussed at a workshop of software architects near Venice in May, 2011 to describe what the participants saw as a common architectural style that many of them had been recently exploring. In May 2012, the same group decided on "microservices" as the most appropriate name. James presented some of these ideas as a case study in March 2012 at 33rd Degree in Krakow in</span> [<span>Microservices - Java, the Unix Way</span>](http://2012.33degree.org/talk/show/67) <span>as did Fred George</span> [<span>about the same time</span>](http://www.slideshare.net/fredgeorge/micro-service-architecure)<span>. Adrian Cockcroft at Netflix, describing this approach as "fine grained SOA" was pioneering the style at web scale as were many of the others mentioned in this article - Joe Walnes, Dan North, Evan Botcher and Graham Tackley</span>

<span>For a tight explanation of microservices in action listen to this interview with Sir Cockroft at the</span> [<span>DevOps Cafe</span>](https://www.youtube.com/watch?v=dAOkEnBS498)<span>. The microservices section starts about 30 minutes in.</span><span style="font-size: 12px;"> </span><span>My gloss on the talk is:</span>

*   <span>Optimizing for speed causes developers to integrate code into a monolithic app. Getting a monolithic app to work is hard. When one feature breaks and must be rolled back that means the other features released at the same time must also be backed out.</span>

*   <span>Once more than 100 developers work on a project a threshold is reached where building a product gets harder and harder using a monolithic approach. At 10 developers it's OK.</span>

*   <span>Netflix had problems getting their DVD app out with around 100 people. Then they went to cloud and wanted to break up the app into separate pieces. Which ended up being microservices. Microservices grew from the desire to for speed and agility to have teams deploy independently.</span>

*   <span>Microservices are different from SOA because they are loosely coupled. If updating one service requires the coordination of other services, then you are not loosely coupled. If pushing your app and would require an update to Google, Facebook, and Foursquare APIs then that’s not loosely coupled.</span>

*   <span>An app should be deployed independently and the APIs it depends on should have enough stability that everything can be decoupled. Decoupling and separation of concerns is the key to microservices.</span>

*   <span>If  one service goes down and there’s a ripple effect where dependent services start to fail then that’s a problem. Then you end up building bulkheads, circuit breakers, and other enterprise type patterns. You are building defensive code.</span>

*   <span>The problem with waterfall is it takes too long and the feedback loops are too disconnected. Problems are externalized to some other group. In a waterfall approach by the time a problem is found the original team is probably already gone.</span>

*   <span>Continuous delivery, turning around code in days or weeks, running code in production creates an immediate feedback loop and the externalities go away. If a developer writes bads code and the service goes down they get a call immediately.</span>

*   <span>The blast radius on the boundary of failure must be contained. Decoupled microservices means more availability. If one service fails then it’s easy to identify and contain that failure. Who is involved in that service is easily determined so a problem doesn’t spread out and take out other services.</span>

*   <span>This is very different than the tightly linked enterprise model where the ops guys have told developers everything will be perfect so you don’t have to code for things going down. The idea that ops can provide a perfect machine for developers to run on is one of the problems in the world. It’s dangerous. Telling developers that anything can break at any time is a different contract with developers. Developers have to do more work, but in the end products get to market quicker and they are more reliable. And because developers are running it themselves they spend less time talking to other groups in handoff meetings so they have more time to program.</span>

<span></span>

<span>As we might expect, the goals are are all noble. Avoiding defensive code, decoupling, separations of concerns, fast deployment, fault isolation, fast feedback loops, developer responsibility, focused apps that do one thing very well, independent upgrade paths, graceful versioning, separation between teams...all check.</span>

The question is: are microservices the only or even the best way to achieve these goals?

Take the first point, of independent features in a release requiring a general rollback of all features if one fails. That's an artifact of branching and release granularity policies. Etsy presumablyly gets around this issue by not buffering features in branches or in time to be released at once. Continually releasing many times a day means small sets of changes are released, which are independently revertible, so the problem doesn't happen. [Feature flags](http://stackoverflow.com/questions/7707383/what-is-a-feature-flag) are another, perhaps less graceful way, of dealing with misbehaving features in production.

Though microservices are one solution to the rollback problem, they are not the only solution, or even the most elegant solution, which is a pattern we'll see repeated in the melee.

## <span>Services are Nothing New</span>

<span>Take a look at the materials in the</span> <span>_Related Articles_</span> <span>section and it’s pretty clear microservices have won the thought leader space. Is there another side to the story? </span>

<span>I agree with others in that microservices is more a rebranding/marketecture spin on the very old notion of services. Do we really need a new word? I think it makes communication clearer, so yes. What has changed from the old days of services is the context. And a new binding requires a new word. It's like a linguistic method of versioning. 10 years ago all the words around that eras version of microservices would be different, so we need a new word now to reflect a new world.</span>

<span>What rankles a bit is the comparison of microservices with enterprise services. It's a strawman argument. Enterprise anything is like a mounted knight after the age of gunpowder. An easy target.</span>

The real comparison is with Unix and our old friend [/etc/services](file:///etc/services), that had a large number of independent services like nfs, ntp, smtp, whois, echo, time, ftp, quote, and hostnames. All of varying complexity. Some quite simple, some quite complex. But they are independent. And get this, they [<span>all ran</span>](http://en.wikipedia.org/wiki/Inetd) on the same machine without the need for a container or a VM. Amazing!

<span>How were services created? By reading books by</span> [<span>W. Richard Stevens</span>](http://en.wikipedia.org/wiki/W._Richard_Stevens) <span>and creating your own message handlers, formats, and protocols using native TCP/IP and threading libraries. Then on top of that you could build your own Actors, state machine processors, message forwarders, publish and subscribe mechanisms, timer handlers, thread pools, works queues, and advanced CPU scheduling algorithms.  Or you could have used a framework like</span> [<span>ACE</span>](http://www.cs.wustl.edu/~schmidt/ACE.html) <span>to help you.</span><span style="font-size: 12px;"> </span>

<span>A bewildering number of services like the above were built and they performed like a bat out of hell. No HTTP, web servers, or any other modern “improvements” required.</span>

<span>In the mobile era HTTP has become more of a strategy tax than a help.</span>

<span>So services have always worked and none of the reasons why are new.</span><span style="font-size: 12px;"> </span>

## <span>In Defense of the Monolith - As Above So Below</span><span style="font-size: 12px;"> </span>

<span>Monoliths have also always worked. A service with any sort of complexity typically also has a complex threading and messaging structure internally as a service is the endpoint for a wide mix of requests/response dialogues. This means services internally can get very complex, requiring large teams working on them, along with stringent latency and high availability requirements.</span>

<span>What we see is that complexity is conserved. When it's all your own code complexity can’t be made simple by pushing it behind a wall. The complexity is somewhere. Simple must be intrinsically simple to really be simple, otherwise it’s just an abstraction and that won’t fool anyone for long.</span>

<span>All the things we like about decoupling and separation of concerns are possible in a code base. If you can’t make complex code work then look to your code structure, your language choice, your programmers, your source code control policies, your branching policies, your bug fixing policies, your release policies, and your build and deployment system. Don’t pass the buck to the Monolith.</span>

<span>Decoupling, for example, has long been accomplished via a mechanism called libraries. I know, this is software engineering and not algorithms, so please condescend with me. If a code base is structured into independent libraries with clean interfaces then hundreds of people from all over the world can work on them independently. Yes, it’s true. Libraries can be put in the source tree as separate products that are included in any project that needs them. They have their own release policies, bug tracking, etc. What must be stay true is the interface. Just like a service interface. If the interface changes then another version of the library can be created. Just like a service. Each library can be tested on its own. Just like a service. Each team can develop using their own process and policies. Just like a service.</span>

<span>How do all these different components get along inside a process?</span>

<span>Internally code in a process has, or should have, much the same sort of boot sequence a box undergoes for bringing together all library components, instantiating them, and configuring, and them managing them at run time, etc.</span><span style="font-size: 12px;"> </span>

<span>All things that run code must do this. Microservices must have a way internally to resolve internal and external service references, start services in some sort of dependent order, configure the services on startup, reconfigure the services at runtime, handle failures, handle HA, use metrics for monitoring, and gracefully bring this complex mess back down again. And so on.</span><span style="font-size: 12px;"> </span>

<span>The OS does this same sort of thing when it comes up. Each process running on the OS does the same thing. And each service does the same thing.</span><span> In each case what changes are the mechanisms to accomplish the goal, not the pattern itself. And each case is subject to their own set of difficulties that can't be hidden behind abstraction layers.</span>

<span>The one thing a Monolith can’t do is support developing in separate languages because a single image must be built. But that’s a tradeoff to be evaluated, not a hard reason against.</span>

So, the goals of the microservices movement are all peaches and cream. The mechanisms however for realizing those goals are more flexible than the proponents give credit. Source code can and does become a [Big Ball of Mud](http://en.wikipedia.org/wiki/Big_ball_of_mud). Much of the agile/TDD/extreme programming movement is a way of managing code entropy. Services can and do become their own Big Ball of Mud through exactly the same forces. A Monolithic code base, with proper software engineering, can work and work well. It does for Etsy.

## <span>Related Articles</span>

*   [On Reddit](http://www.reddit.com/r/programming/comments/2e3l2b/the_great_microservices_vs_monolithic_apps/) and [on Hacker News](https://news.ycombinator.com/item?id=8203373)

*   <span>[Kelly Sommers](https://twitter.com/kellabyte/status/413002813459812352)</span><span>: Modelling fads like micro services are bad because it makes people stop thinking and prematurely decide on boundaries for all things broadly</span>

*   [<span>App Platform clouds - from servers, to paas, and back to servers</span>](http://michaelneale.blogspot.com/2013/08/app-platform-clouds-from-servers-to.html) <span>- Servers --> Force.com --> GAE --> Heroku classic/CloudBees/EY --> CloudFoundry/Heroku Procfile --> Docker based clouds</span>

*   [<span>Adrian Cockcroft on Microservices and DevOps</span>](http://www.infoq.com/interviews/adrian-cockcroft-microservices-devops)

*   [<span>The Microservice Architecture Sounds Like Service-Oriented Architecture</span>](http://www.petrikainulainen.net/software-development/design/the-microservice-architecture-sounds-like-service-oriented-architecture/)

*   [<span>Velocity and Volume (or Speed Wins) by Adrian Cockcroft</span>](https://www.youtube.com/watch?v=wyWI3gLpB8o)

*   [<span>Microservices - Money for old rope or re-badging SOA for the cool kids</span>](http://service-architecture.blogspot.com/2014/03/microservices-money-for-old-rope-or-re.html)

*   [<span>Services, Microservices, Nanoservices – oh my!</span>](http://arnon.me/2014/03/services-microservices-nanoservices/)

*   [<span>The microservice declaration of independence</span>](https://docs.google.com/document/d/1LsaEw790rMeV6ICaVUxZtf5FqHJSVpr8v-N0-IUDTK4/edit)

*   [<span>CQRS</span>](http://martinfowler.com/bliki/CQRS.html) <span>- Command Query Responsibility Segregation</span>

*   [<span>Microservices: It’s not (only) the size that matters, it’s (also) how you use them – part 2</span>](http://www.tigerteam.dk/2014/micro-services-its-not-only-the-size-that-matters-its-also-how-you-use-them-part-2/)

*   [<span>How Micro-Services approach worked out in production</span>](http://abdullin.com/post/how-micro-services-approach-worked-out-in-production/)

*   [<span>Micro Services: Java, the Unix Way</span>](http://www.infoq.com/presentations/Micro-Services)<span></span>

*   [<span>Micro Service Architecture</span>](http://www.slideshare.net/eduardsi/micro-service-architecture)

*   [<span>The buzz around microservices</span>](http://www.planetgeek.ch/2014/03/18/the-buzz-around-microservices/)

*   [<span>Micro SOA</span>](https://groups.google.com/forum/#!topic/object-composition/e6K7KhKkdnM) <span>thread on G+</span>

*   [<span>Micro and Nano Services</span>](http://alexfalkowski.blogspot.com/2013/12/micro-and-nano-services.html)

*   <span>Adrian Cockcroft</span> [<span>Tweet</span>](https://twitter.com/adrianco/status/441883572618948608)<span>: Twitter microservices map looks just like the Netflix one. We called this the "Death Star" diagram</span>

*   [<span>Micro Services Style Data Work Flow</span>](http://www.markhneedham.com/blog/2013/02/18/micro-services-style-data-work-flow/)

*   [<span>Scaling Gilt</span>](http://www.slideshare.net/slideshow/embed_code/30059475#) <span>- From Monolithic Ruby App to Scala Distributed Micro-Services</span>

*   [<span>What is Service Choreography?</span>](http://nic.ferrier.me.uk/blog/2013_12/what-is-service-choreography)

*   [<span>Practical microservices - YOW 2013</span>](http://www.slideshare.net/spnewman/practical-microservices-yow-2013)

*   [<span>Micro Service Architectures with James Lewis and Matt Collinge</span>](http://m.dotnetrocks.com/show.aspx?showNum=917)

*   [<span>Building Clojure Services at Scale</span>](http://blog.josephwilk.net/clojure/building-clojure-services-at-scale.html)

*   [<span>Micro-services - When should you use micro-services?</span>](http://davidmorgantini.blogspot.co.uk/2013/08/micro-services-when-should-you-use.html?utm_source=buffer&utm_campaign=Buffer&utm_content=buffer933a0&utm_medium=twitter)

*   [<span>Aligning software architecture and code</span>](http://www.codingthearchitecture.com/2013/07/03/aligning_software_architecture_and_code.html) <span>- Monolithic architecture, micro-services or something in-between?</span>

*   [<span>Microservices - a brief presentation</span>](http://developer-blog.cloudbees.com/2013/10/microservices-brief-presentation.html)

*   [<span>Micro-Service Architecture</span>](http://vimeo.com/55042628) <span>by Fred George</span>

*   [<span>Why Puppet, Chef, Ansible aren't good enough</span>](https://news.ycombinator.com/item?id=7378764)<span></span>

*   [<span>Stevey's Google Platforms Rant</span>](http://pastebin.com/FBbSkp2i)

*   [<span>Netflix: How 'Separation of Concerns' Can Benefit Your API Design</span>](http://www.programmableweb.com/news/netflix-how-separation-concerns-can-benefit-your-api-design/analysis/2014/03/04)

*   [<span>Optimizing the Netflix API</span>](http://techblog.netflix.com/2013/01/optimizing-netflix-api.html)

<span>.</span>

</div>