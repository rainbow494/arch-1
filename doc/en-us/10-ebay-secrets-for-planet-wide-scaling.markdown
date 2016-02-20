## [10 eBay Secrets for Planet Wide Scaling](/blog/2009/11/17/10-ebay-secrets-for-planet-wide-scaling.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, November 17, 2009 at 11:27AM</span></div>

<div class="body">

You don't even have to make a bid, Randy Shoup, an eBay Distinguished Architect, gives this [presentation](http://www.hpts.ws/session9/shoup.pdf) on how eBay scales, for free. Randy has done a fabulous job in this presentation and in other talks listed at the end of this post getting at the heart of the principles behind scalability. It's more about ideas of how things work and fit together than a focusing on a particular technology stack.

### Impressive Stats

In case you weren't sure, eBay is big, with lots of: users, data, features, and change...

*   Over 89 million active users worldwide
*   190 million items for sale in 50,000 categories
*   Over 8 billion URL requests per day
*   Hundreds of new features per quarter
*   Roughly 10% of items are listed or ended every day
*   In 39 countries and 10 languages
*   24x7x365
*   70 billion read / write operations / day
*   Processes 50TB of new, incremental data per day
*   Analyzes 50PB of data per day

### 10 Lessons

The presentation does a good job explaining each lesson, but the list is...

1.  **Partition Everything** - if you can't split it, you can't scale it. Split everything into manageable chunks by function and data.
2.  **Asynchrony Everywhere** - connect independent components through event queues
3.  **Automate Everything** - components should automatically adjust and the system should learn and improve itself.
4.  **Remember Everything Fails** - monitor everything, provide service even when parts start failing.
5.  **Embrace Inconsistency** - pick for each feature where you need to be on the CAP continuum, no distributed transactions, inconsistency can be minimized by careful operation ordering, become eventually consistent through async recovery and reconciliation.
6.  **Expect (R)evolution** - change is constant, design for extensibility, incrementally deploy changes.
7.  **Dependencies Matter** - minimize and control dependencies, use abstract interfaces and virtualization, components have an SLA, consumers responsible for recovering from SLA violations.
8.  **Be Authoritative** - Know which data is authoritative, which data isn't, and treat it accordingly.
9.  **Never Enough Data -** data drives finding optimization opportunities, predictions, recommendations, so save it all.
10.  **Custom Infrastructure** - maximize the utilization of every resource.

### Related Articles

*   [eBay Related Posts on HighScalability](http://highscalability.com/blog/category/ebay)
*   [Scalability Best Practices: Lessons from eBay](http://www.infoq.com/articles/ebay-scalability-best-practices) by Randy Shoup
*   [Episode 109: eBay's Architecture Principles](http://odeo.com/episodes/23259163-Episode-109-eBay-s-Architecture-Principles-with-Randy-Shoup) with Randy Shoup, [transcript](http://www.se-radio.net/transcript-109-ebay039s-architecture-principles-randy-shoup)

</div>