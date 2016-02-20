## [Learn from my pain - 5 Lessons from Ello's Adventures in Rapid Scaling ](/blog/2015/1/21/learn-from-my-pain-5-lessons-from-ellos-adventures-in-rapid.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, January 21, 2015 at 8:56AM</span></div>

<div class="body">

![](https://farm9.staticflickr.com/8604/16329115931_66d528caac_o.jpg)

<span id="docs-internal-guid-0f95ddf3-0aca-70da-e58d-4a16f16c57a9"></span>

<span>Within one week</span> [<span>Ello</span>](https://ello.co) <span>went from thousands of sessions a day to a few million sessions a day.</span> [<span>Mike Pack</span>](https://twitter.com/zombidev) <span>wrote a great article sharing what they’ve learned:</span> [<span>5 Early Lessons from Rapid, High Availability Scaling with Rails</span>](http://mikepackdev.com/blog_posts/40-5-early-lessons-from-rapid-high-availability-scaling-with-rails)<span>.</span>

<span>Some of their scaling challenges: quantity of data, team size, DNS, bot prevention, responding to users, inappropriate content, and other forms of caching. What did they learn?</span>

1.  **Move the graph**. User relationships were implemented on a standard Rails stack using Heroku and Postgres. The relationships table became the bottleneck. Solution: denormalize the social graph and move hot data into Redis. Redis is used for speed and Postgres is used for durability. Lesson: know the core pillar that supports your core offering and make it work.

2.  **Create indexes early, or you're screwed**. There's a camp that says only create indexes when they are needed. They are wrong. The lack of btree indexes kills query performance. Forget a unique index and your data becomes corrupted. Once the damage is done it's hard to add unique indexes later. The data has to be cleaned up and indexes take a long time to build when there's a lot of data.

3.  **Sharding is cool, but not that cool**. <span>Shard all the things only after you've tried vertically scaling as much as possible. Sharding caused a lot of pain. Creating a covering index from the start and adding more RAM so data could be </span>served from memory, not from disk, would have saved a lot of time and stress as the system scaled.

4.  **Don't create bottlenecks, or do**. Every new user automatically followed a system user that was used for announcements, etc. Scaling problems that would have been months down the road hit quickly as any write to the system user caused a write amplification of millions of records. The lesson here is not what you may think. While scaling to meet the challenge of the system user was a pain, it made them stay ahead of the scaling challenge. Lesson: self-inflict problems early and often.

5.  **It always takes 10 times longer**. All the solutions mentioned take much longer to implement than you might think. Early estimates of a couple days soon give way to the reality of much longer time hits. Simply moving large amounts of data can take days. Adding indexes to large amounts of data takes time. And with large amounts of data problems tend to happen as you get to the larger data sizes which means you need to apply a fix and start over. 

This full [article](http://mikepackdev.com/blog_posts/40-5-early-lessons-from-rapid-high-availability-scaling-with-rails) is excellent and is filled with much more detail that makes it well worth reading.

</div>