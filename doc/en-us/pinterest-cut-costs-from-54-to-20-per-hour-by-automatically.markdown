## [Pinterest Cut Costs from $54 to $20 Per Hour by Automatically Shutting Down Systems](/blog/2012/12/12/pinterest-cut-costs-from-54-to-20-per-hour-by-automatically.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, December 12, 2012 at 9:30AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7180/6886606039_bc5c70dbf9_m.jpg)

We've long known one of the virtues of the cloud is, through the magic of services and automation, that systems can be shut or tuned down when not in use. What may be surprising is how much money can be saved. 

This aspect of cloudiness got a lot of pub at [AWS re:Invent](https://reinvent.awsevents.com/) and is being rebranded under the term [Cost-Aware Architecture](https://medium.com/21st-century-architectures/8c07ed78d4d4). An [interesting example](http://techcrunch.com/2012/12/02/the-philosophy-behind-amazon-web-services-cloud-strategy/) was given by Ryan Park, Pinterest’s technical operations lead:

*   20% of their systems are shutdown after hours in response to traffic loads
*   Reserved instances are used for standard traffic 
*   On-demand and spot instances are used to handle the elastic load throughout the day. When more servers are needed for an auto-scaled service,  spot requests are opened and on-demand instances are started at the same time. Most services are targeted to run at about 50% on-demand and 50% spot.
*   Watchdog processes continually check what's running. More instances are launched when needed and terminated when not needed. If spot prices spike and spot instances are shut down, on-demand replacement instances are launched. Spot instances will be relaunched when the price goes back down. Spot capacity issues are rare and rarely are apparent to users.
*   Using this approach costs have gone from $54 per hour to $20 per hour
*   Only 2 weeks of engineering were required to build the system and with very little maintenance needed, it saves a lot of money.

We are in very early days of Adaptive Architectures. What is being managed are relatively large resource abstractions running on relatively large scale computer abstractions. Yet it's a start. More at [Cloud Programming Directly Feeds Cost Allocation Back Into Software Design](http://highscalability.com/cloud-programming-directly-feeds-cost-allocation-back-software-design).

## Related Articles 

*   [On Hacker News](http://news.ycombinator.com/item?id=4993753)
*   [AWS re:Invent Werner Vogel Keynote Live Blog](http://www.aarondelp.com/2012/11/aws-reinvent-werner-vogel-keynote-live.html) by the quick typing Aaron Delp 
*   [Pinterest Architecture Update - 18 Million Visitors, 10x Growth,12 Employees, 410 TB Of Dat](http://highscalability.com/blog/2012/5/21/pinterest-architecture-update-18-million-visitors-10x-growth.html)
*   [A Short On The Pinterest Stack For Handling 3+ Million Users](http://highscalability.com/blog/2012/2/16/a-short-on-the-pinterest-stack-for-handling-3-million-users.html)

</div>