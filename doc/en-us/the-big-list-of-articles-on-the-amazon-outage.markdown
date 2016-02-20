## [The Big List of Articles on the Amazon Outage](/blog/2011/4/25/the-big-list-of-articles-on-the-amazon-outage.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, April 25, 2011 at 9:51AM</span></div>

<div class="body">

![](http://farm6.static.flickr.com/5190/5643644140_c73394b6ea_m.jpg)

_Please see [The Updated Big List Of Articles On The Amazon Outage](http://highscalability.com/blog/2011/5/2/the-updated-big-list-of-articles-on-the-amazon-outage.html) for a new improved list._

So many great articles have been written on the Amazon Outage. Some aim at being helpful, some chastise developers for being so stupid, some chastise Amazon for being so incompetent, some talk about the pain they and their companies have experienced, and some even predict the downfall of the cloud. Still others say we have seen a sea change in future of the cloud, a prediction that's hard to disagree with, though the shape of the change remains...cloudy.

I'll try to keep this list update as more information comes out. There will be a lot for developers to consider going forward. If there's a resource you think should be added, just let me know.

## Amazon's Explanation of What Happened

*   [Summary of the Amazon EC2 and Amazon RDS Service Disruption in the US East Region](http://aws.amazon.com/message/65648/)
*   [Hackers News thread on AWS Service Disruption Post Mortem](http://hackerne.ws/item?id=2496738) 
*   [Quite Funny Commentary on the Summary](http://i.imgur.com/ldP1f.jpg)

## Experiences from Specific Companies, Both Good and Bad

*   [Lessons Netflix Learned from the AWS Outage](http://techblog.netflix.com/2011/04/lessons-netflix-learned-from-aws-outage.html) by several Netflixians on the Netflix Tech Blog
*   [How Heroku Survived the Amazon Outage](http://status.heroku.com/incident/151) on the Heroku status page
*   [How SimpleGeo Stayed Up During the AWS Downtime](http://developers.simplegeo.com/blog/2011/04/26/how-simplegeo-stayed-up/) by Mike Malone
*   [How SmugMug survived the Amazonpocalypse](http://don.blogs.smugmug.com/2011/04/24/how-smugmug-survived-the-amazonpocalypse) by Don MacAskill  ([Hacker News](http://news.ycombinator.com/item?id=2480763) discussion) 
*   [How Bizo survived the Great AWS Outage of 2011 relatively unscathed...](http://dev.bizo.com/2011/04/how-bizo-survived-great-aws-outage-of.html) by Someone at Bizo
*   [Joe Stump's explanation](http://www.focus.com/questions/information-technology/amazon-ec2-has-gone-down--what-would-prefered-hosting-be/#comment43192) of how SimpleGeo survived 
*   [How Netflix Survived the Outage](http://www.slideshare.net/adrianco/netflix-in-the-cloud-2011)
*   [Why Twilio Wasn’t Affected by Today’s AWS Issues](http://www.twilio.com/engineering/2011/04/22/why-twilio-wasnt-affected-by-todays-aws-issues/) on Twilio Engineering's Blog ([Hacker News](http://news.ycombinator.com/item?id=2472999) thread)
*   [On reddit's outage](http://www.reddit.com/r/announcements/comments/gva4t/on_reddits_outage/#) 
*   [What caused the Quora problems/outage in April 2011?](http://www.quora.com/Quora-Outage-April-21-22-2011/What-caused-the-Quora-problems-outage-in-April-2011)
*   [Recovering from Amazon cloud outage](http://tomatohater.com/2011/04/21/recovering-amazon-cloud-outage/) by Drew Engelson of PBS. 
    *   PBS was affected for a while primarily because we do use EBS-backed RDS databases. Despite being spread across multiple availability-zones, we weren’t easily able to launch new resources ANYWHERE in the East region since everyone else was trying to do the same. I ended up pushing the RDS stuff out West for the time being.  [From Comment](http://don.blogs.smugmug.com/2011/04/24/how-smugmug-survived-the-amazonpocalypse/#comment-4737)

## Amazon Web Services Discussion Forum 

A fascinating peek into the experiences of people who were dealing with the outage while they were experiencing it. Great real-time social archeology in action.

*   [Amazon Web Services Discussion Forum](https://forums.aws.amazon.com/forum.jspa?forumID=30&start=0) 
*   [Cost-effective backup plan from now on?](https://forums.aws.amazon.com/thread.jspa?threadID=65860&tstart=0)
*   [Life of our patients is at stake - I am desperately asking you to contact](https://forums.aws.amazon.com/thread.jspa?threadID=65649&tstart=0)
*   [<span></span>Why did the EBS, RDS, Cloudformation, Cloudwatch and Beanstalk all fail?](https://forums.aws.amazon.com/thread.jspa?threadID=65897&tstart=0)
*   [Moved all resources off of AWS](https://forums.aws.amazon.com/thread.jspa?threadID=65896&tstart=0)
*   [Any success stories?](https://forums.aws.amazon.com/forum.jspa?forumID=30&start=300)
*   [Is the mass exodus from East going to cause demand problems in the West?](https://forums.aws.amazon.com/thread.jspa?threadID=65784&tstart=25)
*   [<span></span>Finally back online after about 71 hours](https://forums.aws.amazon.com/thread.jspa?threadID=65828&tstart=25)
*   [Amazon EC2 features vs windows azure](https://forums.aws.amazon.com/thread.jspa?threadID=65834&tstart=25)
*   [<span></span>Aren't Availability Zones supposed to be "insulated from failures"?](https://forums.aws.amazon.com/thread.jspa?threadID=65221&tstart=25)
*   [What a lot of people aren't realizing about the downtime:](https://forums.aws.amazon.com/thread.jspa?threadID=65850&tstart=0)
*   [ELB CNAME](https://forums.aws.amazon.com/thread.jspa?threadID=32044&tstart=50&start=150)
*   [<span></span>Availability Zones were used in a misleading manner](https://forums.aws.amazon.com/thread.jspa?threadID=65457&tstart=425)
*   [Tip: How to recover your instance](https://forums.aws.amazon.com/thread.jspa?threadID=65371&tstart=325)
*   [Crying in Forum Gets Results, Silver-level AWS Premium Support Doesn't](https://forums.aws.amazon.com/thread.jspa?threadID=65617&tstart=325)
*   [<span></span>Well-worth reading: "design for failure" cloud deployment strategy](https://forums.aws.amazon.com/thread.jspa?threadID=65780&tstart=25)
*   [New best practice](https://forums.aws.amazon.com/thread.jspa?threadID=65749&tstart=25)
*   [Don't bother with Premium Support](https://forums.aws.amazon.com/thread.jspa?threadID=65136&tstart=475)
*   [Best practices for multi-region redundancy](https://forums.aws.amazon.com/thread.jspa?threadID=65185&tstart=450)
*   <span></span>"[Postmortum](https://forums.aws.amazon.com/thread.jspa?threadID=65450&tstart=175)"
*   [Learning from this case](https://forums.aws.amazon.com/thread.jspa?threadID=65513&tstart=125)
*   [<span></span>Amazon, still no instructions what to do?](https://forums.aws.amazon.com/thread.jspa?threadID=65388&tstart=525)
*   [Anyone else prepared for an all-nighter?](https://forums.aws.amazon.com/thread.jspa?threadID=65338&tstart=550)
*   [Is Jeff Bezos going to give a public statement?](https://forums.aws.amazon.com/thread.jspa?threadID=65811&tstart=100)
*   [<span></span>Rackspace, GoGrid, StormonDemand and Others](https://forums.aws.amazon.com/thread.jspa?threadID=65857&tstart=100)
*   [Jeff Barr, Werner Vogels and other AWS persons - where have you been???](https://forums.aws.amazon.com/thread.jspa?threadID=65815&tstart=150)
*   [After you guys fix EBS do I have do anything on my side?](https://forums.aws.amazon.com/thread.jspa?threadID=65168&tstart=175)
*   [<span></span>Need Help!!! Lives of people and billions in revenue are at risk now!!!](https://forums.aws.amazon.com/thread.jspa?threadID=65765&tstart=225)
*   [I've Got A Suspicion](https://forums.aws.amazon.com/thread.jspa?threadID=65678&tstart=275)
*   [<span></span>Farewell EC2, Farewell](https://forums.aws.amazon.com/thread.jspa?threadID=65585&tstart=325)

There were also many many instances of support and help in the log. 

## In Summary

*   [Amazon EC2 outage: summary and lessons learned](http://blog.rightscale.com/2011/04/25/amazon-ec2-outage-summary-and-lessons-learned/) by RightScale
*   [AWS outage timeline & downtimes by recovery strategy](http://www.randomhacks.net/articles/2011/04/25/aws-outage-timeline-and-recovery-strategy-downtimes) by Eric Kidd
*   [The Aftermath of Amazon’s Cloud Outage](http://www.datacenterknowledge.com/archives/2011/04/25/the-aftermath-of-amazons-cloud-outage) by Rich Miller 

## Taking Sides: It's the Customer's Fault

*   [So Your AWS-based Application is Down? Don’t Blame Amazon](http://www.thestoragearchitect.com/2011/04/22/so-your-aws-based-application-is-down-dont-blame-amazon/) by The Storage Architect
*   [The Cloud is not a Silver Bullet](http://stu.mp/2011/04/the-cloud-is-not-a-silver-bullet.html) by Joe Stump ([Hacker News](http://news.ycombinator.com/item?id=2482581) thread)
*   [The AWS Outage: The Cloud's Shining Moment](http://broadcast.oreilly.com/2011/04/the-aws-outage-the-clouds-shining-moment.html) by George Reese ([Hacker News](http://news.ycombinator.com/item?id=2477540) discussion)
*   [Failing to Plan is Planning to Fail](http://blog.acrowire.com/cloud-computing/failing-to-plan-is-planning-to-fail) by Ted Theodoropoulos
*   [Get a life and build redundancy/resiliency in your apps](http://groups.google.com/group/cloud-computing/browse_thread/thread/e8079a54e6a8c4b9/72756bf9e587869d?show_docid=72756bf9e587869d) on the Cloud Computing group

## Taking Sides: It's Amazon's Fault

*   [Stop Blaming the Customers - the Fault is on Amazon Web Services](http://www.readwriteweb.com/cloud/2011/04/almost-as-galling-as-the.php) by Klint Finley
*   [AWS is down: Why the sky is falling](http://justinsb.posterous.com/aws-down-why-the-sky-is-falling) by Justin Santa Barbara  ([Hacker News](http://news.ycombinator.com/item?id=2471899) thread)
*   [Amazon Web Services are down](http://news.ycombinator.com/item?id=2469838) - Huge Hacker News thread

## Lessons Learned and Other Insight Articles

*   [Amazon’s EBS outage](http://storagemojo.com/2011/04/29/amazons-ebs-outage/) by Robin Harris of StorageMojo
*   [People Using Amazon Cloud: Get Some Cheap Insurance At Least](http://smoothspan.wordpress.com/2011/04/23/people-using-amazon-cloud-get-some-cheap-insurance-at-least/)  by Bob Warfield
*   [Basic scalability principles to avert downtime](http://ronaldbradford.com/blog/basic-scalability-principles-to-avert-downtime-2011-04-23) by Ronald Bradford
*   [Amazon crash reveals 'cloud' computing actually based on data centers](http://www.itworld.com/cloud-computing/158517/amazon-crash-reveals-cloud-computing-actually-based-data-centers) by Kevin Fogarty
*   [Seven lessons to learn from Amazon's outage](http://www.zdnet.com/blog/saas/seven-lessons-to-learn-from-amazons-outage/1296) By Phil Wainewright
*   [The Cloud and Outages : Five Key Lessons](http://www.cloudsigma.com/en/blog/2011/04/23/21-cloud-outages-lessons-learned) by Patrick Baillie ([Cloud Computing Group](http://groups.google.com/group/cloud-computing/browse_thread/thread/6e9549afbff6386f/05919d8527c69a09?show_docid=05919d8527c69a09#) discussion)
*   [Some thoughts on outages](http://till.klampaeckel.de/blog/archives/151-Some-thoughts-on-outtages.html) by Till Klampaeckel 
*   [Amazon.com’s real problem isn’t the outage, it’s the communication](http://www.geekwire.com/2011/amazoncoms-real-problem-outage-communication) by Keith Smith 
*   [How to work around Amazon EC2 outages](http://webmonkeyuk.wordpress.com/2011/04/21/how-to-work-around-amazon-ec2-outages/) by James Cohen ([Hacker News](http://news.ycombinator.com/item?id=2471258) thread)
*   [Today’s EC2 / EBS Outage: Lessons learned](http://agilesysadmin.net/ec2-outage-lessons) on Agile Sysadmin
*   [Amazon EC2 has gone down -what would a prefered hosting platform be?](http://www.focus.com/questions/information-technology/amazon-ec2-has-gone-down--what-would-prefered-hosting-be/) on Focus
*   [Single Points of Failure](http://cloudability.com/single-points-of-failure) by Mat 
*   [Coping with Cloud Downtime with Puppet](http://www.reddit.com/r/programming/comments/gvac7/coping_with_cloud_downtime_with_puppet/)
*   [Amazon Outage Concerns Are Overblown](http://timcrawford.org/2011/04/21/amazon-outage-concerns-are-overblown/) by Tim Crawford
*   [Where There Are Clouds, It Sometimes Rains](http://claylo.com/post/4817029650/where-there-are-clouds-it-sometimes-rains) by Clay Loveless
*   [Availability, redundancy, failover and data backups at LearnBoost ](http://blog.learnboost.com/blog/availability-redundancy-and-failover-at-learnboost/) by Guillermo Rauch
*   [Cloud hosting vs colocation](http://chrischandler.name/the-real-cost-of-cloud-hosting) by Chris Chandler ([Hacker News](http://news.ycombinator.com/item?id=2482123) thread)
*   [Amazon’s EC2 & EBS outage](http://arnon.me/2011/04/amazons-ec2-ebs-outage/) by Arnon Rotem-Gal-Oz
*   [Complex Systems Have Complex Failures. That’s Cloud Computing](http://etherealmind.com/complex-systems-complex-failures-cloud-computing) by Greg Ferro
*   [Amazon Web Services, Hosting in the Cloud and Configuration Management](http://www.ichilton.co.uk/blog/web/amazon-web-service-hosting-in-the-cloud-and-configuration-management-397.html) by Ian Chilton
*   [Lessons learned from deploying a production database in EC2](http://agiletesting.blogspot.com/2011/04/lessons-learned-from-deploying.html) by by Grig Gheorghiu of Agile Testing
*   [Bezos on Amazon](http://daringfireball.net/linked/2011/04/27/bezos-amazon) as a technology and invention company by John Gruber on Daring Fireball.

## Vendor's Vent

*   [Amazon Outage Proves Value of Riak’s Vision](http://www.productionscale.com/home/2011/4/22/on-clouds-and-spofs-or-the-great-aws-outage-of-april-2011.html#axzz1KZPTwX4z) by Basho
*   [Magical Block Store: When Abstractions Fail Us](http://joyeur.com/2011/04/24/magical-block-store-when-abstractions-fail-us/)  by Mark Joyent ([Hacker News](http://news.ycombinator.com/item?id=2479613) discussion)
*   [On Cascading Failures and Amazon’s Elastic Block Store](http://joyeur.com/2011/04/22/on-cascading-failures-and-amazons-elastic-block-store/) by Jason 
*   [An unofficial EC2 outage postmortem - the sky is not falling](http://cloudharmony.com/b/2011/04/unofficial-ec2-outage-postmortem-sky-is.html) from CloudHarmony
*   [Cloudfail: Lessons Learned from AWS Outage](http://www.appdynamics.com/blog/2011/04/25/critical-applications-in-the-cloud/) by Jyoti Bansal 

<div id="followup11304260" class="journal-entry-follow-up">

<div class="follow-up-caption">**Update** on Friday, April 29, 2011 at 8:27AM by [![Registered Commenter](/universal/images/transparent.png "Registered Commenter")Todd Hoff](/member/highscalability "Registered Commenter") </div>

<div class="follow-up-body">

## [Summary of the Amazon EC2 and Amazon RDS Service Disruption in the US East Region](http://aws.amazon.com/message/65648/)

*   A network change was performed as part of our normal AWS scaling activities in a single Availability Zone in the US East Region. The configuration change was to upgrade the capacity of the primary network. During the change, one of the standard steps is to shift traffic off of one of the redundant routers in the primary EBS network to allow the upgrade to happen. The traffic shift was executed incorrectly and rather than routing the traffic to the other router on the primary network, the traffic was routed onto the lower capacity redundant EBS network. 
*   When this network connectivity issue occurred, a large number of EBS nodes in a single EBS cluster lost connection to their replicas. When the incorrect traffic shift was rolled back and network connectivity was restored, these nodes rapidly began searching the EBS cluster for available server space where they could re-mirror data. Once again, in a normally functioning cluster, this occurs in milliseconds. In this case, because the issue affected such a large number of volumes concurrently, the free capacity of the EBS cluster was quickly exhausted, leaving many of the nodes “stuck” in a loop, continuously searching the cluster for free space. This quickly led to a “re-mirroring storm,” where a large number of volumes were effectively “stuck” while the nodes searched the cluster for the storage space it needed for its new replica. At this point, about 13% of the volumes in the affected Availability Zone were in this “stuck” state.

</div>

</div>

</div>