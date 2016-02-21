## [How Facebook Tells Your Friends You're Safe in a Disaster in Under Five Minutes](/blog/2015/9/28/how-facebook-tells-your-friends-youre-safe-in-a-disaster-in.html)

    

    

![](https://c1.staticflickr.com/1/652/21759732026_52643d03bd_m.jpg)

_Here's an article update: [How Facebook's Safety Check Works](http://highscalability.com/blog/2015/11/14/how-facebooks-safety-check-works.html)._

    In a disaster there’s a raw and immediate need to know your loved ones are safe. I felt this way during 9/11\. I know I’ll feel this way during the next wild fire in our area. And I vividly remember feeling this way during the     [    1989 Loma Prieta earthquake    ](https://en.wikipedia.org/wiki/1989_Loma_Prieta_earthquake)    .    

    Most earthquakes pass beneath notice. Not this one and everyone knew it. After ceiling tiles stopped falling like snowflakes in the computer lab, we convinced ourselves the building would not collapse, and all thoughts turned to the safety of loved ones. As it must have for everyone else. Making an outgoing call was nearly impossible, all the phone lines were busy as calls poured into the Bay Area from all over the nation. Information was stuck. Many tense hours were spent in ignorance as the TV showed a constant stream of death and destruction.    

    It’s over a quarter of a century later, can we do any better?    

    Facebook can. Through a product called     [    Safety Check    ](https://www.facebook.com/about/safetycheck/)    , which connects friends and loved ones during a disaster. When a disaster hits Safety Check prompts people in the area to indicate if they are OK or not. Then Facebook closes the worry loop by telling their friends how they are doing.    

[    Brian Sa    ](https://www.linkedin.com/pub/brian-sa/2/7a7/a3b)    , Engineer Manager at Facebook, created Safety Check out of his experience of the devastating earthquake in     [    Fukushima Japan in 2011    ](http://www.telegraph.co.uk/news/worldnews/asia/japan/8953574/Japan-earthquake-tsunami-and-Fukushima-nuclear-disaster-2011-review.html)    . He told his very moving story in a talk he     [    gave at @Scale    ](https://www.youtube.com/watch?v=ptsCWGZW_P8)    .    

    During the earthquake Brian put a banner on Facebook with helpful information sources, but he was moved to find a better way to help people in need. That impulse became Safety Check.    

    My first reaction to Safety Check was damn, why didn’t anyone think of this before? It’s such a powerful idea.    

    The answer became clear as I listened to a talk in the    [     same video    ](https://www.youtube.com/watch?v=ptsCWGZW_P8)     given by     [    Peter Cottle    ](https://www.linkedin.com/pub/peter-cottle/a/961/387)    , Software Engineer at Facebook, who also talked about building Safety Check.    

    It’s likely only Facebook could have created Safety Check. This observation dovetails nicely with Brian’s main lesson in his talk:    

*       **Solve real-world problem in a way that only YOU can**        . Instead of taking the conventional route, think about the unique role you and your company can play.    

    Only Facebook could create Safety Check, not because of resources as you might expect, but because Facebooks lets employees build crazy things like Safety Check and because only Facebook has 1.5 billion geographically distributed users, with a degree of separation between them of only 4.74 edges, and only Facebook has users who are fanatical about reading their news feeds. More about this later.    

    In fact, Peter talked about how resources were a problem in a sort of product development Catch-22 at Facebook. The team for Safety Check was small and didn’t have a lot of resources attached to it. They had to build the product and prove its success without resources before they could get the resources to build the product. The problem had to be efficiently solved at scale without the application of lots of money and lots of resources.    

    As is often the case constraints led to a clever solution. A small team couldn’t build a big pipeline and index, so they wrote some hacky PHP and effectively got the job done at scale.    

    So how did Facebook build Safety Check? Here’s my gloss on both Brian’s and Peter’s talks:    

*       You can think of Facebook as this giant primordial soup. Billions of people use Facebook and over time behaviours begin to emerge, there are trends that go viral.    

    *       One viral example is how users started updating their profile pictures to a red equality sign as a way to represent marriage equality.    

    *       In support Facebook built a rainbow overlay tool that made it a lot easier to use the profile picture as a means of communication. A practice that had it origins in the very earliest of Facebook. Before the invention of the status message users would change their profile picture to communicate their current zeitgeist.    

    *   Making it easy to customize profile pictures**increased adoption by 1000x**.

*       This gave them the idea of         **making it easier for people to tell their friends they were OK**         during a disaster.    

    *       During a disaster people would update their status saying they were OK.    

    *       This is not an ideal solution to the problem of letting people know you are OK.    

    *       It’s not very structured. There are problems in both directions, the telling people you are OK direction and your friends getting the information that you’re OK direction.    

    *       First, not all your friends will see the update.    

    *       Second, there’s no way for users to get a list of all their friends who impacted by a disaster.    

    *       Applying more structured thinking to the problem of disaster status notification resulted in     [    Safety Check    ](https://www.facebook.com/about/safetycheck/)    .    

*       How it works:    

    *       If you are in an area impacted by a disaster Facebook will send you a push notification asking if you are OK. (nothing is said on who decides when a disaster occurs).    

    *       Tapping the “I’m Safe” button marks that your are safe.    

    *       All your friends are notified that you are safe.    

    *       Friends can also see a list of all the people impacted by the disaster and how they are doing.    

*       How do you build the pool of people impacted by a disaster in a certain area? Building a geoindex is the obvious solution, but it has weaknesses.    

    *       People are constantly moving so the index will be stale.    

    *       A geoindex of 1.5 billion people is huge and would take a lot of resources they didn’t have. Remember, this is a small team without a lot of resources trying to implement a solution.    

    *       Instead of keeping a data pipeline that’s rarely used active all of the time, the solution should work only when there is an incident. This requires being able to make a query that is         **dynamic and instant**        .    

*       The solution         **leveraged the shape of the social graph and its properties**        .    

    *       When there’s a disaster, say an earthquake in Nepal, a hook for Safety Check is         **turned on in every single news feed load**        .    

    *   **When people check their news feed the hook executes**. If the person checking their news feed is not in Nepal then nothing happens.

    *       When someone in Nepal checks their news feed is when the magic happens.    

    *       Safety Check         **fans out to all their friends**         on their social graph. If a friend is in the same area then a push notification is sent asking if they are OK.    

    *       **The process keeps repeating recursively**        . For every friend found in the disaster area a job is spawned to check their friends. Notifications are sent as needed.    

*   In practice this **solution was** **very effective**. The product experience feels live and instant because the algorithm is so fast at finding people. Everyone in the same room, for example, will appear to get their notifications at the same time. Why?

    *   Using the news feed gives a **random sampling of users that is biased towards the most active users** **with the most friends**. And it filters out inactive users, which is billions of rows of computation which need not be performed.

    *       The         **graph is dense and interconnected**        .     [    Six Degrees of Kevin Bacon    ](https://en.wikipedia.org/wiki/Six_Degrees_of_Kevin_Bacon)     is wrong, at least on Facebook. The         **average distance between any two of Facebook’s 1.5 billion users is 4.74 edges**        . Sorry Kevin. With 1.5 billion users the whole graph can be explored within 5 hops. Most people can be efficiently reached by following the social graph.    

    *       There’s         **a lot of parallelism**         for free using a social graph approach. Friends can be assigned to different machines and processed in parallel. As can their friends, and so on.    

*       **Race Conditions became a problem**         do to the distributed nature of the solution.    

    *       Two machines in two different datacenters have a user that’s friends with the same person. This means both edges are traversed which ends up sending two notifications to the same person.    

    *       Didn’t think this would be a big deal, but in practice users found it really stressful to get multiple notifications in a disaster scenario. Users thought they must be in real trouble if they got two notifications at the same time. And you also don’t want your safety related project to feel buggy in the middle of a disaster.    

    *       A database was used to store state so only one machine would check a user. The problem is when you have multiple datacenters, say one in Europe and one in the US, the propagation delay makes it take too long to determine if a user is being processed yet.    

    *       An in-memory locking structure was added for a two layer solution. The in-memory lock is checked and if the bit is set for a user the job for that user is aborted.    

*       Biggest product launch was for an earthquake in Nepal.    

    *       In just under 5 minutes 3 million people were invited to give their status. There were so many people in Nepal that 1 billion rows were checked.    

    *   **2/3rds of Facebook’s user base was explored in about 5 minutes**.

*       Ran into some problems building the first version of Safety Check.    

    *       Fragmentation over the desktop, mobile web, and native apps created enormous complexities in managing the content, features, and operations across multiple platforms.    

    *       Wanted to personalize the content which required hand-coding which was slow, tedious, and error prone.    

    *       When a system is only on during an emergency it’s hard to tell if it will work during the emergency.    

    *       So what kind of client-server model should be adopted in order to build a system that is cross platform yet still responds natively? How can dynamic content be included? How can the system be tested to make sure it’s reliable?    

*       Chose to have the server responsible for text and images and configuration information. The client would prefetch data, handle real-time invalidation, and respond to client events natively.    

*       Real-time invalidation.    

    *       Whenever you are dealing with prefetched data it’s important to specify when that data should expire. For example, if a user opens the app and some data is uploaded, then they close the app and only open it a few days later. If the data that was uploaded was an election announcement then if the election is passed the user should not see the announcement.    

    *       A TTL (time to live) parameter tells clients how long they can use the data before it needs to be refreshed.    

    *       An interaction count indicates how many times the user can interact with data.    

    *       More optional filters, like remaining_battery_percentage, which would tell the client not to show the user a message if the device is in a low battery condition.    

*       Enforcing content limits    

    *       Specifying the maximum number of lines in text is a bad idea. A string can grow dramatically when translated to other languages.    

    *       Different platforms and devices have different screen sizes, so one fixed limit doesn’t work.    

    *       So define content length restrictions on the server and build in a buffer to account for long translations.    

*       Dynamic content    

    *       This allows Safety Check messages to be personalized, like insert a user name into the text.    

    *       Came up with a language, like a mail merge, to specify substitutions like “name of the crisis” and “crisis URL”.    

*       Positive caching    

    *       Where you remember the units that were previously eligible    

    *       Safety Check is the type of unit that maintains a fixed channel of communication with the user. For the event duration there are no view limits. If it is turned on on Friday there’s constant delivery until it’s turned off on Friday.    

    *       Positive caching benefits long-running units because you don’t have to evaluate the eligibility of any of the units. You do have to check the eligibility of cached units because you don’t want to show it any more times than intended.    

*       Negative caching    

    *       Where you remember no units were eligible.    

    *       Amber Alert is a one time message. The load profile for this type of unit is really different. For example, if it’s turned on at 8:50AM, by 9:00AM it’s already reached peak delivery and then it decays really quickly. In one hour half the people have been reached, in 3 hours nearly all the target audience has been reached.    

    *       Negative caching benefits short-running units because most of the time there are not eligible units to show. Negative caching remembers that fact.    

*       Dark Launch    

    *       Tested the system with live traffic to find unexpected problems. A flag was used to keep it hidden from users so there was no user impact.    

    *       Running the test for 24 hours found some unexpected issues. There was insufficient capacity in one of the subsystems that generated social sentences.    

*       Kill Switches    

    *       Facebook is comprised of hundreds of interlocking systems.    

    *       Sometimes in the case of a catastrophic failure you need to shed load quickly.    

    *       Kill switches allow you to completely turn off an entire system. You can also kill specific app versions or devices.    

## Related Articles

*   [On reddit](https://www.reddit.com/r/tech/comments/3mq29e/how_facebook_tells_your_friends_youre_safe_in_a/)

    