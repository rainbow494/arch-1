## [Paper: Large-scale cluster management at Google with Borg](/blog/2015/4/16/paper-large-scale-cluster-management-at-google-with-borg.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, April 16, 2015 at 8:56AM</span></div>

<div class="body">

![](https://farm8.staticflickr.com/7635/16979575928_db7726a8a5_m.jpg)

_[Joe Beda (@jbeda)](https://twitter.com/jbeda/status/588665857506611200?refsrc=email&s=11): Borg paper is finally out. Lots of reasoning for why we made various decisions in #kubernetes. Very exciting._

The hints and allusions are over. We now have everything about Google's long rumored Borg project in one iconic Google style paper: [Large-scale cluster management at Google with Borg](http://research.google.com/pubs/pub43438.html).

When Google blew our minds by audaciously treating the [Datacenter as a Computer](http://highscalability.com/blog/2013/8/22/the-datacenter-as-a-computer-an-introduction-to-the-design-o.html) it did not go unnoticed that by analogy there must be an operating system for that datacenter/computer.

Now we have the story behind a critical part of that OS:

> Google's Borg system is a cluster manager that runs hundreds of thousands of jobs, from many thousands of different applications, across a number of clusters each with up to tens of thousands of machines.
> 
> It achieves high utilization by combining admission control, efficient task-packing, over-commitment, and machine sharing with process-level performance isolation. It supports high-availability applications with runtime features that minimize fault-recovery time, and scheduling policies that reduce the probability of correlated failures. Borg simplifies life for its users by offering a declarative job specification language, name service integration, real-time job monitoring, and tools to analyze and simulate system behavior.
> 
> We present a summary of the Borg system architecture and features, important design decisions, a quantitative analysis of some of its policy decisions, and a qualitative examination of lessons learned from a decade of operational experience with it.
> 
> Virtually all of Google’s cluster workloads have switched to use Borg over the past decade. We continue to evolve it, and have applied the lessons we learned from it to Kubernetes

The next version of Borg was called Omega and Omega is being rolled up into [Kubernetes](https://github.com/GoogleCloudPlatform/kubernetes) (steersman, helmsman, sailing master), which has been open sourced as part of [Google's Cloud](https://cloud.google.com/) initiative.

**Note how the world has changed**. A decade ago when Google published their industry changing [Big Table](http://static.googleusercontent.com/media/research.google.com/en/us/archive/bigtable-osdi06.pdf) and [Map Reduce](http://research.google.com/archive/) papers they launched a thousand open source projects in response. Now we are not only seeing Google open source their software instead of others simply copying the ideas, the software has been released well in advance of the paper describing the software.

**The future is still in balance**. There's a huge fight going on for the future of what software will look like, how it is built, how it is distributed, and who makes the money. In the search business keeping software closed was a competitive advantage. In the age of AWS the only way to capture hearts and minds is by opening up your software. Interesting times.

## Related Articles

*   [Return of the Borg: How Twitter Rebuilt Google’s Secret Weapon](http://www.wired.com/2013/03/google-borg-twitter-mesos/?cid=co6188564)
*   [What's the difference between Apache's Mesos and Google's Kubernetes](http://stackoverflow.com/questions/26705201/whats-the-difference-between-apaches-mesos-and-googles-kubernetes)
*   [2011 GAFS Omega](https://www.youtube.com/watch?v=0ZFMlO98Jkc) John Wilkes

</div>