## [The Secret of Scaling: You Can't Linearly Scale Effort with Capacity](/blog/2014/6/4/the-secret-of-scaling-you-cant-linearly-scale-effort-with-ca.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, June 25, 2014 at 8:56AM</span></div>

<div class="body">

![](https://farm6.staticflickr.com/5572/14193665886_94f850f885_m.jpg)

The title is a paraphrase of something Raymond Blum, who leads a team of Site Reliability Engineers at Google, said in his talk [How Google Backs Up the Internet](http://highscalability.com/blog/2014/2/3/how-google-backs-up-the-internet-along-with-exabytes-of-othe.html). I thought it a powerful enough idea that it should be pulled out on its own:

> Mr. Blum explained common backup strategies don’t work for Google for a very googly sounding reason: typically **they scale effort with capacity**.
> 
> If backing up twice as much data requires twice as much stuff to do it, where stuff is time, energy, space, etc., it won’t work, it doesn’t scale. 
> 
> You have to **find efficiencies so that capacity can scale faster than the effort needed to support that capacity**.
> 
> A different plan is needed when making the jump from backing up one exabyte to backing up two exabytes.

When you hear the idea of not scaling effort with capacity it sounds so obvious that it doesn't warrant much further thought. But it's actually a profound notion. Worthy of better treatment than I'm giving it here.

It's the intuition behind Big O notation, that as problems get larger they become different in kind and need different algorithms to solve them. It's also at the heart the DevOps dictum of automate-all-things. It's why a business built on working for an hourly fee isn't scalable. It's why when humans grouped themselves into organizations larger than [Dunbar's Number](http://en.wikipedia.org/wiki/Dunbar%27s_number) that we had to invent hierarchy and bureaucracy. And it's why when those organizations of people get too large, like Rome, they collapse.

Scaling effort with capacity is usually how things begin. It's a bargain we make for simplicity. If a hole needs digging to plant a tree a shovel will do fine. Building a dam requires monster equipment and complex systems.

Scaling effort with capacity is also a characteristic of a pre-industrial world. The Great Wall of China and the Pyramids are just a few examples of what can be done with inexpensive labor applied at scale. Crowdsourcing is in the spirit of this age old system, just hopefully a little more humane.

Monitoring effort against capacity is a good way of testing when something has entered a different phase. When you are in a flow it's hard to know when things have changed. If you or a machine or a process are going crazy, be a little mindful, ask yourself if you are scaling effort with capacity? If you are, what can you do differently? Look for force multipliers. Obviously automation is the major way of improving utilization and efficiency, but maybe there are other things you can do?

</div>