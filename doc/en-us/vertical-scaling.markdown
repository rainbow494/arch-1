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

Business is booming. Time to upgrade again. Bigger is better!...

[Click to read more ...](/blog/2014/3/17/intuitively-showing-how-to-scale-a-web-application-using-a-c.html)

    