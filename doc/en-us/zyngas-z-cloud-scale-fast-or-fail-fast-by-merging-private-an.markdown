## [Zynga's Z Cloud - Scale Fast or Fail Fast by Merging Private and Public Clouds](/blog/2011/5/19/zyngas-z-cloud-scale-fast-or-fail-fast-by-merging-private-an.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, May 19, 2011 at 9:06AM</span></div>

<div class="body">

![](http://farm5.static.flickr.com/4049/4335031559_fa690c1f81_m.jpg)

Release early and often. A/B testing. Creating a landing page and buying ads on AdSense. All are ways of providing quick feedback in order to validate an idea. If you are like Zynga, with 250 million active users a month, how do you cost effectively prove out a game that could flop or get 90 million users (like CityVille) in an instant?

Zynga handles this problem inlle an innovative way, by inverting the typical cloud burst scenario that has excess traffic flowing from a datacenter to a cloud, to having a game start in the cloud and then moving to the datacenter once the game has proved popular enough to keep.

This process is nicely described by Charles Babcock in [Lessons From FarmVille: How Zynga Uses The Cloud](http://www.informationweek.com/news/global-cio/interviews/229402805), in an interview with Allan Leinwand, CTO of infrastructure engineering at Zynga.

When paired down to its essence, Zynga's strategy goes something like this:

*   **Games are risky**.  Even with all their experience, Zynga can't know how many people will play a game or how fast the adoption rate will be. This makes capacity planning more like throwing a dart at a distant dart board, while blind folded, drunk, after having been spun around three times. Spending on a large infrastructure build out while in this condition could be costly. 90 million users have came from a new game, CityVille. Did they know CityVille would be so successful? With enough certainty that they would buy equipment for 90 million users? And for a spiking growth curve could they even requisition infrastructure fast enough to respond to demand?
*   **Zynga mitigates this risk by launching games using Amazon's** **EC2**. While EC2 is more expensive than Zynga's own datacenter, it's less expensive than doing a large infrastructure build out that sits unused, either because a game isn't popular or a game has a long ramp up time. Amazon is used as a small scale experimental apparatus that allows a mad scientist to pay only for the capacity used, yet it can still handle growth if a game becomes successful.
*   **After a game matures it is brought in house to their private Z Cloud**. Zynga leases datacenter space on the West and East coasts. They've built something they call the Z Cloud as their game infrastructure. When they can plot the slope of the growth curve for a game they can start capacity planning and buying datacenter space. The game is then moved into their Z Cloud.
*   **The public and private cloud are one**. Even when a game is moved into their private Z Cloud, parts of the game may reside on Amazon. The key is Zynga sees this as a hybrid environment. It's not a public cloud, or a private cloud, but one system to architect and manage. The strengths of each system are taken advantage of based on product lifecyle, costs, and feature requirements. 
*   **Like attracts like**. The Z Cloud works something like Amazon internally, so it's not a disruptive jump between the clouds. Like Amazon their cloud is based on virtualization and automation. Thousands of physical servers can easily be deployed in a day. Servers are highly standardized to make this process simpler. 

Zynga is as ever tight lipped about the details of their infrastructure, but that doesn't really matter here, the idea of this easy bidirectional flow between clouds is a powerful one. Also, Zynga's proving ground approach doesn't preclude cloud bursting when appropriate. The flexibility for managing costs and risks is enormous. If only there was a cloud platform that could make this easier. Oh wait, there's...[OpenStack](http://www.openstack.org/).

## Experiment Like MythBusters

I love the [MythBusters](http://dsc.discovery.com/tv/mythbusters/) as an example of going from small scale to a large scale experiments. I talk about them a bit in [What Should I Do? Choosing SQL, NoSQL or Both for Scalable Web Applications](http://www.slideshare.net/toddhoffious/what-should-ido-11):

![](http://farm4.static.flickr.com/3421/5734781020_d7b23e6b00.jpg)

## Related Articles

*   [How FarmVille Scales To Harvest 75 Million Players A Month](http://highscalability.com/blog/2010/2/8/how-farmville-scales-to-harvest-75-million-players-a-month.html)
*   [Clinical Trial Process](http://en.wikipedia.org/wiki/Clinical_trial)
*   [Trial by Fire](http://en.wikipedia.org/wiki/Trial_by_fire)

</div>