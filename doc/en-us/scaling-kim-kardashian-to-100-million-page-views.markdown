## [Scaling Kim Kardashian to 100 Million Page Views](/blog/2015/2/16/scaling-kim-kardashian-to-100-million-page-views.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, February 16, 2015 at 9:02AM</span></div>

<div class="body">

![](https://farm9.staticflickr.com/8636/16548853301_66d73f52b2_o.jpg)

<span id="docs-internal-guid-e50a42f4-9343-20ff-cdaa-23fff595f737"></span>

<span>The team at</span> [<span>PAPER</span>](http://www.papermag.com/) <span>estimated their</span> [<span>article</span>](http://www.papermag.com/2014/11/kim_kardashian.php) <span>(NSFW) containing pictures of a very naked</span> [<span>Kim Kardashian</span>](http://en.wikipedia.org/wiki/Kim_Kardashian) <span>would quickly receive over 100 million page views. The very definition of bursty viral driven traffic.</span>

<span>As a comparison in 2013 it</span> [<span>was estimated</span>](http://www.cnet.com/news/google-search-scratches-its-brain-500-million-times-a-day/) <span>Google processed over 500 million searches a day. So a nude Kim Kardashian is worth one-fifth of a Google. Strangely, I can believe it.</span>

<span>How did they handle this traffic gold mine? A complete recounting of the unusual behind the scenes story is told by Paul Ford in</span> [<span>How PAPER Magazine’s web engineers scaled their back-end for Kim Kardashian</span>](https://medium.com/message/how-paper-magazines-web-engineers-scaled-kim-kardashians-back-end-sfw-6367f8d37688) <span>(SFW).  (BTW, only one butt pun was made intentionally in this story, all others are serendipity).</span>

<span>What can we learn from the experience? I know what you are thinking. This is just a single static page with a few static pictures. It’s not a complex problem like</span> [<span>search</span>](http://highscalability.com/display/Search?moduleId=4876569&searchQuery=google) <span>or a</span> [<span>social network</span>](http://highscalability.com/blog/2009/10/13/why-are-facebook-digg-and-twitter-so-hard-to-scale.html)<span>. Shouldn’t any decent CDN be enough to handle that? And you would be correct, but that’s not the whole of the story:</span>

1.  **Bursty traffic**. There’s an informal rule that a site should be engineered to handle bursts of an order of magnitude more than typical traffic. PAPER normally handles hundreds of thousands of people in a given month, so two orders of magnitude more traffic in a compressed period of time definitely counts as something to worry about.

2.  <span>**Plan ahead**</span><span>. Rather than trust to fate, PAPER knew they had a big story, so they contacted their web infrastructure team and gave them five days lead time to make sure the site could handle the coming crush of viewers.</span>

3.  **Pre-Kardashian Stack**. PAPER ran in a single AWS zone on a single instance (size unknown), MySQL was probably the database, [<span>Moveable Type</span>](https://movabletype.org/) is the CMS, [<span>Tornado</span>](http://www.tornadoweb.org/en/stable/) is the web server, [<span>Varnish</span>](https://www.varnish-cache.org/) is the cache. This setup served between a half-million and a million unique visitors a month.

4.  **Post-Kardashian Stack**. The pre-Kardashian site was transformed into a site using a pre-warmed load balancer, four front-end machines, a shared filesystem, and Amazon’s CloudFront CDN. Here’s a [<span>nice diagram</span>](https://d262ilb51hltx0.cloudfront.net/max/2000/1*NRRjxiTzjIFBK4UlJ3m2ww.jpeg) of the stack PAPER used to handle the photo release. Four m3.large front-end machines were added that hosted Moveable Type, Tornado, and Varnish. Then a distributed file system layer was built on [<span>GlusterFS</span>](http://glusterfs), a scale-out network-attached storage file system. All the media was copied to a Gluster Filesystem Server that ran on an m3.xlarge. More storage nodes could be added as needed, which is what Gluster brought to the table. The front-end machines used Gluster as their virtual disk. ELB was used to balance traffic across the front-end machines.

5.  <span>**Testing**</span><span>. The CDN will handle the load for the static content, Varnish will cache the website’s dynamic content, and the ELB will distribute traffic, but the website must still handle the traffic.</span> [<span>Bees with Machine Guns</span>](https://github.com/newsapps/beeswithmachineguns) <span>was used to test the system at 2,000 requests per second. The goal was to test at 10,000 requests per second, but apparently the tool couldn’t be made to reach that rate.</span>

6.  <span>**Emergency response planning**</span><span>. It was estimated the system could handle 30 million visitors in the first hour, but there was a plan on how to start more front-end servers as needed. The plan didn’t use Chef, Puppet, or even Docker, apparently the needed commands were stored in a Google Docs document.</span>

7.  **Instagram stole the traffic!** Content creators often lose out to content aggregators. Traffic to PAPER’s site was far less than hoped for. This says it all: “Kardashian had tweeted out the news — but pointed to the photo on her Instagram account. Her 25 million followers went there instead of to PAPER’s website.” A word to the wise: **part of your rollout plan should include strategies that make sure you get most of the benefit from your work**.

8.  <span>**PAPER retaliates by going full frontal**</span><span>. PAPER took back the initiative from Instagram by publishing a full-frontal nude shot of Kardashian. Instagram doesn’t allow these sort of pics, so this time traffic flowed to PAPER. Traffic jumped, with 30 million unique visitors over a few days. The site handled all the traffic with no problems.</span>

9.  **Back to the Pre-Kardashian Stack**. A few weeks later the new stack was torn down and the old stack was back running the site. Not exactly the poster child for auto scalability, but still an impressive degree of flexibility.

10.  <span>**Can your comment system handle the load?**</span> <span>This kind of article will get a lot of comments. Don’t forget the load on your comment system. If it fails you’ll look bad, even if the rest of the site is humming along just fine. PAPER uses Facebook’s comment plugin which seems to handle the 5,100+ comments with no problems.</span>

11.  <span>**Estimation?**</span> <span>One part of the story that I would be interested in is how PAPER projected the page views at 100 million? That estimate drove all the changes. If it was too large then they wasted a lot of money. If it was too small they would get crushed in more ways than one. What’s the process for that sort of projection?</span>

<span>This article is unique, I don’t think I’ve ever read anything quite like it. It brings together a lot of different topics and audiences into one place and tries to relate them all...while be entertaining. </span>If you are a DevOp the article won’t have a lot for you. Yet it does a great job at addressing a general audience, explaining to them what it takes to run a website. Or as anonymous benefactor wrote to me:

> <span>I liked it because it can be read by nearly anyone - including the many non-technical people who need to understand the message about scaling with cloud resources - to get an understanding of the motivation, activities, and benefits of building on a cloud platform and using it for challenges that would break non-cloud IT infrastructure. A good story, told in sufficient detail, delivering a useful message about scaling.</span>

<span>No doubt some of the decisions are debatable, but they always are. Paul Ford had a great response to that angle of attack:</span>

> <span>One of the things nerds love to do is look at other people’s stacks and say, “what a house of cards!” In fact I fully expect people to link to this article and write things like, “sounds okay, but they should have used Jizzawatt with the Hamstring extensions and Graunt.ns for all their smexing.”</span>

## <span>Related Articles</span>

*   [On Reddit](http://www.reddit.com/r/programming/comments/2w3mis/scaling_kim_kardashian_to_100_million_page_views/)

*   <span>[Strategy: Three Techniques To Survive Traffic Surges By Quickly Scaling Your Site](http://highscalability.com/blog/2014/3/19/strategy-three-techniques-to-survive-traffic-surges-by-quick.html)</span>

*   [<span>Super Bowl Advertisers Ready For The Traffic? Nope..It's Lights Out</span>](http://highscalability.com/blog/2013/2/6/super-bowl-advertisers-ready-for-the-traffic-nopeits-lights.html)<span>.</span>

*   [<span>22 Recommendations For Building Effective High Traffic Web Software</span>](http://highscalability.com/blog/2013/12/16/22-recommendations-for-building-effective-high-traffic-web-s.html)

*   [<span>StubHub Architecture: The Surprising Complexity Behind The World’s Largest Ticket Marketplace</span>](http://highscalability.com/blog/2012/6/25/stubhub-architecture-the-surprising-complexity-behind-the-wo.html)

*   [<span>Handle 1 Billion Events Per Day Using A Memory Grid</span>](http://highscalability.com/handle-1-billion-events-day-using-memory-grid)

*   [<span>Tech Tuesday: Our Technology Stack</span>](http://imgur.com/blog/2013/06/04/tech-tuesday-our-technology-stack/)

*   <span>[Two Issues with Putting Varnish in Front of the ELB](http://www.reddit.com/r/devops/comments/2t8ib2/how_paper_magazines_web_engineers_scaled_their/cnx7n13)</span>

</div>