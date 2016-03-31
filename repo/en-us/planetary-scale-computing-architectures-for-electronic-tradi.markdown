## [Planetary-Scale Computing Architectures for Electronic Trading and How Algorithms Shape Our World](/blog/2014/2/19/planetary-scale-computing-architectures-for-electronic-tradi.html)

    

    

Algorithms are moving out of the Platonic realm and are becoming dynamic first class players in real life. We've seen corporations become people. Algorithms will likely also follow that path to agency.

[Kevin Slavin](http://www.ted.com/speakers/kevin_slavin.html) in his intriguing TED talk: [How Algorithms Shape Our World](http://datascience.berkeley.edu/kevin-slavin-algorithms-shape-world/ ), gives many and varied examples of how algorithms have penetrated RL. 

One of his most interesting examples is from a highly technical paper on [Relativistic statistical arbitrage](http://www.alexwg.org/publications/PhysRevE_82-056104.pdf), which says to make money on markets you have to be where the people are, the red dots (on the diagram below), which means you have to put servers where the blue dots are, many of which are in the ocean. Here's the diagram from the paper:

![](http://farm6.staticflickr.com/5525/12326035505_c6a15e5288.jpg)

Mr. Slavin neatly sums this up by saying:

> And it's not the money that's so interesting actually. It's what the money motivates, that **we're actually terraforming the Earth itself with this kind of algorithmic efficiency**. And in that light, you go back and you look at Michael Najjar's photographs, and you realize that they're not metaphor, they're prophecy. They're prophecy for the kind of seismic, terrestrial effects of the math that we're making. And the landscape was always made by this sort of weird, uneasy collaboration between nature and man. But now there's this third co-evolutionary force: algorithms -- the Boston Shuffler, the Carnival. And we will have to understand those as nature, and in a way, they are.

The introduction to the paper spells out why this is so:

>     Recent advances in high-frequency financial trading have brought typical trading latencies below 500 microseconds, at which point light propagation delays due to geographically separated information sources become relevant for trading strategies and coordination e.g., it takes 67 ms, over 100 times longer, for light to travel between antipodal points along the Earth’s surface. Moreover, as trading times continue to decrease in coming years e.g., latencies in the microseconds are already being targeted by traders, this feature will become even more pronounced. Here we report a relativistic generalization of statistical arbitrage trading strategies for space like separated trading locations. In particular, we report two major findings. First, we prove that there exist optimal intermediate locations between trading centers that host cointegrated securities such that coordination of arbitrage trading from those intermediate points maximizes profit potential in a locally auditable manner. Second, we show that if such intermediate coordination nodes are themselves promoted to trading centers that can utilize local information, a novel econophysical effect arises wherein the propagation of security pricing information through a chain of such nodes is effectively slowed or stopped.    

We are not far from a world where the decision to build out these facilities will be driven by an optimization algorithm, as foreshadowed by Daniel Suarez in his ground breaking book [Daemon](http://www.amazon.com/Daemon-Daniel-Suarez/dp/0451228731).  

If you think this is horse puckey [in a recent talk](http://www.youtube.com/watch?v=eNliOm9NtCM) by Raymond Blum, team lead for Google Site Reliability Engineers, shows how deep is the drive for algorithmic automation. In [my gloss on the talk](http://highscalability.com/blog/2014/2/3/how-google-backs-up-the-internet-along-with-exabytes-of-othe.html) I summarize one of Mr. Blum's lessons:

> Take humans out of the loop. How many copies of an email are kept by GMail? It’s not something a human should care about. Some parameters are configured by GMail and the system take care of it. This is a constant theme. High level policies are set and systems make it so. Only bother a human if something outside the norm occurs.

The motivation for this way of looking at problems is simply **scale**. You can’t scale linearly at a large scale. You can’t have 100 times as much data require 100 times the people or machine resources. Automation is the key to improving utilization and efficiency. It's a force multiplier.

And when we say automation what do we really mean? Algorithms. And what drives algorithms? Telemetry and learning. The need for telemetry and control is part of the hype around the Internet of Things, and part of the hype around BigData is that it drives learning. But it's all about algorithms.

For another Google example take a look at [Spanner](http://research.google.com/archive/spanner.html): Google's scalable, multi-version, globally-distributed, and synchronously-replicated database. It is the first system to distribute data at global scale and support externally-consistent distributed transactions. 

Spanner operates on a truly global scale. And the only way it can do that is through algorithms that make decisions automatically about where to store data and where to move it to make the best use of underlying resources like storage capacity and bandwidth, while also providing the required redundancy and high performance. It's a job way too complex for any human to do or even manage.

Here's a quote from a [Wired article](http://www.wired.com/wiredenterprise/2012/11/google-spanner-time/) on Spanner: 

> One effect, Fikes explains, is that Google spends less money managing its system. “When there are outages, things just sort of flip — client machines access other servers in the system,” he says. “It’s a much easier service story…. The system responds — and not a human.”

As Google becomes ever more expert in [deep learning](http://www.technologyreview.com/news/524026/is-google-cornering-the-market-on-deep-learning/) and the physical world through APIs becomes more like infrastructure-as-a-service, algorithms will be able to truly mind blowing things, [besides losing 1000 points](http://en.wikipedia.org/wiki/2010_Flash_Crash) on the Dow Jones in just a few minutes. 

Mr. Slavin's talk gives many more examples, like book pricing on Amazon, Netflix recommendations, deciding which movies get made, optimal bin packing for elevators, cleaning robots, and a 800 mile trench built just to lay a fiber optical cable so a trading algorithm can run 3 milliseconds faster.  

It's a really eye opening talk, the key theme being how algorithms are increasingly mediating our world for us, making decisions for us, and shaping the world to make the algorithms work better. The upside is obvious. The downside is we may not like or even truly understand the shape of the world that algorithms create.

This future is here, now, and will become more evenly distributed over time because as Mr. Slavin smartly concludes: "it's a bright future if you're an algorithm."

    