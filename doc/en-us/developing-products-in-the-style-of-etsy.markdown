## [Developing Products in the Style of Etsy](/blog/2015/6/1/developing-products-in-the-style-of-etsy.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, June 1, 2015 at 8:56AM</span></div>

<div class="body">

![](https://farm9.staticflickr.com/8812/18245410766_7c1ea15a16_m.jpg)

How should you go about structuring your project? We have two general paradigms that I'll characterize as flowing from the Etsy [coaching tree](http://en.wikipedia.org/wiki/Coaching_tree), emphasising the monolith, and from the Netflix coaching tree, emphasizing microservices. This is of course an over simplification, but it's for instructional purposes only. For a broad comparison of the two approaches take a look at [The Great Microservices Vs Monolithic Apps Twitter Melee](http://highscalability.com/blog/2014/7/28/the-great-microservices-vs-monolithic-apps-twitter-melee.html).

This is not a good vs. evil sort of mythos. The Force is truly one. We simply have two valid and functional ways of looking the world.

I think [wdewind](https://news.ycombinator.com/item?id=9291630) nails the heart of the difference:

> The point of the article is that local optimization gives you this tiny boost in the beginning for a long term cost that eventually moves the organization is a direction of shipping less. It's not that innovative technologies are bad.

The mentioned article is [Choose Boring Technology](http://mcfunley.com/choose-boring-technology) by Dan McKinley, in which Dan does a great job exploring Etsy style development with both insight and wisdom. 

Dan explores four different principles:

*   **Embrace Boredom**. Choose tried and true technology that is well understood. Examples: PHP, Memcached; Cron; Squid; Python; MySQL. With new technology the unknowns pose a significant risk. While not a new idea, it's one of those principles that it helps to have solid backup for if your are the one on a team advocating for the boring instead of the new and shiny. What's new is the idea of giving your project N new technology tokens that you can spend on trying something risky. This bounds your risk profile while giving you flexibility to evolve over time.
*   **Optimize Globally**. Picking the right tool for the job ignores the organizational debt that accrues by selecting a tool that doesn't fit within what an organization already knows how to do well. It creates baggage. You have to figure out how to test, install, monitor, etc. the new thing. "Your job is keeping the company in business, god damn it. And the 'best' tool is the one that occupies the 'least worst' position for as many of your problems as possible."
*   **Choose New Technology, Sometimes.** Pass any newtechnology through a gauntlet.First consider how to solve a problem without doing anything new. Usually what you have is good enough to get the job done. If you really need to do something new then talk about it. Figure out exactly why a new problem requires a new tool? If the new technology overlaps with existing functionality then be clear on how the migration will occur. 
*   **Just Ship.** There's a freedom in constraint. By intentionality structuring how problems are solved you are freeing developers from constantly waring with tools. Developers can focus on shipping solutions to business problems. 

Here are two competing views on the article from an informative discussion of the article [on Hacker News](https://news.ycombinator.com/item?id=9291215).

[MCRed](https://news.ycombinator.com/item?id=9291394):  

> This seems to be written from the "Engineers are monkeys" perspective. As if they spend their time flinging poo and you really need "solid" boing technology that's already well designed so the poo doesn't mess it up.
> 
> You shouldn't choose node.js or MongoDB because they are "innovative"-- but because they are poorly engineered. (Erlang did what node does but much better, and MongoDB is poorly engineered global write lock mess that is probably better now but whose hype way exceeded its quality for many years.)
> 
> The engineers are monkey's idea is that engineers can't tell the difference-- and it seems to be supported by the popularity of those two technologies.
> 
> But if you know what you're doing, you choose good technologies-- Elixir is less than a year old but its built on the boring 20 years of work that has been done in Erlang. Couchbase is very innovative but it's built on nearly a decade of couchdb and memcache work.
> 
> You choose the right technologies and they become silver bullets that really make your project much more productive.
> 
> Boring technologies often have a performance (in time to market terms) cost to them. Really you can't apply rules of thumb like this and the "innovation tokens" idea is silly.
> 
> I say this having done a product in 6 months with 4 people that should have taken 12 people 12 months to do, using Elixir (not even close to 1.0 of elixir even) and couchbase and trying out some of my "wacky" ideas for how a web platform should be built-- yes, I was using cutting edge new ideas in this thing that we took to production very quickly. The difference?
> 
> Those four engineers were all good. Not all experienced-- one had been programming less than a year-- but all good.
> 
> Seems everyone talks about finding good talent and how important that is but they don't seem to be able to do it. I don't know.
> 
> I do know is, don't use "engineers are monkies" rules of thumb-- just hire human engineers.

<div>[wdewind](https://news.ycombinator.com/item?id=9291630):</div>

<div>

> Having come from Etsy and witnessed the success of this type of thinking first hand, I think you missed the point of the article and I think you are using a tiny engineering organization (4 people) in your thinking, instead of a medium to large one (120+ engineers).
> 
> The problem isn't "we are starting a new codebase with 4 engineers, are we qualified to choose the right technology?" it's "we are solving a new problem, within a massive org/codebase, that could probably be solved more directly with a different set of technologies than the existing ones the rest of the company is using. Is that worth the overhead?" and the answer is almost always no. Ie: is local optimization worth the overhead?
> 
> Local optimization is extremely tempting no matter who you are, where you are. It's always easy to reach a point of frustration and come to the line of reasoning of "I don't get why we are wasting so much time to ship this product using the 'old' stuff when we could just use 'newstuff' and get it out the door in the next week." This happens to engineers of all levels, especially in a continuous deployment, "Just Ship" culture. **The point of the article is that local optimization gives you this tiny boost in the beginning for a long term cost that eventually moves the organization is a direction of shipping less. It's not that innovative technologies are bad.**
> 
> > But if you know what you're doing, you choose good technologies
> 
> No, if you know what you are doing you make good organizational decisions. It matters less what technology you use than that the entire organization uses the same technology. Etsy has a great engineering team and yet the entire site is written in PHP. I don't think there is a single engineer working at Etsy who thinks PHP is the best language out there, but the decision to be made at the time was "there is a site using PHP, some Python, some Ruby etc., how do we make this easier to work on?" Of those three python and ruby are almost universally thought of as better languages than PHP, but in this case the correct decision was picking a worse technology because more of the site was written in it, the existing infrastructure supported it more completely and so as an organization and a business we could get back to shipping products more quickly by all agreeing to use PHP. Etsy certainly does not think of its engineers as monkeys, quite the opposite.

The comparison of the Etsy style of product development with the Netflix style could not be more stark. With a microservices approach, while there are base standards, every team is able to not be boring, to optimize locally, choose new technologies freely, but they must of course still ship.

Shipping is the common thread, but the path to shipping is very different. And since both Etsy and Netflix ship, we are arguing about style, not substance. 

</div>

</div>