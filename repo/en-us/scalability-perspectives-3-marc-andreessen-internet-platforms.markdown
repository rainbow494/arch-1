## [Scalability Perspectives #3: Marc Andreessen – Internet Platforms](/blog/2008/11/24/scalability-perspectives-3-marc-andreessen-internet-platform.html)

    

    

_[Scalability Perspectives](http://highscalability.com/tags/perspectives) is a series of posts that highlights the ideas that will shape the next decade of IT architecture. Each post is dedicated to a thought leader of the information age and his vision of the future. Be warned though – the journey into the minds and perspectives of these people requires an open mind._

### Marc Andreessen

Marc Andreessen is known as an internet pioneer, entrepreneur, investor, startup coach, blogger, and a multi-millionaire software engineer best known as co-author of Mosaic, the first widely-used web browser, and founder of Netscape Communications Corporation. He was the chair of Opsware, a software company he founded originally as Loudcloud, when it was acquired by Hewlett-Packard. He is also a co-founder of Ning, a company which provides a platform for social-networking websites. He has recently joined the Board of Directors of Facebook and eBay.  

Marc is an investor in several startups including Digg, Metaplace, Plazes, Qik, and Twitter. His passion is to create new technologies, to start new companies, and to scale them up.

### Internet Platforms Rule the Cloud

From Marc's Blog Post on the Three Kinds of Platforms:  

<cite>One of the hottest of hot topics these days is the topic of Internet platforms, or platforms on the Internet. However, the concept of "platform" is also the focus of a swirling vortex of confusion -- lots of platform-related concepts, many of them highly technical, bleeding together; lots of people harboring various incompatible mental images of what's about to happen in our industry as a consequence of various platforms. I think this confusion is due in part to the term "platform" being overloaded and being used to mean many different things, and in part because there truly are a lot of moving parts at play that intersect in fascinating but complex ways.</cite>  

Marc attempts to disentangle and examine the topic of "Internet platform" in detail. He has identified three distinct approaches to providing an Internet platform and shows us where each of the three approaches could go.

### Internet Platforms Defined

<cite>**A "platform" is a system that can be programmed and therefore customized by outside developers** -- users -- and in that way, adapted to countless needs and niches that the platform's original developers could not have possibly contemplated, much less had time to accommodate.</cite>  
...  
<cite>The key term in the definition of platform is "programmed". If you can program it, then it's a platform. If you can't, then it's not.</cite>  

The Internet gives rise to three new models of platform that is playing out in the Internet industry today. Marc calls these Internet platform models "levels", because as you go from Level 1 to Level 2 to Level 3, each kind of platform is harder to build, but much better for the developer. Further, each level typically supersets the levels below.

### Level 1 Platform - "Access API"

<cite>This is the kind of Internet platform that is most common today. This is typically a platform provided in the form of a web services API -- which will typically be accessed using an access protocol such as REST or SOAP.</cite>  

<cite>Architecturally, the key thing to understand about this kind of platform is that the developer's application code lives outside the platform -- the code executes somewhere else, on a server elsewhere on the Internet that is provided by the developer.</cite>  

Examples: eBay, Paypal, Flickr, Delicious

*   The entire burden of building and running the application itself is left entirely to the developer
*   The easiest kind of Internet platform to create

### Level 2 Platform - "Plug-In API"

<cite>This is the kind of platform approach that historically has been used in end-user applications to let developers build new functions that can be injected, or "plug in", to the core system and its user interface.</cite>  

<cite>In the Internet realm, the first Level 2 platform that I'm aware of is the Facebook platform.</cite>  

<cite>When you develop a Facebook app, you are not developing an app that simply draws on data or services from Facebook, as you would with a Level 1 platform. Instead, you are building an app that acts like a "plug-in" into Facebook -- your app literally shows up within the Facebook user experience, often as a box in the middle of a page that Facebook otherwise defines, such as a user profile page.</cite>

*   The third-party app itself lives outside the platform
*   The entire burden of building and running a Level 2 platform-based app is left entirely to the developer
*   Unlike a Level 1 platform where the burden of exposing the app to users is also placed on the developer, Level 2 Internet platforms -- as demonstrated by Facebook -- will be able to directly help their developers get users for their apps
*   Level 2 platforms are significantly harder to create than Level 1 platforms

### Level 3 Platform - "Runtime Environment"

<cite>In a Level 3 platform, the huge difference is that the third-party application code actually runs inside the platform -- developer code is uploaded and runs online, inside the core system. For this reason, in casual conversation I refer to Level 3 platforms as "online platforms".</cite>  

<cite>In addition, it is highly likely that a Level 3 platform will also superset Level 2 and Level 1 -- i.e., a Level 3 platform will typically also have some kind of plug-in API and some kind of access API.</cite>  

<cite>Put in plain English? A Level 3 platform's developers upload their code into the platform itself, which is where that code runs. As a developer on a Level 3 platform, you don't need your own servers, your own storage, your own database, your own bandwidth, nothing... in fact, often, all you will really need is a browser. The platform itself handles everything required to run your application on your behalf.</cite>  

<cite>Obviously this is a huge difference from Level 2\. And this difference -- and what makes it possible -- is why I think **Level 3 platforms are the future**.</cite>

*   Level 3 platforms are much harder to build than Level 2 platforms.
*   The level of technical expertise required of someone to develop on your platform drops by at least 90%, and the level of money they need drops to $0
*   The Level 3 Internet platform approach is much more like the computer industry's typical platform (PC) model than Levels 2 or 1.

### Who is building Level 3 Internet platforms?

Marc has built one - [Ning](http://www.ning.com/) has been built from the start to be a Level 3 platform.  

Other Level 3 platforms include:

*   Salesforce.com
*   [Second Life](http://highscalability.com/second-life-architecture-grid)
*   [Amazon.com](http://highscalability.com/amazon-architecture)
*   [zembly.com](http://zembly.com/)
*   Akamai

### How will we see the new platforms of the future?

<cite>We are used to seeing platforms ship as products -- you buy and install a PC or a server and you build an app that runs on it, or equivalently you download and install an open source platform such as Perl or Ruby and you build an app that runs on it.</cite>  

<cite>The platforms of the future won't be like that. The platforms of the future will be online services that you will tap into over the Internet, perhaps with nothing more running locally than a browser. They won't have anything you download, or even an SDK. They will look more like services than software. To paraphrase the Book of Matthew, "you will know them by their URLs".</cite>  

Read Marc's full blog post for more details! Can you add more Level 3 Platforms?

### Information Sources

*   Marc Andreessen's Blog: [The three kinds of platforms you meet on the Internet](http://blog.pmarca.com/2007/09/the-three-kinds.html)
*   Wikipedia: [Marc Andreessen](http://en.wikipedia.org/wiki/Marc_Andreessen)
*   CrunchBase: [Marc Andreessen](http://www.crunchbase.com/person/marc-andreessen)
*   Internet Pioneers: [Marc Andreessen](http://www.ibiblio.org/pioneers/andreesen.html)
*   [Internet Platforms Defined](http://innowave.blogspot.com/2007/09/internet-platforms-defined.html)
*   [Interview with Marc Andreessen from 1995](http://americanhistory.si.edu/collections/comphist/ma1.html)

    