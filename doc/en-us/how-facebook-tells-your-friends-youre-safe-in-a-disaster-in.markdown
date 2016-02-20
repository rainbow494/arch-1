## [How Facebook Tells Your Friends You're Safe in a Disaster in Under Five Minutes](/blog/2015/9/28/how-facebook-tells-your-friends-youre-safe-in-a-disaster-in.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, September 28, 2015 at 8:56AM</span></div>

<div class="body">

![](https://c1.staticflickr.com/1/652/21759732026_52643d03bd_m.jpg)

_Here's an article update: [How Facebook's Safety Check Works](http://highscalability.com/blog/2015/11/14/how-facebooks-safety-check-works.html)._

<span>In a disaster there’s a raw and immediate need to know your loved ones are safe. I felt this way during 9/11\. I know I’ll feel this way during the next wild fire in our area. And I vividly remember feeling this way during the</span> [<span>1989 Loma Prieta earthquake</span>](https://en.wikipedia.org/wiki/1989_Loma_Prieta_earthquake)<span>.</span>

<span>Most earthquakes pass beneath notice. Not this one and everyone knew it. After ceiling tiles stopped falling like snowflakes in the computer lab, we convinced ourselves the building would not collapse, and all thoughts turned to the safety of loved ones. As it must have for everyone else. Making an outgoing call was nearly impossible, all the phone lines were busy as calls poured into the Bay Area from all over the nation. Information was stuck. Many tense hours were spent in ignorance as the TV showed a constant stream of death and destruction.</span>

<span>It’s over a quarter of a century later, can we do any better?</span>

<span>Facebook can. Through a product called</span> [<span>Safety Check</span>](https://www.facebook.com/about/safetycheck/)<span>, which connects friends and loved ones during a disaster. When a disaster hits Safety Check prompts people in the area to indicate if they are OK or not. Then Facebook closes the worry loop by telling their friends how they are doing.</span>

[<span>Brian Sa</span>](https://www.linkedin.com/pub/brian-sa/2/7a7/a3b)<span>, Engineer Manager at Facebook, created Safety Check out of his experience of the devastating earthquake in</span> [<span>Fukushima Japan in 2011</span>](http://www.telegraph.co.uk/news/worldnews/asia/japan/8953574/Japan-earthquake-tsunami-and-Fukushima-nuclear-disaster-2011-review.html)<span>. He told his very moving story in a talk he</span> [<span>gave at @Scale</span>](https://www.youtube.com/watch?v=ptsCWGZW_P8)<span>.</span>

<span>During the earthquake Brian put a banner on Facebook with helpful information sources, but he was moved to find a better way to help people in need. That impulse became Safety Check.</span>

<span>My first reaction to Safety Check was damn, why didn’t anyone think of this before? It’s such a powerful idea.</span>

<span>The answer became clear as I listened to a talk in the</span>[ <span>same video</span>](https://www.youtube.com/watch?v=ptsCWGZW_P8) <span>given by</span> [<span>Peter Cottle</span>](https://www.linkedin.com/pub/peter-cottle/a/961/387)<span>, Software Engineer at Facebook, who also talked about building Safety Check.</span>

<span>It’s likely only Facebook could have created Safety Check. This observation dovetails nicely with Brian’s main lesson in his talk:</span>

*   <span>**Solve real-world problem in a way that only YOU can**</span><span>. Instead of taking the conventional route, think about the unique role you and your company can play.</span>

<span>Only Facebook could create Safety Check, not because of resources as you might expect, but because Facebooks lets employees build crazy things like Safety Check and because only Facebook has 1.5 billion geographically distributed users, with a degree of separation between them of only 4.74 edges, and only Facebook has users who are fanatical about reading their news feeds. More about this later.</span>

<span>In fact, Peter talked about how resources were a problem in a sort of product development Catch-22 at Facebook. The team for Safety Check was small and didn’t have a lot of resources attached to it. They had to build the product and prove its success without resources before they could get the resources to build the product. The problem had to be efficiently solved at scale without the application of lots of money and lots of resources.</span>

<span>As is often the case constraints led to a clever solution. A small team couldn’t build a big pipeline and index, so they wrote some hacky PHP and effectively got the job done at scale.</span>

<span>So how did Facebook build Safety Check? Here’s my gloss on both Brian’s and Peter’s talks:</span>

*   <span>You can think of Facebook as this giant primordial soup. Billions of people use Facebook and over time behaviours begin to emerge, there are trends that go viral.</span>

    *   <span>One viral example is how users started updating their profile pictures to a red equality sign as a way to represent marriage equality.</span>

    *   <span>In support Facebook built a rainbow overlay tool that made it a lot easier to use the profile picture as a means of communication. A practice that had it origins in the very earliest of Facebook. Before the invention of the status message users would change their profile picture to communicate their current zeitgeist.</span>

    *   Making it easy to customize profile pictures**increased adoption by 1000x**.

*   <span>This gave them the idea of</span> <span>**making it easier for people to tell their friends they were OK**</span> <span>during a disaster.</span>

    *   <span>During a disaster people would update their status saying they were OK.</span>

    *   <span>This is not an ideal solution to the problem of letting people know you are OK.</span>

    *   <span>It’s not very structured. There are problems in both directions, the telling people you are OK direction and your friends getting the information that you’re OK direction.</span>

    *   <span>First, not all your friends will see the update.</span>

    *   <span>Second, there’s no way for users to get a list of all their friends who impacted by a disaster.</span>

    *   <span>Applying more structured thinking to the problem of disaster status notification resulted in</span> [<span>Safety Check</span>](https://www.facebook.com/about/safetycheck/)<span>.</span>

*   <span>How it works:</span>

    *   <span>If you are in an area impacted by a disaster Facebook will send you a push notification asking if you are OK. (nothing is said on who decides when a disaster occurs).</span>

    *   <span>Tapping the “I’m Safe” button marks that your are safe.</span>

    *   <span>All your friends are notified that you are safe.</span>

    *   <span>Friends can also see a list of all the people impacted by the disaster and how they are doing.</span>

*   <span>How do you build the pool of people impacted by a disaster in a certain area? Building a geoindex is the obvious solution, but it has weaknesses.</span>

    *   <span>People are constantly moving so the index will be stale.</span>

    *   <span>A geoindex of 1.5 billion people is huge and would take a lot of resources they didn’t have. Remember, this is a small team without a lot of resources trying to implement a solution.</span>

    *   <span>Instead of keeping a data pipeline that’s rarely used active all of the time, the solution should work only when there is an incident. This requires being able to make a query that is</span> <span>**dynamic and instant**</span><span>.</span>

*   <span>The solution</span> <span>**leveraged the shape of the social graph and its properties**</span><span>.</span>

    *   <span>When there’s a disaster, say an earthquake in Nepal, a hook for Safety Check is</span> <span>**turned on in every single news feed load**</span><span>.</span>

    *   **When people check their news feed the hook executes**. If the person checking their news feed is not in Nepal then nothing happens.

    *   <span>When someone in Nepal checks their news feed is when the magic happens.</span>

    *   <span>Safety Check</span> <span>**fans out to all their friends**</span> <span>on their social graph. If a friend is in the same area then a push notification is sent asking if they are OK.</span>

    *   <span>**The process keeps repeating recursively**</span><span>. For every friend found in the disaster area a job is spawned to check their friends. Notifications are sent as needed.</span>

*   In practice this **solution was** **very effective**. The product experience feels live and instant because the algorithm is so fast at finding people. Everyone in the same room, for example, will appear to get their notifications at the same time. Why?

    *   Using the news feed gives a **random sampling of users that is biased towards the most active users** **with the most friends**. And it filters out inactive users, which is billions of rows of computation which need not be performed.

    *   <span>The</span> <span>**graph is dense and interconnected**</span><span>.</span> [<span>Six Degrees of Kevin Bacon</span>](https://en.wikipedia.org/wiki/Six_Degrees_of_Kevin_Bacon) <span>is wrong, at least on Facebook. The</span> <span>**average distance between any two of Facebook’s 1.5 billion users is 4.74 edges**</span><span>. Sorry Kevin. With 1.5 billion users the whole graph can be explored within 5 hops. Most people can be efficiently reached by following the social graph.</span>

    *   <span>There’s</span> <span>**a lot of parallelism**</span> <span>for free using a social graph approach. Friends can be assigned to different machines and processed in parallel. As can their friends, and so on.</span>

*   <span>**Race Conditions became a problem**</span> <span>do to the distributed nature of the solution.</span>

    *   <span>Two machines in two different datacenters have a user that’s friends with the same person. This means both edges are traversed which ends up sending two notifications to the same person.</span>

    *   <span>Didn’t think this would be a big deal, but in practice users found it really stressful to get multiple notifications in a disaster scenario. Users thought they must be in real trouble if they got two notifications at the same time. And you also don’t want your safety related project to feel buggy in the middle of a disaster.</span>

    *   <span>A database was used to store state so only one machine would check a user. The problem is when you have multiple datacenters, say one in Europe and one in the US, the propagation delay makes it take too long to determine if a user is being processed yet.</span>

    *   <span>An in-memory locking structure was added for a two layer solution. The in-memory lock is checked and if the bit is set for a user the job for that user is aborted.</span>

*   <span>Biggest product launch was for an earthquake in Nepal.</span>

    *   <span>In just under 5 minutes 3 million people were invited to give their status. There were so many people in Nepal that 1 billion rows were checked.</span>

    *   **2/3rds of Facebook’s user base was explored in about 5 minutes**.

*   <span>Ran into some problems building the first version of Safety Check.</span>

    *   <span>Fragmentation over the desktop, mobile web, and native apps created enormous complexities in managing the content, features, and operations across multiple platforms.</span>

    *   <span>Wanted to personalize the content which required hand-coding which was slow, tedious, and error prone.</span>

    *   <span>When a system is only on during an emergency it’s hard to tell if it will work during the emergency.</span>

    *   <span>So what kind of client-server model should be adopted in order to build a system that is cross platform yet still responds natively? How can dynamic content be included? How can the system be tested to make sure it’s reliable?</span>

*   <span>Chose to have the server responsible for text and images and configuration information. The client would prefetch data, handle real-time invalidation, and respond to client events natively.</span>

*   <span>Real-time invalidation.</span>

    *   <span>Whenever you are dealing with prefetched data it’s important to specify when that data should expire. For example, if a user opens the app and some data is uploaded, then they close the app and only open it a few days later. If the data that was uploaded was an election announcement then if the election is passed the user should not see the announcement.</span>

    *   <span>A TTL (time to live) parameter tells clients how long they can use the data before it needs to be refreshed.</span>

    *   <span>An interaction count indicates how many times the user can interact with data.</span>

    *   <span>More optional filters, like remaining_battery_percentage, which would tell the client not to show the user a message if the device is in a low battery condition.</span>

*   <span>Enforcing content limits</span>

    *   <span>Specifying the maximum number of lines in text is a bad idea. A string can grow dramatically when translated to other languages.</span>

    *   <span>Different platforms and devices have different screen sizes, so one fixed limit doesn’t work.</span>

    *   <span>So define content length restrictions on the server and build in a buffer to account for long translations.</span>

*   <span>Dynamic content</span>

    *   <span>This allows Safety Check messages to be personalized, like insert a user name into the text.</span>

    *   <span>Came up with a language, like a mail merge, to specify substitutions like “name of the crisis” and “crisis URL”.</span>

*   <span>Positive caching</span>

    *   <span>Where you remember the units that were previously eligible</span>

    *   <span>Safety Check is the type of unit that maintains a fixed channel of communication with the user. For the event duration there are no view limits. If it is turned on on Friday there’s constant delivery until it’s turned off on Friday.</span>

    *   <span>Positive caching benefits long-running units because you don’t have to evaluate the eligibility of any of the units. You do have to check the eligibility of cached units because you don’t want to show it any more times than intended.</span>

*   <span>Negative caching</span>

    *   <span>Where you remember no units were eligible.</span>

    *   <span>Amber Alert is a one time message. The load profile for this type of unit is really different. For example, if it’s turned on at 8:50AM, by 9:00AM it’s already reached peak delivery and then it decays really quickly. In one hour half the people have been reached, in 3 hours nearly all the target audience has been reached.</span>

    *   <span>Negative caching benefits short-running units because most of the time there are not eligible units to show. Negative caching remembers that fact.</span>

*   <span>Dark Launch</span>

    *   <span>Tested the system with live traffic to find unexpected problems. A flag was used to keep it hidden from users so there was no user impact.</span>

    *   <span>Running the test for 24 hours found some unexpected issues. There was insufficient capacity in one of the subsystems that generated social sentences.</span>

*   <span>Kill Switches</span>

    *   <span>Facebook is comprised of hundreds of interlocking systems.</span>

    *   <span>Sometimes in the case of a catastrophic failure you need to shed load quickly.</span>

    *   <span>Kill switches allow you to completely turn off an entire system. You can also kill specific app versions or devices.</span>

## Related Articles

*   [On reddit](https://www.reddit.com/r/tech/comments/3mq29e/how_facebook_tells_your_friends_youre_safe_in_a/)

</div>