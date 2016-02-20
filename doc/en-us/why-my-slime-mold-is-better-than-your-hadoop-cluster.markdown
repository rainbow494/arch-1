## [Why My Slime Mold is Better than Your Hadoop Cluster](/blog/2012/4/9/why-my-slime-mold-is-better-than-your-hadoop-cluster.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, April 9, 2012 at 9:15AM</span></div>

<div class="body">

**Update**: [Organism without a brain creates external memories for navigation](http://arstechnica.com/science/2012/10/organism-without-a-brain-creates-external-memories-for-navigation/) shows slime mold is even cooler than originally thought, storing a record of where it's been using slime: The authors conclude, the slime isn't just the mold's calling card. Instead, it's a way of marking the environment so that the organism can sense where it's been, and not expend effort on searches that won't pay off. Although the situation isn't an exact parallel, the authors make a comparison to the pheromone trails used by ants. 

In [<span>After Life: The Strange Science Of Decay</span>](http://www.youtube.com/watch?feature=player_embedded&v=sNAxrpzc6ws#) <span>there’s a truly incredible sequence of gorgeously shot video showing how</span> [<span>creeping slime mold</span>](http://waynesword.palomar.edu/slime1.htm) <span>solves mazes and performs other other amazing feats of computation. Take a look at what simple one celled organisms can do:</span>  

<iframe width="400" height="233" src="http://www.youtube.com/embed/sNAxrpzc6ws#t=63m45s" frameborder="0" s=""></iframe>  

<span>The whole video is really well done and shockingly revelatory. It’s the story of decay, how atoms created during the Big Bang and through countless supernova explosions are continually rearranged and reused by the complex process of life.</span>  

<span>The most glaring take away for me was how sterile is the world of computing.</span>  

<span>In the real-world material is plentiful and follows basic physical and chemical rules for self-assembly. Inside cells chemicals and bits of RNA and DNA whiz around each other at amazing rates of speed. They crash into each other and if there's a fit a reaction takes place and something larger is created. And the process continues until larger and larger structures are built. All without organization. Everything happens because there are elements with basic properties, supplies of those elements, ways of those elements coming into contact with each other, ways for those elements to combine together, and ways for those elements to be torn apart and recycled.</span>  

<span>Code, which you would think would be the most alive part of our systems, is as dead as statues in a church. Code is carefully constructed, deployed, groomed, and maintained. Code is no more alive than a golem.</span>  

<span>Data is far worse off than code. Data has no independent reality or opportunity for serendipitous creation. Data is dead. A data physics, an attempt to give life to data in the same way elements in the table elements come alive when brought together in a physical world. Imbue data with a vitality and create a physical universe in which they can combine according to their nature. That’s what we need to make computing come alive.</span>

## <span>The Circle of Life</span>

<span>To have something to decay it must have been alive at some point in time. That begs the question of what is life?</span>  

<span>Stuart Kauffman defines life as a</span> [<span>collectively autocatalytic system</span>](http://vastaanotin.tv.funet.fi/cgi-bin/tv?mrl=rtmp://salama.tv.funet.fi:21015/simplevideostreaming/fi/tut/sgn/kauffman/Lecture_2010-04-26.mp4)<span>:</span>

> <span>No single molecule knows how to replicate, but together they do. There are thousands of kinds of molecules in your cells. No molecule knows how to replicate. DNA requires RNA, which requires proteins, which require the translation of RNA to proteins, which requires proteins to do the translation, which are called synthetase, they load the right amino acids on to the right transfer RNAs that bind to the right sites on the messenger RNA. A cell is a collectively autocatalytic system. All free living organisms are collectively autocatalytic systems, in richly coupled cross catalytic network.</span>
> 
> As the diversity of molecules increases the number of interactions increases faster. At some point there's a phase transition and you just get popping out at you the emergence of collectively [autocatalytic sets](http://en.wikipedia.org/wiki/Autocatalytic_set). It's not reducible to the underlying physics.

<span>This documentary picks up where Kauffman leaves off, but what we are seeing is a higher level autocatalytic system composed not of atoms, but of what happens to those atoms once they’ve got the life ball rolling. And that is, unfortunately, death.</span>  

<span>Death followed by decay provides the building blocks of life. Nutrients locked in plants and animals are freed by an intricate cycle of bacteria, mold, fungi, and beetles who work to decompose every living thing. A process dramatically shown by the video. Life has evolved this glorious system of converting simple atoms to complex entities and for then ripping them back down to simple parts. Atoms are reused in an endless cycle. Queue the</span> [<span>Lion King</span>](http://www.youtube.com/watch?v=vX07j9SDFcc)<span>, but it is a beautiful thing.</span>

## <span>Slime Mold - The Decomposers</span>

<span>Slime mold is part of the decomposition process. They feed on decaying vegetation, bacteria, fungi, and even other slime molds.  Largest of the single celled organisms, they can grow to more than 3 square meters. Single celled does not mean boring. Slime molds are</span> [<span>self organized systems</span>](http://en.wikipedia.org/wiki/Self-organization)<span>, like bird flocking, that has no leader and no central point of control, yet it acts a single unit.</span>  

<span>Professor</span> [<span>John Tyler Bonner</span>](http://en.wikipedia.org/wiki/John_Tyler_Bonner) <span>has spent a lifetime studying slime molds has this to say about these incredible beings:</span>

> <span>Slime molds are "no more than a bag of amoebae encased in a thin slime sheath, yet they manage to have various behaviours that are equal to those of animals who possess muscles and nerves with ganglia -- that is, simple brains."</span>

<span>Fascinating as these beings are, what’s cool for computer people is the solution they’ve evolved to solve the problem of efficiently finding and distributing food that seems more elegant than any human solution to the</span> [<span>Travelling salesman problem</span>](http://en.wikipedia.org/wiki/Travelling_salesman_problem)<span>.</span>

## <span>Amazing</span>

<span>Slime mold can</span> [<span>solve simple mazes</span>](http://www.youtube.com/watch?v=75k8sqh5tfQ)<span>. Put food at the center of a maze and the slime mold will find the quickest route to the food.</span>  
<span class="full-image-block ssNonEditable"><span>![](http://farm6.staticflickr.com/5448/6911878342_8a8be207f6_m.jpg?__SQUARESPACE_CACHEVERSION=1333919795832)</span></span>

*   <span>Typically a maze is solved by visiting all possible passages and marking them as visited until the exit is found. This is a serial approach.</span>
*   <span>Slime mold solves a maze in one pass without visiting all passages by following the food gradient created by the food inserted into the maze. It does not always find the shortest path and sometimes it finds the longest path.</span>

## <span>Better Network Designers than Humans</span>

<span>Mazes are just the start. Computation problems can be simulated by distributing food sources and letting the slime mold forage. In the process they’ll create a network.</span>  

<span class="full-image-block ssNonEditable"><span>![](http://farm8.staticflickr.com/7103/6909163662_1fe391ede6_m.jpg?__SQUARESPACE_CACHEVERSION=1333919815327)</span></span>

*   <span>As an example, lay food representing the distribution of cities around Tokyo. Apparently slime mold loves oats so they use oats as the food source. Let’s see what kind of network the slime mold comes up with. How will it compare with the rail network designed by humans?</span>
*   <span>The network that the slime mold came up with to link all its food sources is nearly identical the rail network created by humans to link up cities around Tokyo by rail. In fact, it’s better in that it’s more resilient, it’s more reliable as the slime mode has built in more redundant links. The slime mold has figured out how to efficiently link together multiple locations, without the entire toolkit available to humans.</span>
*   <span>Start the experiment by setting a chunk of slime mold near the oats. The slime mold grows out in all directions until encompasses all the food sources.</span>
*   <span>Within hours it shrinks back leaving an intricate web of tubes linking all the food sources.</span>
*   <span>The tubes are used to transfer nutrients to the slime mold. The entire network is part of one single cell. Watch the video, it’s stunning to watch.</span>
*   <span>Slime mold is constantly pulsating as it creeps around looking for food. As one part of it finds something to eat it pulses more rapidly. Scientists believe it’s this pulsing that transmit information across the entire cell allowing the slime mold to move towards it food source. These pulsations control where and how they grow across the forest floor and around the food sources. With this approach slime mold can make multiple decisions simultaneously.</span>
*   <span>It’s building an efficient network to transport food around, yet at the same time the network it builds can’t cost too much, it can’t use too many resources.</span>
*   <span>Since the network is subject to damage the slime builds in redundant links between locations to make sure its food supply is not cut off.</span>

<span>The slime mold is showing smart behaviour. It hasn’t got a brain, yet it can solve complex problems with simple rules.</span>

## <span>Daleks!</span>

<span>I know what you are thinking. Make a cyborg! Meet the</span> [<span>phi-bot</span>](http://eprints.soton.ac.uk/268247/1/TsudaS08AlifeInHardware.pdf)<span>, a robot controlled by slime mold that sits on a chip.</span>

<span class="full-image-block ssNonEditable"><span>![](http://farm8.staticflickr.com/7238/6911918358_d0cb08b412_m.jpg?__SQUARESPACE_CACHEVERSION=1333919833078)</span></span>  

<span>The chip detects these pulses and translates them into robot movement. Yes, it was inspired by Dalek’s from Dr. Who. How cool is that! The idea is the slime mold process information in completely different way than standard algorithms and they are trying to learn more about how that works.</span>

If you have a little Maker in you, you can actually [buy your own slime mold](http://www.carolina.com/product/physarum+culture+kit.do). Or just wait awhile and I imagine we may see a specialized Slime Mold Cloud in the future. With Mechanical Turk Amazon put an API in front of a collection of humans, with new robot technology why not put an API in front of slime mold and create a Slime Mold Instance?

## <span>Related Articles</span>

*   [<span>Slime mould solves maze in one pass ... assisted by gradient of chemo-attractants</span>](http://arxiv.org/abs/1108.4956)
*   [<span>The Phi-Bot: A Robot Controlled by a Slime Mould</span>](http://eprints.soton.ac.uk/268247/1/TsudaS08AlifeInHardware.pdf)
*   <span>[Slime Mold Photos](http://waynesword.palomar.edu/slime1.htm)</span>
*   [<span>Decomposers in the Food Chain</span>](http://books.google.com/books?id=5xTDsmrTun8C&pg=PT18&lpg=PT18&dq=creeping+slime+mold&source=bl&ots=4EF-XWVPbm&sig=zfqqtGGL-1NuXzVVhMzZhbqgFbk&hl=en&sa=X&ei=E-yBT7DJG6zZiQLs3NmZAw&ved=0CIUBEOgBMA0#v=onepage&q=creeping%20slime%20mold&f=false)
*   <span>[What a maze-solving oil drop tells us of intelligence](http://www.redicecreations.com/article.php?id=9598)</span>

</div>