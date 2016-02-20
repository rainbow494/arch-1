## [Netflix: Harden Systems Using a Barrel of Problem Causing Monkeys - Latency, Conformity, Doctor, Janitor, Security, Internationalization, Chaos](/blog/2011/7/20/netflix-harden-systems-using-a-barrel-of-problem-causing-mon.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, July 20, 2011 at 9:21AM</span></div>

<div class="body">

![](http://farm7.static.flickr.com/6144/5956126692_1a76dd54ef_m.jpg)

With a new Planet of the Apes coming out, this may be a touchy subject with our new overlords, but Netflix is using a whole lot more trouble injecting monkeys to test and iteratively harden their systems. We learned previously how Netflix used [Chaos Monkey](http://highscalability.com/blog/2010/12/28/netflix-continually-test-by-failing-servers-with-chaos-monke.html), a tool to test failover handling by continuously failing EC2 nodes. That was just a start. More monkeys have been added to the barrel. Node failure is just one problem in a system. Imagine a problem and you can imagine creating a monkey to test if your system is handling that problem properly. Yury Izrailevsky talks about just this approach in this very interesting post: [The Netflix Simian Army](http://techblog.netflix.com/2011/07/netflix-simian-army.html).

I know what you are thinking, if monkeys are so great then why has Netflix been down lately. [Dmuino addressed](http://news.ycombinator.com/item?id=2783354) this potential embarrassment, putting all fears of cloud inferiority to rest:

> Unfortunately we're not running 100% on the cloud today. We're working on it, and we could use more help. The latest outage was caused by a component that still runs in our legacy infrastructure where we have no monkeys :)

To continuously test the resilience of Netflix's system to failures, they've added a number of new monkeys, and even a gorilla:

<div id="_mcePaste">

*   **Latency Monkey** induces artificial delays in our RESTful client-server communication layer to simulate service degradation and measures if upstream services respond appropriately. 
*   **Conformity Monkey** finds instances that don’t comply with best-practices and shuts them down. 
*   **Doctor Monkey** taps into health checks that run on each instance as well as monitors other external signs of health (i.e. CPU load) to detect unhealthy instances. 
*   **Security Monkey **searches for security violations or vulnerabilities, such as improperly configured AWS security groups, and terminates the offending instances. It also makes sure all our SSL and DRM certificates are valid and are not coming up for renewal.
*   **10-18 Monkey (short for Localization-Internationalization, or l10n-i18n)** detects configuration problems in instances serving customers in multiple geographic regions, using different languages and character sets. 
*   **Chaos Gorilla** is similar to Chaos Monkey, but simulates an outage of an entire Amazon availability zone. 

</div>

Just as important as the test tools is that they've gone to the trouble to identify failure scenarios, implement code to deal with them, and then implement code that can verify that the problems have been dealt with. That's just as important a process as the tools. Rarely do things just fail. They become intermittent, they become slow, they don't failover cleanly, the don't recover cleanly, they lie about what is really happening, they don't collect the data you need to figure out what is going, they corrupt messages, they drop work randomly, they do crazy things that are failures but don't fit in clean crisp failure definition boundaries. Figuring out what can be done in response to a complex set of interacting problems is the real magic here.

## Layering is Key

Without an architecture built around APIs, that is one with aggressive layering to isolate components, it would be impossible to test a system in this way. By creating insertion points for normal services and for testing, it becomes possible to expose the entire system to scripting and manipulation. Big ball of mud systems can't do this, neither can monolithic 2-tier type systems.

## When do monkeys run? Protecting Against Cascading Failure

The obvious downside of letting monkeys into your system is that you can easily imagine the whole thing melting down through an innocent coding error. There's a little more supervision than that. [Dmuino](http://news.ycombinator.com/item?id=2783701) explains:

> Most monkeys only run when we have developers who could notice and fix problems. It also happens that our peak usage is while we're home and our quiet time is while we're at the office. That means, in general, the monkeys don't run on a Sunday night.

More details in the [original post](http://techblog.netflix.com/2011/07/netflix-simian-army.html).

</div>