## [Heroku Emergency Strategy: Incident Command System and 8 Hour Ops Rotations for Fresh Minds](/blog/2011/4/27/heroku-emergency-strategy-incident-command-system-and-8-hour.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, April 27, 2011 at 8:35AM</span></div>

<div class="body">

![](http://farm6.static.flickr.com/5021/5659457251_24b7c16394_m.jpg)

In [Resolved: Widespread Application Outage](http://status.heroku.com/incident/151)<span>, <span>Heroku</span> tells their story of how they dealt with the</span> [Amazon outage](http://highscalability.com/blog/2011/4/25/the-big-list-of-articles-on-the-amazon-outage.html). While taking 100% responsibility for the downtime, they also shared a number of the strategies they used to bring their service back to full working order.

<span>One of <span>Heroku's</span> most interesting strategies wasn't a technical hack at all, but how they consciously went about deploying their Ops personnel in response to the emergency. An outline of their strategy is:</span>

*   Monitoring systems immediately alerted Ops to the problem. 
*   An on-call engineer applied triage logic to the problem and classified it as serious, which caused the on-call [Incident Commander](http://en.wikipedia.org/wiki/Incident_command_system) to be woken out of restful slumber. 
*   <span>The <span>IC</span> contacted <span>AWS</span>. They were in constant contact with their <span>AWS</span> representative and worked closely with <span>AWS</span> to solve problems.</span>
*   <span>The <span>IC</span> alerted <span>Heroku</span> engineers. A full crew: support, data, and other engineering teams worked around the clock to bring everything back online.</span>
*   The Ops team instituted an emergency incident commander rotation of 8 hours per shift, once it became clear the outage was serious, in-order to keep a fresh mind in charge of the situation at all times. 
*   <span>Status updates were frequentish, even if they didn't add a lot of new information, just to let everyone know <span>Heroku</span> was working the problem. </span>

## Incident Command System

Chrishenn in a [comment](http://news.ycombinator.com/item?id=2488149) on the Heroku post on [Hacker News](http://news.ycombinator.com/item?id=2487973), thinks Heroku was using an emergency response system based on the [Incident Command System](http://en.wikipedia.org/wiki/Incident_command_system) model:  a systematic tool used for the command, control, and coordination of emergency response. A picture of the model from wikipedia:

<span class="full-image-block ssNonEditable"><span>![](http://farm6.static.flickr.com/5190/5661503400_2081acd3b3_o.png?__SQUARESPACE_CACHEVERSION=1303915402523)</span></span>

I've never heard of ICS before, but it looks worth looking into if you are searching around for a proven structure. Chrishenn says it works:

> I've experienced it first hand and can say it works very well, but I have never seen it used in this context. The great thing about it is it's expandability---it will work for teams of nearly any size. I'd be interested in seeing if any other technology companies/backend teams are using it.

## Lessons Learned

*   Have a monitoring system so you know immediately when there are problems.
*   <span>Be a really big customer so Amazon will help you specifically with your problems. This seemed to help <span>Heroku</span> a lot. I noticed in the Amazon developer forums a lot of people forgot to do this and didn't get the personal help they needed.</span>
*   <span>Have a structured emergency response process. <span>Heroku</span> has an on-call engineer, an <span>IC</span> coordinator, an escalation process, and the ability to call in all the troops when needed. It sounds like <span>Heroku</span> handled the incident like a well oiled machine. That takes practice, so practice ahead of failures instead of trying to figure out all this stuff when the <span>EBS</span> is falling.</span>
*   Recognizing that tired people make bad decisions and putting the coordinator on 8 hour rotation to prevent this type of secondary failure is really genius.
*   <span><span>Heroku's</span> status update policy shows an insightful understanding of customer psychology. Managing customer feelings and expectations was a great move. Nobody wants to feel abandoned in a pressure situation. Even if the status updates did not contain a lot of details, they did show <span>Heroku</span> was there and cared.</span>
*   <span>Do not over promise and under deliver. <span>Heroku's</span> status messages were measured, reassuring, and honest. Heroku didn't know what was wrong or when the service would be up again, so they just didn't say. Nothing creates resentment faster than lying in these situations. </span>

## Related Articles

*   [Hacker News Thread](http://news.ycombinator.com/item?id=2487973)  

</div>