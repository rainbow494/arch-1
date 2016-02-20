## [Seven of the Nastiest Anti-patterns in Microservices](/blog/2015/8/3/seven-of-the-nastiest-anti-patterns-in-microservices.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, August 3, 2015 at 8:56AM</span></div>

<div class="body">

![](https://farm1.staticflickr.com/422/19635212354_b1caec8235_m.jpg)

[Daniel Bryant](https://twitter.com/danielbryantuk) gave an energetic talk at [Devoxx UK 2015](http://www.devoxx.co.uk/) on lessons learned from over five years of experience with microservice based projects. The talk: [The Seven Deadly Sins of Microservices: Redux](http://container-solutions.com/the-seven-deadly-sins-of-microservices-redux/) ([video](https://www.parleys.com/tutorial/seven-deadly-sins-microservices), [slides](http://www.slideshare.net/dbryant_uk/devoxxuk-2015-the-seven-deadly-sins-of-microservices-full-version)).

If you don't want to risk your immortal API then be sure to avoid:

1.  **Lust** - using the latest and greatest tech with the idea it will solve all your problems. It won't. Do you really need microservices at all? If you do go microservices do you really need new tech in your stack? Choose boring technology. Know why you are choosing something. A monolith can perform better and because a monolith can be developed faster it may also be the correct choice in proving your business case 
2.  **Gluttony** - excessive communication protocols. Projects often have a crazy number of protocols for gluing parts together. Standardize on the glue across an organization. Choose one synchronous and one asynchronous protocol. Don't gold-plate.
3.  **Greed** - all your service are belong to us. Do not underestimate the impact moving to a microservice approach will have on your organization. Your business organization needs to change to take advantage of microservices. Typically orgs will have silos between Dev, QA, and Ops with even more silos inside each silo like front-end, middleware, and database. Use cross functional teams like Spotify, Amazon, and Gilt. Connect rather than divide your company. 
4.  **Sloth** - creating a distributed monolith. If you can't deploy your services independently then they aren't microservices. Decouple. Transform data at a less central part of the stack. Some options are schema-first design and consumer-driven contracts.
5.  **Wrath** - blowing up when bad things happen. Bad things happen all the time so you need to test. Microservices are inherently distributed so you have network problems to deal with that weren't a problem in a monolith. The book Release It! has a lot of good fault tolerance patterns. Operationally you need to implement continuous delivery, agile, and devops. Test for failures using real life disaster scenarios testing, live injection failure testing, and something like Netflix's Simian Army.
6.  **Envy** - the shared single domain fallacy. A lot of time has been spent building and perfecting the model of a single domain. There's one big database with a unified schema. Microservices decompose a system along different lines and that can cause contention in an organization. Reports can be generated using pull by service or data pumps with events. 
7.  **Pride** - testing in the world of transience. Does your stuff really work? We all make mistakes. Think testing at the developer level, operational level, and business level. Surprisingly little has been written about testing microservices. Invest in your build pipeline testing. Some tools: Serenity BOD, Wiremock/Saboteur, Jenkins Performance Plugin. Testing in production is an emerging idea with companies that deploy many microservices.

##  Related Articles

*   [Microservices - Not A Free Lunch!](http://highscalability.com/blog/2014/4/8/microservices-not-a-free-lunch.html)
*   [The Great Microservices Vs Monolithic Apps Twitter Melee](http://highscalability.com/blog/2014/7/28/the-great-microservices-vs-monolithic-apps-twitter-melee.html)
*   [Developing Products In The Style Of Etsy](http://highscalability.com/blog/2015/6/1/developing-products-in-the-style-of-etsy.html) 
*   [The 10 Deadly Sins Against Scalability](http://highscalability.com/blog/2013/6/10/the-10-deadly-sins-against-scalability.html)

</div>