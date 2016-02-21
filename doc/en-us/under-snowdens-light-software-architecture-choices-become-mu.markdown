## [Under Snowden's Light Software Architecture Choices Become Murky](/blog/2014/1/8/under-snowdens-light-software-architecture-choices-become-mu.html)

    

    

![](http://farm8.staticflickr.com/7354/11830796195_5f12ff15f9_m.jpg)

_    Adrian Cockcroft on the future of     [    Cloud, Open Source, SaaS and the End of Enterprise Computing    ](http://perfcap.blogspot.com/2013/12/looking-back-at-2013-with-pointers-to.html)    :    _

>     _Most big enterprise companies are actively working on their AWS rollout now. Most of them are also trying to get an in-house cloud to work, with varying amounts of success, but even the best private clouds are still years behind the feature set of public clouds, which is has a big impact on the agility and speed of product development_    

    While the     [    Snowden revelations    ](http://america.aljazeera.com/articles/multimedia/timeline-edward-snowden-revelations.html)     have tattered the thin veil of trust secreting Big Brother from We the People, they may also be driving a fascinating new tension in architecture choices between Cloud Native (scale-out, IaaS), Amazon Native (rich service dependencies), and Enterprise Native (raw hardware, scale-up).    

    This tension became evident in a recent     [    HipChat interview    ](http://highscalability.com/blog/2014/1/6/how-hipchat-stores-and-indexes-billions-of-messages-using-el.html)     where HipChat, makers of an AWS based SaaS chat product, were busy creating an on-premises version of their product that could operate behind the firewall in enterprise datacenters. This is consistent with other products from Atlassian in that they do offer hosted services as well as installable services, but it is also an indication of customer concerns over privacy and security.    

    The result is a potential shattering of backend architectures into many fragments like we’ve seen on the front-end. On the front-end you can develop for iOS, Android, HTML5, Windows, OSX, and so on. Any choice you make is like declaring for a cold war power in a winner take all battle for your development resources. Unifying this mess has been the refuge of cloud based services over HTTP. Now that safe place is threatened.    

    To see why consider the design dilemma HipChat is faced with. How can HipChat make an application that is Cloud Native, Amazon Native, and Enterprise Native all at the same time? Consider that certain customers are already pushing HipChat to go Amazon Native, which would be needed to operate in multiple regions, multiple availability zones, etc, ala Netflix.    

    As a startup Cloud Native is perfect for SaaS. You get a sweet little elastic scale-out architecture with minimal external dependencies.    

    How does Cloud Native transfer to the enterprise? Let’s say you have 20 enterprise customers. What kind of common cloud architecture can you expect?  If we had a winner in the not-Amazon private cloud race that had a very rich set of services that was widely adopted in the enterprise then that would be an option, but that’s not the case, yet. So what you may want to do is scale up. Get a huge machine with a few terabytes of memory and run on that. It’s simpler to install and manage in an unknown environment.    

    The problem is scale-up Enterprise Native is the exact opposite of how you would run on Amazon Native. All those nice Amazon Services won’t exist in the enterprise or in some other Cloud Native solution. And even now Amazon doesn’t have these huge instance types and the ones they do have are very expensive given the underlying raw hardware isn’t really that expensive, especially for an enterprise with an IT department.    

    These are three radically different architectures. HipChat is a relatively simple application, so it’s possible to minimize dependencies, create layers,  create services, select portable Open Source projects, and all that good stuff. But it’s a huge burden, one that will impact every aspect of development and deployment going forward. More complex products will not be so fortunate. Choices will have to be made.    

    We’ve transitioned to a mobile first world where iOS has been the developer platform of choice for revenue generation. This process has taken time as everyone has adjusted to market forces and trends. Are we now seeing a similar process where developers will have to make a choice between Enterprise First or an Amazon First strategy? This thinking may seem remote for many developers, but follow the money. Enterprise developers     [    generate four times as much revenue    ](http://www.developereconomics.com/still-building-consumer-apps-enterprise-pays-4x/)     as those targeting consumers.    

    Before Snowden it was much easier to conclude that in the mobile age cloud based SaaS would be the common denominator. But what about now?    

    