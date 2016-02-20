## [10 Common Server Setups For Your Web Application](/blog/2014/9/10/10-common-server-setups-for-your-web-application.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, September 10, 2014 at 8:56AM</span></div>

<div class="body">

<span class="full-image-float-right ssNonEditable"><span>![](https://farm4.staticflickr.com/3854/15183294732_d929eb9489_m.jpg?__SQUARESPACE_CACHEVERSION=1410218596923)</span></span>

If you need a good overview of different ways to setup your web service then Mitchell Anicas has written a good article for you: [5 Common Server Setups For Your Web Application](https://www.digitalocean.com/community/tutorials/5-common-server-setups-for-your-web-application).

We've even included a few additional possibilities at no extra cost.

1.  **Everything on One Server**. Simple. Potential for poor performance because of resource contention. Not horizontally scalable. 
2.  **Separate Database Server**. There's an application server and a database server. Application and database don't share resources. Can independently vertically scale each component. Increases latency because the database is a network hop away.
3.  **Load Balancer (Reverse Proxy)**. Distribute workload across multiple servers. Native horizontal scaling. Protection against DDOS attacks using rules. Adds complexity. Can be a performance bottleneck. Complicates issues like SSL termination and stick sessions.
4.  **HTTP Accelerator (Caching Reverse Proxy)**. Caches web responses in memory so they can be served faster. Reduces CPU load on web server. Compression reduces bandwidth requirements. Requires tuning. A low cache-hit rate could reduce performance. 
5.  **Master-Slave Database Replication**. Can improve read and write performance. Adds a lot of complexity and failure modes.
6.  **Load Balancer + Cache + Replication**. Combines load balancing the caching servers and the application servers, along with database replication. Nice explanation in the article.
7.  **Database-as-a-Service ([DBaaS](http://www.forbes.com/sites/oracle/2013/12/05/why-database-as-a-service-dbaas-will-be-the-breakaway-technology-of-2014/))**. Let someone else run the database for you.  RDS is one example from Amazon and there are hosted versions of many popular databases.
8.  **Backend as a Service ([BaaS](http://en.wikipedia.org/wiki/Backend_as_a_service))**. If you are writing a mobile application and you don't want to deal with the backend component then let someone else do it for you. Just concentrate on the mobile platform. That's hard enough. Parse and Firebase are popular examples, but there are many more.
9.  **Platform as a Service ([PaaS](http://en.wikipedia.org/wiki/Platform_as_a_service))**. Let someone else run most of your backend, but you get more flexibility than you have with BaaS to build your own application. Google App Engine, Heroku, and Salesforce are popular examples, but there are many more.
10.  **Let Somone Else Do it**. Do you really need servers at all? If you have a store then a service like Etsy saves a lot of work for very little cost. Does someone already do what you need done? Can you leverage it?

</div>