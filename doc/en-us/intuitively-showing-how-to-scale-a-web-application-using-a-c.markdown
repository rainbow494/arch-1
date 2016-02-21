## [Intuitively Showing How To Scale a Web Application Using a Coffee Shop as an Example](/blog/2014/3/17/intuitively-showing-how-to-scale-a-web-application-using-a-c.html)

    

    

_This is a [guest repost](http://www.sgdevs.com/2014/03/scaling-coffee-shop-web-application.html) by [Sriram Devadas](http://www.sgdevs.com/), Engineer at Vistaprint, Web platform group. A fun and well written analogy of how to scale web applications using a familiar coffee shop as an example. No coffee was harmed during the making of this post._

I own a small coffee shop.  

My expense is proportional to resources  
100 square feet of built up area with utilities, 1 barista, 1 cup coffee maker.  

My shop's capacity  
Serves 1 customer at a time, takes 3 minutes to brew a cup of coffee, a total of 5 minutes to serve a customer.  

Since my barista works without breaks and the German made coffee maker never breaks down,  
my shop's maximum throughput = 12 customers per hour.

    [![](http://farm3.staticflickr.com/2815/13221217823_9c718efa70_o.jpg)](http://farm3.staticflickr.com/2815/13221217823_9c718efa70_o.jpg)    

### Web server

Customers walk away during peak hours. We only serve one customer at a time. There is no space to wait.  

I upgrade shop. My new setup is better!  

Expenses  
Same area and utilities, 3 baristas, 2 cup coffee maker, 2 chairs  

Capacity  
3 minutes to brew 2 cups of coffee, ~7 minutes to serve 3 concurrent customers, 2 additional customers can wait in queue on chairs.  

Concurrent customers = 3, Customer capacity = 5

    [![](http://farm8.staticflickr.com/7429/13221217523_7446de710d_o.jpg)](http://farm8.staticflickr.com/7429/13221217523_7446de710d_o.jpg)    

### Scaling vertically

Business is booming. Time to upgrade again. Bigger is better!  

Expenses  
200 square feet and utilities, 5 baristas, 4 cup coffee maker, 3 chairs  

Capacity goes up proportionally. Things are good.  

In summer, business drops. This is normal for coffee shops. I want to go back to a smaller setup for sometime. But my landlord wont let me scale that way.  

Scaling vertically is expensive for my up and coming coffee shop. Bigger is not always better.

    [![](http://farm4.staticflickr.com/3697/13221217253_46526faf20_o.jpg)](http://farm4.staticflickr.com/3697/13221217253_46526faf20_o.jpg)    

### Scaling horizontally with a Load Balancer

Landlord is willing to scale up and down in terms of smaller 3 barista setups. He can add a setup or take one away if I give advance notice.  

If only I could manage multiple setups with the same storefront...  

There is a special counter in the market which does just that!  
It allows several customers to talk to a barista at once. Actually the customer facing employee need not be a barista, just someone who takes and delivers orders. And baristas making coffee do not have to deal directly with pesky customers.  

Now I have an advantage. If I need to scale, I can lease another 3 barista setup (which happens to be a standard with coffee landlords), and hook it up to my special counter. And do the reverse to scale down.  

While expenses go up, I can handle capacity better too. I can ramp up and down horizontally.

    [![](http://farm3.staticflickr.com/2813/13221065825_fdec77834d_o.jpg)](http://farm3.staticflickr.com/2813/13221065825_fdec77834d_o.jpg)    

### Resource intensive processing

My coffee maker is really a general purpose food maker. Many customers tell me they would love to buy freshly baked bread. I add it to the menu.  

I have a problem. The 2 cup coffee maker I use, can make only 1 pound of bread at a time. Moreover it takes twice as long to make bread.  

In terms of time,  
1 pound of bread = 4 cups of coffee  

Sometimes bread orders clog my system! Coffee ordering customers are not happy with the wait. Word spreads about my inefficiency.  

I need a way of segmenting my customer orders by load, while using my resources optimally.

    [![](http://farm3.staticflickr.com/2882/13221065675_8d797b646c_o.jpg)](http://farm3.staticflickr.com/2882/13221065675_8d797b646c_o.jpg)    

### Asynchronous Queue based processing

I introduce a token based queue system.  

Customers come in, place their order, get a token number and wait.  
The order taker places the order on 2 input queues - bread and coffee.  
Baristas look at the queues and all other units and decide if they should pick up the next coffee or the next bread order.  
Once a coffee or bread is ready, it is placed on an output tray and the order taker shouts out the token number. The customer picks up the order.  

- The inputs queues and output tray are new. Otherwise the resources are the same, only working in different ways.  

- From a customers point of view, the interaction with the system is now quite different.  

- The expense and capacity calculations are complicated. The system's complexity went up too. If any problem crops up, debugging and fixing it could be problematic.  

- If the customers accept this asynchronous system, and we can manage the complexity, it provides a way to scale capacity and product variety. I can intimidate my next door competitor.

    [![](http://farm8.staticflickr.com/7018/13221066345_15f7cb3542_o.jpg)](http://farm8.staticflickr.com/7018/13221066345_15f7cb3542_o.jpg)    

### Scaling beyond

We are reaching the limits of our web server driven, load balanced, queue based asynchronous system. What next?  

My already weak coffee shop analogy has broken down completely at this point.  

Search for DNS round robin and other techniques of scaling if you are interested to know more.  

If you are new to web application scaling, do the same for each of the topics mentioned in this page.  

My coffee shop analogy was an over-simplification, to spark interest on the topic of scaling web applications.  

If you really want to learn, tinker with these systems and discuss them with someone who has practical knowledge.

    