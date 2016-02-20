## [How BuddyPoke Scales on Facebook Using Google App Engine](/blog/2010/1/22/how-buddypoke-scales-on-facebook-using-google-app-engine.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Friday, January 22, 2010 at 7:47AM</span></div>

<div class="body">

![](http://farm4.static.flickr.com/3128/2391360922_ea7903b510.jpg) How do you scale a viral Facebook app that has skyrocketed to a mind boggling 65 million installs (the population of France)? That's the fortunate problem [BuddyPoke](http://www.buddypoke.com/) co-founder Dave Westwood has and he talked about his solution at Wednesday's [Facebook Meetup](http://www.meetup.com/facebookmeetup/calendar/12179578/). Slides for the complete talk are [here](http://www.slideshare.net/cschalk/google-app-engine-and-social-apps). For those not quite sure what BuddyPoke is, it's a social network application that lets users show their mood, hug, kiss, and poke their friends through on-line avatars.

In many ways BuddyPoke is the quintessentially modern web application. It thrives off the energy of social network driven ecosystems. Game play mechanics, viral loops, and creative monetization strategies are all part of if its everyday conceptualization. It mashes together different technologies, not in a dark Frankensteining sort of way, but in a smart way that gets the most bang for the buck. Part of it runs on Facebook servers (free). Part of it runs on flash in a browser (free). Part of it runs on a storage cloud (higher cost). And part of runs on a Platform as a Service environment (that's GAE) (low cost). It also integrates tightly with other services like PayPal (a slice). Real $$ are made selling virtual goods like [gold coins](http://www.humanexperience.nl/wp-content/uploads/2009/06/buddypoke.png) redeemable in pokes. User's can also have their avatars made into dolls, t-shirts, and a whole army of other Zazzle powered gifts.

It's really quite an amazing enterprise that only makes the barest sort of sense in a modern context. I can only imagine what revolutionary era farmers would make of all this. Would BuddyPoke even be something that could have existed 10 years ago? The reason I ask is to handle all those users BuddyPoke requires enormous levels of scaling at very low costs. If you had to build out your own colo site in a datacenter could BuddyPoke ever have worked economically? Would he have ever got funding for a build out?

BuddyPoke works through a very clever mix of strategies and services. And it's evident through Dave's talk that these are all very conscious strategies. Dave talks a lot about moving functionally to different locations in order to minimize costs, selecting techniques to minimize costs, yet keep the performance high enough that users can keep on poking.

## General Lessons Learned

Before we get to specific GAE recommendations, Dave made a few interesting general observations:

1.  The cost of GAE for BuddyPoke is low. I tried to find out what the actual cost was, because some people have reported higher than expected costs, and I was wondering what his experience showed. Nobody ever answers these types of questions and Dave was no exception. He did say he payed way less than a penny per customer, but I'm not sure about the units. A penny per year? Per operation? No comment :-)
2.  The attraction to GAE's Persistence as a Service model was its simplicity. Dave said he's a 3D graphics guy, not an infrastructure guy, so he didn't want to mess with that part of the system. This is GAE's sweet spot.
3.  Most of the cost of BuddyPoke is in content delivery. The main app for BuddyPoke is a flash file must be served. These costs are much higher than the costs for running the actual application. Dave is investigating Rackspace for file serving. GAE has a relatively high failure rate for accessing content, which is acceptable when returning avatars, but is not OK for loading up the initial image.
4.  You can't code naively for GAE. In order to get performance you must select strategies that work best with how GAE works.

## Google App Engine Lessons

Here are some of the strategies BuddyPoke uses to scale on Facebook and Google App Engine:

1.  Use the browser's CPU. These CPU cycles are free whereas you pay for them in GAE. For BuddyPoke this means: 1) perform calculations in the flash client 2) wait on Facebook operations in the client, not GAE.
2.  Set maximums. Rather than allowing users to upload a large number of icons, for example, set a max of six or so, otherwise the datastore and your costs will explode.
3.  [Memcache](http://code.google.com/appengine/articles/scaling/memcache.html) everything in order to smooth out datastore issues.
4.  Use [sharded counters](http://code.google.com/appengine/articles/sharding_counters.html) for stats like install counts. Otherwise contention problems will bog down everything and shoot up your quotas.
5.  Sending large numbers of [emails](http://code.google.com/appengine/docs/java/mail/) uses a lot of CPU. Batch them using task queues or cron jobs.
6.  Use multiple [appspot](http://code.google.com/appengine/docs/whatisgoogleappengine.html) applications for different functions instead of one big application. Errors, for example, are sent to a specific error app. This keeps the logs and other stats separated by function.
7.  Split a table into two parts: main model and an additional info model. Datastore puts are your largest cost so you need to do everything you can to minimize their cost. In the main model use keyed access only, no indexes, and keep no extra fields. Gets will be fast and puts will be cheap. Writing extra fields costs time and money. So a “date joined” field for a User model doesn't belong in the main table. It isn't used often so put this kind of data in an additional info model. Put your indexes on this model and perform queries against it. Because the additional info model has indexes it will cause more synchronized writes which costs more, is slower, and can back up processing if you need to write this model frequently.
8.  To minimize datastore hops use batch gets and puts.

On the surface BuddyPoke seems simple, but under hood there's some intricate strategy going on. Minimizing costs while making it scale and perform is not obvious. Who does what, when, why and how takes some puzzling out. It's certainly an approach a growing class of apps will find themselves using in the future.

## Related Articles

1.  [Ustream Video of the Talk](http://www.ustream.tv/recorded/4118948)
2.  [Building Scalable, Complex Apps on App Engine](http://code.google.com/events/io/2009/sessions/BuildingScalableComplexApps.html) by Brett Slatkin
3.  [Google App Engine - A Second Look](http://highscalability.com/blog/2009/2/21/google-appengine-a-second-look.html)
4.  [BuddyPoke on Google App Engine](http://www.youtube.com/watch?v=0zz-oSrWfj0)

</div>