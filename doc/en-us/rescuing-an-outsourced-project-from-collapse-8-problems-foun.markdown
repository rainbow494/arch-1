## [Rescuing an Outsourced Project from Collapse: 8 Problems Found and 8 Lessons Learned](/blog/2015/2/11/rescuing-an-outsourced-project-from-collapse-8-problems-foun.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, February 11, 2015 at 8:56AM</span></div>

<div class="body">

[![](https://farm9.staticflickr.com/8652/16314638648_e124f4a5f9_m.jpg)](http://medicmadness.com/2010/02/teach-them-while-their-young/)

If you are one of those people that think most of the products featured on HighScalability use way too many servers then you'll love this story: 130 VMs serving less than 10,000 users daily were chopped down to just one machine.

Here's the setup. A smallish website was having problems. Users were unhappy. In the balance was not only the product, but the company. The site was built using Angular, Symfony2, Postgres, Redis, Centos, 8 HP blades with 128 G RAM each, two racks, a very large HP 3par storage array, a 1Gbps uplink, and VMWare.

More than enough power for the task at hand. Yet the system couldn't handle the load. What would you do?

That's the story Jacques Mattheij tells in his very entertaining and educational [Saving a Project and a Company](http://jacquesmattheij.com/saving-a-project-and-a-company) article.

Jacques says much was right about the website, but time pressure and mismanagement created big problems at the system level. "A single clueless person in a position of trust with non technical management, an outsourced project and a huge budget, what could possibly go wrong?" Sound familiar? 

## Problem 1: Virtualization Gone Crazy

This was fascinating. The system had 130 VMs, each restricted to 3G RAM and 2 or 4 cores. Having so many small VMs burned up the CPU and memory budget without getting much performance. Assigning a VM to every process denies the guest OS the ability to prioritize and schedule. The VM architecture is required to "divide resources fairly in a mostly static fashion and that setup doesn’t have the in-depth knowledge the guest OS does about the multiple processes it is scheduling." In addition, each VM will have to be secured, tuned, and maintained. 

Don't over use virtualization. Use it for high availability, isolation, and snapshotting. Only a handful of VMs are used now, which greatly reduced system complexity and increased security. 

Simplicity is key: a lot of the problems they were experiencing were made tractable and eventually solvable.

## Problem 2: Zero Application Level Documentation

There was no documentation at all saying how the whole system was supposed to work. Which is possibly why it didn't work.

## Problem 3: Premature Scalification

Jacques makes the point we often make on this site: scale up first, then scale out. If a machine with 64 cores and 256G of RAM no longer works for you, then scale out. Start small and evolve.

With less than 10K daily visitors the entire big iron scale out approach was incredible overkill. Such a large system is much harder to deploy and debug.

Worst of all it eats up your most precious resource: time. Time to make the product work better for its users.

## Problem 4: No Caching

There was no caching at all. No op code cache for PHP, no HTML caching, no front-end caching, no caching of database queries or results, sessions were stored on disk. Needless to say caching is the number one strategy to handle more traffic with fewer resources.

## Problem 5: Poor Database Tuning

The database only had indexes on primary keys. Postgres logged many queries as slow, but nothing was done about them. 

Postgres also has an 'auto vacuum' setting which caused hourly load spikes as it contemplated itsself. Running it once nightly solved the problem.

## Problem 6: Too Much Logging

Lots of log data was being written on every request. Disabling these debug logs improved performance.

## Problem 7: Poor Security

Disabling the debug logs also closed a security loophole as logs were publicly available on the website and were world writeable.

## Problem 8: Poor Hardware

VMware did it's job of providing high availability, but the expensive hardware had a surprising number of failures that slowed everything down.

## Lesson 1: Stop the Bleeding

The first goal was not to make the system perfect, but to make it function. Don't set a broken arm when the patient isn't breathing.

The load on the system is now .6, which leaves plenty of room and time for further optimizations. Micro optimization should not be your first concern in this type of emergency. First find the major bottlenecks and fix them.

## Lesson 2: Instrument Everything

You may have heard this one before. The way you find bottlenecks is through instrumentation. Many fine grain counters were installed to figure out was was really going on this system. Any problem in the relationship between the counters indicated where there was a lack of understanding about what was happening in the system and pointed to a problem that needed fixing.

## Lesson 3: Trust but verify

Keep track of what's happening on an outsourced project. Demand to be shown tangible results at frequent intervals. If you can't verify the results hire someone who can. And this is a good one: do not put the executive and control functions in the same hands.

## Lesson 4: Don’t be sold a bill of goods

Know what you really need. Do you really need a huge expensive system to meet your requirements?

## Lesson 5: Know your business

Identify key metrics, monitor them in realtime, and act immediately when problems are found.

## Lesson 6: Be prepared to roll back

If a new system causes problems have a plan to roll back to your old system just in case.

## Lesson 7: Work incremental, release frequently

Avoid big bang releases. Sequence releases and monitor system health during each release for signs of problems. This well help prevent disasters and troubleshoot problems.

## Lesson 8: Emergency Fixes are a Death March

Jacques tells how he and his team had to work long exhausting hours coaxing the system back to health. Paying attention to these lessons would have prevented most of the problems in the first place. 

Jacques Mattheij has written a really fine article with lots more color that's not in this gloss, so please read [Saving a Project and a Company](http://jacquesmattheij.com/saving-a-project-and-a-company) for many more details.

## Related Articles

*   [On Reddit](http://www.reddit.com/r/programming/comments/2vk8z1/rescuing_an_outsourced_project_from_collapse_8/) / [On Hacker News](https://news.ycombinator.com/item?id=9039184)

</div>