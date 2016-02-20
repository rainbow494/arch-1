## [Here's a 1300 Year Old Solution to Resilience - Rebuild, Rebuild, Rebuild](/blog/2014/4/23/heres-a-1300-year-old-solution-to-resilience-rebuild-rebuild.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, April 23, 2014 at 8:56AM</span></div>

<div class="body">

![](https://farm4.staticflickr.com/3711/13939473364_567efc92e9_n.jpg)

How is it possible that a wooden Shinto shrine built in the 7th century is still standing? The answer depends on how you answer this philosophical head scratcher: With nearly every cell in your body continually being replaced, are you still the same person?

The [Ise Grand Shrine](http://en.wikipedia.org/wiki/Ise_Grand_Shrine) has been in continuous existence for over 1300 years because every twenty years an exact replica has been rebuilt on an adjacent footprint. The former temple is then dismantled.

Now that's resilience. If you want something to last make it a living part of a culture. It's not so much the building that is remade, what is rebuilt and passed down from generation to generation is the meme that the shrine is important and worth preserving. The rest is an unfolding of that imperative.

You can see echoes of this same process in Open Source projects like Linux and the libraries and frameworks that get themselves reconstructed in each new environment.

The patterns of recurrence in software are the result of Darwinian selection process that keeps simplicity and value alive in human minds. 

A blog post on [Persuing Wabi](http://pursuingwabi.com/2013/06/30/ise-grand-shrine-part-1-geku/) has some fabulous photos of the shrine along with a brief description of why it's the way it is:

> The shrines and any associated structures such as bridges and towering torii gates are totally rebuilt every 20 years on an adjacent site. This year, 2013,  is when the newly rebuilt structures will be put to use.  Each rebuild costs about 1/2 billion US dollars, half of which is paid with tax dollars. Each rebuild requires 10,000 cedar trees.  The main pillars from the old structures are used for torii gates in the new shrine.  All the other lumber from the existing structure are distributed around the country to other shrines that need repair.
> 
> Why rebuild every 20 years?  It follows the shinto belief of death and renewal of nature.  The Shinto religion stresses harmony with nature.  It also helps to preserve the building techniques from one generation to the next.  Another reason is that the shrines follow the architectural style of grain houses from the Yayoi period (300 BC to 300 AD), which stored seed rice in case of famines.  The roofs of these warehouses had a thatched roof, which becomes heavier over time as they absorb water.  After 20 to 30 years, the structure could no longer support the roof.  Because the warehouse was vital, a new warehouse would begin construction while the existing structure could still be used. 

More on how the structure design led to the [need for periodic renewal](http://www.japanfs.org/en/news/archives/news_id034293.html):

> This kind of grain warehouse was normally supported by more than a dozen pillars sunk directly into the ground and had a thatched roof. A great deal of rain usually falls in Japan's early-summer monsoon, and as the thatched roof absorbs rainwater it becomes heavier. The heavy roof presses down on the walls, and this closes gaps between the wall boards, keeping the inside dry. In summer, the roof dries out and becomes lighter, allowing air to pass through the building and this also keeps it dry. Thus, the roof and pillars function together like a living organism to securely protect the seed rice from moisture and pests.
> 
> The only way to support a thatched roof designed to increase in weight is to set the pillars directly into the ground. However, with this method, the pillars and the thatched roof eventually start to rot. Thus, the inevitable solution was to reconstruct these warehouses every 20 to 30 years. However, the life-giving seed rice could not be protected if the rebuilding process started only after the old warehouses could no longer be used. Thus, periodic reconstruction of these structures probably became customary, leading eventually to the Sengu ceremonies of Jingu Shrine in Ise, symbolizing buildings that protect life.

The shrine itself is built in an ancient and unique architectural style called [shimmei-zukuri](http://www.aisf.or.jp/~jaanus/deta/s/shinmeizukuri.htm), a style characterized by extreme simplicity of design. The basic layout is a stylized form of the simple warehouses, granaries, and utility buildings existing before the introduction of Buddhist architecture in Japan circa 250 C.E.

Much like how ship builders used to plant oak trees to ensure replacement parts for their ships over time, a special grove of cedar trees are grown specifically so that they may be harvested in time for the next rebuilding.

## Is the shrine just an intriguing slice of history that has nothing to do with the world of technology?

I don't think so. I see a thread of humanity that stretches all the way back to the shrine and forward to our modern software culture. 

There's a direct mapping of the shrine rebuild pattern in an **upgrade practice** that started with the advent of the cloud. Traditionally machines have their software upgraded in place. On the cloud spinning up new machines is simple and cheap, so you can spin up an entirely new cluster on the new software and then just stop the old cluster. What's old is new again.

There's a less direct analogy in an **Open Source project** like Linux. That there's a priesthood in charge of both projects is is one obvious parallel :-) What's really key is that in both projects it's the community itself that is continually renewed, year after year after year. Will Linux last 1300 years? Not likely, but I would not be surprised if something traceable back to Linux is recognizable in the far future. 

The continual reincarnation of **libraries and frameworks** is another parallel. In software we may not rebuild the same exact structure, but we do keep rebuilding the same things over and over again, just in slightly different materials. Move to Go and you need all the same web server, security, APIs, libraries, etc that were just built in Node, C,  C++, Haskell, Scala, Erlang, Dart, Ruby, Python, or JavaScript. For mobile we need to redo huge chunks of the infrastructure that were already built for browsers and servers. Got virtualization? Now you have to reimplement most of that same stuff for containers. How many different key-value database do we have? Logging systems? Package managers? Map Reducers? And so on.

The books we have from antiquity survived because people copied them. Presumably the books chosen to be copied had a value that made the immense expense of copying worth the effort. So the ancient books that have survived to the present are the result of a Darwinian selection process. The shrine as an example of extreme simplicity and functionality must have also had great value to the community.

Perhaps the patterns of recurrence we see in software are not just a wasteful silliness, but a familiar example of a Darwinian selection process, where simplicity and value are kept alive in a community of human minds. 

<span style="font-size: 1.5em;">Related Articles </span>

*   Some beautiful pictures from a trip. [Part 1](http://pursuingwabi.com/2013/06/30/ise-grand-shrine-part-1-geku/) and [part 2](http://pursuingwabi.com/2013/07/14/ise-grand-shrine-part-2-naiku/).
*   [Alexander Rose Visits Ise Shrine Reconstruction Ceremony](http://blog.longnow.org/02013/10/03/alexander-rose-visits-ise-shrine-reconstruction-ceremony/). 
*   [Rebuilding Every 20 Years Renders Sanctuaries Eternal -- the Sengu Ceremony at Jingu Shrine in Ise](http://www.japanfs.org/en/news/archives/news_id034293.html)

</div>