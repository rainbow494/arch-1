## [The Changing Face of Scale - The Downside of Scaling in the Contextual Age ](/blog/2013/3/27/the-changing-face-of-scale-the-downside-of-scaling-in-the-co.html)

    

    

![](http://farm9.staticflickr.com/8104/8595718016_c9b7590454_m.jpg)

[Robert Scoble](http://scobleizer.com/) is a kind of Brothers Grimm for the digital age. Instead of inspired romantics walking around the country side collecting the folk tales of past ages, he is an inspired technologist documenting the current mythology of startups.

One of the developments Robert is exploring is the rise of the [contextual age](http://www.forbes.com/sites/shelisrael/2012/07/17/announcing-age-of-context-a-new-book-with-robert-scoble/). Where every bit of information about you is continually being prodded, pulled, and observed, shoveled into a great learning machine, and turned into a fully actionable [knowledge graph](http://googleblog.blogspot.com/2012/05/introducing-knowledge-graph-things-not.html) of context. A digital identity more real to software than your physical body ever was. 

Sinner or saviour, the Age of Context has interesting implications for startups. It raises the entrance bar to dizzying heights. Much of the reason companies are tearing down the Golden Age of the Web, one open protocol at a time, is to create a walled garden of monopolized information.

To operate in this world you will have to somehow create a walled garden of your own. And it will be a damn big garden. So you must scale. [Facebook](http://gigaom.com/2012/08/13/facebooks-number-of-servers-soar-to-an-estimated-180k/) and [Google](http://www.wired.com/wiredenterprise/2012/10/ff-inside-google-data-center/all/) have an insane number of servers. Then you must gather to yourself the people capable of running these servers, creating the applications and cutting edge devices that tease out the data, store the data, learn from the data, and create a market for the data. A serious capital and technological barrier to entry. 

It's popular to say don't worry about scaling until you need to. It's a wonderful problem to have, etc. A curious aspect of these new contextual systems, Robert noticed, is that they need to plan on scale from the start. Not only must the infrastructure scale to gather and process context and turn it into smart data, the cost of each new customer is now huge.

One of the upsides of your typical software service is new customers are incrementally cheap to service, so they are highly profitable. The data is typically dead. It just sits in inexpensive storage and comes alive on demand, only when a user needs it. Economies of scale are your friend.

In the contextual age economies of scale are not your friend. So much processing power is needed to bring on a new customer and to keep customer data updated with new context, that it's hard to keep up with demand. Data is alive in the contextual age. It always has a cost because it is always being updated. It just doesn't spring to life when a user uses it. And the cost is always increasing because the amount of data and the number of links are always growing. This is a very different data life cycle. To get a detailed idea of what is required take a look at [Prismatic Architecture - Using Machine Learning On Social Networks To Figure Out What You Should Read On The Web](http://highscalability.com/blog/2012/7/30/prismatic-architecture-using-machine-learning-on-social-netw.html).

One example Robert gives in a [StackExchange podcast](http://blog.stackoverflow.com/2013/03/podcast-44-this-should-have-been-43/), is [Tempo](http://tempo.ai/), a smart calendar that "finds everything you need to be prepared." Tempo has had to implement a [take-a-number signup system](http://pandodaily.com/2013/02/15/tempo-follows-mailboxs-lead-adopts-a-take-a-number-signup-system/) because each new customer is so expensive and they have had such huge demand. Note [the keywords](http://www.zdnet.com/tempo-for-ios-is-the-best-business-calendar-app-7000013116/) "smart" and "find everything." Tempo [uses AI](http://gigaom.com/2013/02/19/want-tempos-new-calendar-assistant-youll-have-to-wait-for-its-ai-to-catch-up/) to generate a smart calendar and infer appointment details from context. These are not simple todo list apps that have very predictable scaling properties. 

In a [GigaOM interview](http://gigaom.com/2013/02/19/want-tempos-new-calendar-assistant-youll-have-to-wait-for-its-ai-to-catch-up/), Tempo CEO Raj Singh explains exactly why each new customer is so expensive:

>     It takes a huge amount of computing resources to bring new customer online. Its platform must initially cull through all of the data in the customer’s various email and social media accounts. Once the customer is on-boarded the burden on the AI lessens, though it does reprocess all of that data on a regular basis – any time new email or contact data is added to system, Tempo can generate new semantic links between new data and old. There is just generally a ton of CPU to make all of this work; processing data takes time and we don’t get a network effect, since we have to process each individual’s data. We re-process data constantly; to be semantically relevant and contextual, we’re constantly re-processing, this is very expensive (it’s like Google constantly re-crawling).     

Once separate database silos must be built, constantly rebuilt, and linked together. No small task. Each new customer takes a lot of processing power and in a socially networked world applications quickly become flash mobbed with registration applications. Robert notes Twitter took 6 months to grow to 13,000 users and Twitter had scaling issues for years. Tempo got to 13,000 users the first hour it was open. Hundreds of thousands of users were put on a waiting list. On its first day Tempo server load was 24 times higher than expected. 

Poor planning some say. Yet Tempo is not alone. VentureBeat has an article quoting the Mailbox CEO as saying their [insane 380K person wait list kept app from crashing today](http://venturebeat.com/2013/02/07/mailbox-app-iphone-wait-list/).

While most apps would kill millions of virtual NPCs to have this problem, it does show how the face of scale is changing. Lots of customers can mob applications that take a lot of resources to run. Google with [Google Now](http://www.google.com/landing/now/), for example, can take this load in stride. That's what they do.

Startups in the Contextual Age are not so fortunate. Fairy tales are stories of instruction, showing children how to navigate a dangerous world. New players in the Contextual Age must also navigate a challenging world. Those stories are still being written.

And don't take it too hard if your app doesn't win the popularity lottery, a lot of it is [pure luck](http://300songs.com/2011/07/15/74-hits-are-black-swans-take-the-skinheads-bowling/). 

    