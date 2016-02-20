## [Rumors of Signs and Portents Concerning Freeish Google Cloud](/blog/2008/4/7/rumors-of-signs-and-portents-concerning-freeish-google-cloud.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, April 7, 2008 at 3:30PM</span></div>

<div class="body">_Update 2: Rumor no more. [Google Jumps Head First Into Web Services With Google App Engine](http://www.techcrunch.com/2008/04/07/google-jumps-head-first-into-web-services-with-google-app-engine/). The quick and dirty of it: developers simply upload their Python code to Google, launch the application, and can monitor usage and other metrics via a multi-platform desktop application. There were 10,000 developer slots open and of course I was too late. More as the cobra strikes._  
_Update: TechCrunch reports [Google To Launch BigTable As Web Service](http://www.techcrunch.com/2008/04/04/source-google-to-launch-bigtable-as-web-service/) next week. It competes with Amazon's SimpleDB. Though it won't be truly comparable until they also release an EC2 and S3 equivalent. An internet hit for each data access is a little painful. As Jimmy says in Goodfellas, "That's the way. You don't take no sh*t from nobody. "_  

First [Dave Winer](http://www.scripting.com/stories/2008/03/29/pigs.html) hallucinates a pig on the mean streets of Walnut Creek that told him Google's long foretold cloud offering will be free for bloggers of "modest needs." GigaOM then says a free cloud service is how Google could eat [Amazon's bacon for lunch](http://gigaom.com/2008/03/31/how-google-can-eat-amazons-lunch/).  

The reason for this free cloud buffet is said to be the easier integration of acquisitions who must presumably be in the Google cloud to be taken out. All the free stuff Google offers earns almost no money. They make money on search. Hosting every last CPU cycle on earth has to be costly. What's the return? Cheaper integration of new startups that will also provide no new revenue?  

Perhaps I am simply not clever enough to see the revolutionary brilliance in this line of thought. Though I would be quite pleased to have Google shareholders subsidize my projects.  

[Folknologist](http://www.folknology.com/blogs/default/2008/04/01/1207046700000.html) thinks Google may keep costs down by requiring developers to code to a Cloud Virtual Machine based on Java byte codes...  

Applications would be built using G-ROR, a javascript style RoR framework. Revenue generation would come from an upsell of more memory and CPU. But aren't VMs already the perfect encapsulation from the cloud provider perspective? They just load 'em and run 'em. Seems cost effective enough. For the developer VMs also allow all required flexibility. You don't need to be locked into one environment. You can pick from a large number of operating systems and even wider variety of frameworks. Why lock in?  

If the model is to treat the cloud like one giant Tomcat application server so you can squeeze more users on the same amount of hardware then Google would just be the worlds largest shared hosting company. Not a cloud at all. And multi-tenant execution of applications in the same application server was always a really bad idea given how one badly programmed app can bring down the whole bunch. Not to mention security concerns. VMs offer better control, manageability, and security.  

I could see an [Adoption Led](http://blogs.sun.com/webmink/entry/the_adoption_led_market) market angle for Google. You could start small in a shared container and then as you grow move your app without change to a larger, more powerful, unshared container.  

We certainly do need a better way to create, deploy, and manage applications across VMs and data centers, but I don't quite see how this allows Google to make money offering an expensive service any better than the current VM approach. Though with all their cash maybe they plan to just wait it out until all the others bash themselves apart on the rocky shores of free.  

Just in case this is an April fools joke, I already know I am an idiot, so no harm done.</div>