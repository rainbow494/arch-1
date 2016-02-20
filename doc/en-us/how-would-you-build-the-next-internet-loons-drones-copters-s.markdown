## [How would you build the next Internet? Loons, Drones, Copters, Satellites, or Something Else?](/blog/2014/1/22/how-would-you-build-the-next-internet-loons-drones-copters-s.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, January 22, 2014 at 9:06AM</span></div>

<div class="body">

If you were going to design a next generation Internet at the physical layer that routes around the current Internet, what would it look like? What should it do? How should it work? Who should own it? How should it be paid for? How would you access it?

It has long been said the Internet routes around obstacles. Snowden has revealed some major obstacles. The beauty of the current current app and web system is the physical network doesn't matter. We can just replace it with something else. Something that doesn't flow through choke points like [backhaul networks](http://en.wikipedia.org/wiki/Backhaul_(telecommunications)), [under sea cables](http://www.submarinecablemap.com/), and [cell towers](http://en.wikipedia.org/wiki/Cellular_network). What might that something else look like?

## Google's Loon Project

<span class="full-image-block ssNonEditable"><span>![](http://farm3.staticflickr.com/2849/12075370574_8775ed805d.jpg?__SQUARESPACE_CACHEVERSION=1390340995832)</span></span>

[Project Loon](http://en.wikipedia.org/wiki/Project_Loon) was so named because the idea was thought to be loony. Maybe not.

The idea is to float high-altitude balloons 20 miles in the air to create an aerial wireless network with up to 3G-like speeds. Signals travel through the balloon network from balloon to balloon, then to a ground-based station connected to an Internet service provider (ISP), then onto the global Internet.

There are elements here common to several of the aerial solutions. Stations in the air, be that a drone, balloon, copter, or satellite carries some sort of communication equipment. And some form of Aerial OS implementing data plane management along with a control plane and management plane, acts like a [cellular network](http://en.wikipedia.org/wiki/Cellular_network), with users being handed off to different stations as they move and as the stations move and as new stations are added or drop out of range. You can see this having some complexity, but it's doable. 

The weakness is that it plugs into the global Internet so it flows over all the same corrupted links. With enough coverage would it be possible to connect directly to devices with the signals being routed only in the air?

## The Drones are Here the Drones are Here

<span class="full-image-block ssNonEditable"><span>![](http://farm4.staticflickr.com/3748/12075972006_8a87795a7f.jpg?__SQUARESPACE_CACHEVERSION=1390342317837)</span></span>

This is a picture of the [Solara 60](http://titanaerospace.com/platforms/solara-60/) drone [from Titan Aerospace](http://spectrum.ieee.org/aerospace/aviation/introducing-solara-the-atmospheric-satellite). It's a beast. I has a 250 pound payload, which should be enough for communication equipment and perhaps enough memory and CPU power to be a decent caching node. It can reportedly fly at 65,000 ft for weeks powered by efficient solar cells. Of course power for the computing equipment is still an issue.

And oh look, it already supports [voice and data communications](http://titanaerospace.com/applications/voice-and-data/). As you might expect they are targeted initially at the military and law enforcement agencies, because they get all the cool expensive stuff first, but over time you can imagine costs dropping into the hobbyist range so that we could have a beowulf cluster of these things providing an alternative Internet.

The advantage over the Loon is the drone is not at the mercy of the wind, though the Aerial OS should be able to integrate stations from any platform. 

## PocketQube Satellite

<span class="full-image-block ssNonEditable"><span>![](http://farm4.staticflickr.com/3721/12075740365_b30623bdf6.jpg?__SQUARESPACE_CACHEVERSION=1390344092633)</span></span>

<div id="_mcePaste">PocketQubes is a new satellite design, using a 5cm cube, that can be launched to a 700 km orbit for about $20,000\. They even [have a kickstarter]( http://www.kickstarter.com/projects/pocketqube/want-to-build-a-satellite-but-dont-have-a-nasa-siz). Cubes can be stacked to create larger satellites. That's getting affordable and if you put enough of them up it could possibly make a good backbone for communication.</div>

## Swarming Copters

<span class="full-image-block ssNonEditable"><span>![](http://farm4.staticflickr.com/3812/12076271486_e5140921a9.jpg?__SQUARESPACE_CACHEVERSION=1390343613853)</span></span>

By now you've probably seen the spooky [video of the swarming nano-copters](http://www.youtube.com/watch?v=YQIMGV5vtd4). While these can't provide backbone Internet connectivity, they could provide local or even regional mesh connectivity and then integrate in with the Aerial OS to route to the broader Internet constructed from the other technologies.

In this type of architecture edge caching and edge computing strategies become key as latency requirements would dictate not accessing a centralized cloud for services.

The beauty of personal copters is these will be affordable so the pool of resources can easily grow and as people put more into operation. Something like how [BitTorrent](http://en.wikipedia.org/wiki/BitTorrent) makes use of p2p resources. What they lack in staying power and capacity could be made up for in quantity.

## Would it work?

Obviously this is not a fleshed out proposal. Technical issues abound. Every idea explodes into a tree of implementation details many levels deep, including RF, power, governance, security, protocol design, and so on. 

What is exciting is it seems technologies are coming together so that we can do something different. Something beyond a completely closed system of cell towers and fiber backhauls.

You can start to see pools of communication resources dynamically coming together and being managed by an Aerial OS that connects and routes everything together over a diverse infrastructure owned and run by a diverse set of participants. And with a different infrastructure in place you can start to imagine different ways of [structuring data and applications](http://highscalability.com/blog/2009/12/16/building-super-scalable-systems-blade-runner-meets-autonomic.html) to utilize massively distributed compute resources.

It's a little bit chaotic. But it's a little bit exciting too.

Or do you have something completely different in mind?

[On Hacker News](https://news.ycombinator.com/item?id=7108309)

</div>