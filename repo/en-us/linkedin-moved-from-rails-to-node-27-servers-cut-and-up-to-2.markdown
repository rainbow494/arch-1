## [LinkedIn Moved from Rails to Node: 27 Servers Cut and Up to 20x Faster](/blog/2012/10/4/linkedin-moved-from-rails-to-node-27-servers-cut-and-up-to-2.html)

    

    

![](http://farm9.staticflickr.com/8317/8047052471_8388200c67_m.jpg)

**Update**: More background by Ikai Lan, who worked on the mobile server team at LinkedIn, says [some facts were left out](http://ikaisays.com/2012/10/04/clearing-up-some-things-about-linkedin-mobiles-move-from-rails-to-node-js/): the app made "a cross data center request, guys. Running on single-threaded Rails servers (every request blocked the entire process), running Mongrel, leaking memory like a sieve." Which explains why any non-blocking approach would be a win. And Ikai, I hope as you do that nobody reads HS and just does what somebody else does without thinking. The goal here is information that you can use to make your own decisions.

Ryan Paul has written an excellent [behind-the-scenes look at LinkedIn’s mobile engineering](http://arstechnica.com/information-technology/2012/10/a-behind-the-scenes-look-at-linkedins-mobile-engineering/2/). While the mobile part of the story--23% mobile usage; focus on simplicity, ease of use, and reliability; using a room metaphor; 30% native, 80% HTML; embedded lightweight HTTP server; single client-app connection--could help guide your mobile strategy, the backend effects of moving from Rails to Node.js may also prove interesting. 

After evaluation, some of the advantages of Node.js were:

*   Much better performance and lower memory overhead than other tested options, running up to 20x faster in some scenarios
*   Programmers could leverage their JavaScript skills. 
*   Frontend and backend mobile teams could be combined into a single unit. 
*   Servers were cut to 3 from 30\. Enough headroom remains to handle 10x current levels of resource utilization.
*   Development could focus more on application development than firefighting

Clearly a lot of issues are being mixed together here. We have a rewrite, a change of stack, and a change of logic distribution between the server and the client, so there's plenty of room to argue where the gains really came from, but it's clear LinkedIn believes the use of Node.js was a big win for them. YMMV.

The comment section is, well, vigorous, but has some interesting observations. I especially liked one comment by oluseyi:

> For our inevitable rearchitecting and rewrite, we want to cache content aggressively, store templates client-side (with the ability to invalidate and update them from the server) and keep all state purely client side. This means the application may request from the server all content matching a set of filters updated since a provided timestamp in order to refresh its cache; rather than opening and closing several connections, we want to open a single long-lived connection and stream all of the relevant metadata, assets and content. (Again, remember that the original implementation just rendered the returned HTML, which means that URIs for images, etc pointed to the server, and because the web views were being created and destroyed with navigation, the images were not being effectively cached.)
> 
>   
> In the long-lived connection implementation, there are no longer "views" in the traditional MVC web application sense. The final result of server-side processing in the controller to aggregate any necessary data is not markup written to the output stream but rather a large binary blob which the client unpacks to extract all relevant data.
> 
> So while I see your concern that the term MVC is being misused here, its usage is correct in a web application context-specific sense. The "view" (markup written to output, including references to external URIs that must be resolved by the rendering web view) is now an aggregated data stream, which gets unpacked, cached and then rendered client-side into the view.

## Related Articles 

*   [Clearing up some things about LinkedIn mobile’s move from Rails to node.js](http://ikaisays.com/2012/10/04/clearing-up-some-things-about-linkedin-mobiles-move-from-rails-to-node-js/) ([On Hacker News](http://news.ycombinator.com/item?id=4615429))
*   [Is Node.Js Becoming A Part Of The Stack? SimpleGeo Says Yes.](http://highscalability.com/blog/2011/2/22/is-nodejs-becoming-a-part-of-the-stack-simplegeo-says-yes.html) 
*   [3 Secrets To Lightning Fast Mobile Design At Instagram](http://highscalability.com/blog/2012/6/7/3-secrets-to-lightning-fast-mobile-design-at-instagram.html)
*   [10 Golden Principles For Building Successful Mobile/Web Applications](http://highscalability.com/blog/2012/7/5/10-golden-principles-for-building-successful-mobileweb-appli.html)
*   [On Hacker News](http://news.ycombinator.com/item?id=4613870)

    