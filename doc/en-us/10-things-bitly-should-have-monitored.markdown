## [10 Things Bitly Should Have Monitored](/blog/2014/1/29/10-things-bitly-should-have-monitored.html)

    

    

![](http://farm8.staticflickr.com/7429/12207770764_298a873fb2_o.png)

Monitor, monitor, monitor. That's the advice every startup gives once they reach a certain size. But can you ever monitor enough? If you are Bitly and everyone will complain when you are down, probably not.

Here are [10 Things We Forgot to Monitor](http://word.bitly.com/post/74839060954/ten-things-to-monitor) from Bitly, along with good stories and copious amounts of code snippets. Well worth reading, especially after you've already started monitoring the lower hanging fruit.

An interesting revelation from the article is that:

> We run bitly split across two data centers, one is a managed environment with DELL hardware, and the second is Amazon EC2.  

1.  **Fork Rate**. A strange configuration issue caused processes to be created at a rate of several hundred a second rather than the expected 1-10/second. 
2.  **Flow control packets**.  A network configuration that honors flow control packets and isn’t configured to disable them, can temporarily cause dropped traffic.
3.  **Swap In/Out Rate**. Measure the right thing. It's the rate memory is swapped in/out that can impact performance, not the quantity. 
4.  **Server Boot Notification**. Use an init script to capture when servers are dying. Servers do die, but are they dying too often? 
5.  **NTP Clock Offset**. If you are not checking one of you servers is probably not properly time synced. 
6.  **DNS Resolutions**. This is a key part of your infrastructure that often goes unchecked. It can be the source of a lot of latency and availability problems. On the Internal DNS check quantity, latency, and availability. Also verify External DNS servers give the correct answers and are available. 
7.  **SSL Expiration**. Don't let those certificates expire. Set up an expiration check.
8.  **DELL OpenManage Server Administrator (OMSA)**.  Monitor the outputs from OMSA to know when failures have occurred. 
9.  **Connection Limits**. Do you know how close you are to your connection limits?
10.  **Load Balancer Status**. It's important to have visibility into your load balancer status by making the health stats visible.  

    