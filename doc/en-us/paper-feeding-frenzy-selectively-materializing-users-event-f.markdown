## [Paper: Feeding Frenzy: Selectively Materializing Users’ Event Feeds](/blog/2012/1/17/paper-feeding-frenzy-selectively-materializing-users-event-f.html)

    

    

![](http://farm8.staticflickr.com/7148/6715530229_a401abd485_m.jpg)

How do you scale an inbox that has multiple highly volatile feeds? That's a problem faced by social networks like Tumblr, Facebook, and Twitter. Follow a few hundred event sources and it's hard to scalably order an inbox so that you see a correct view as event sources continually publish new events.

This can be considered like a [view materialization](http://en.wikipedia.org/wiki/Materialized_view) problem in a database. In a database a view is a virtual table defined by a query that can be accessed like a table. Materialization refers to when the data behind the view is created. If a view is a join on several tables and that join is performed when the view is accessed, then performance will be slow. If the view is precomputed access to the view will be fast, but more resources are used, especially considering that the view may never be accessed.

Your wall/inbox/stream is a view on all the people/things you follow. If you never look at your inbox then materializing the view in your inbox is a waste of resources, yet you'll be mad if displaying your inbox takes forever because all your event streams must be read, sorted, and filtered. 

What's a smart way of handling the materialization problem? That's what is addressed in a very good paper on the subject, [Feeding Frenzy: Selectively Materializing Users’ Event Feeds](http://research.yahoo.com/pub/3203), from researchers at Yahoo!, who found:

> The best policy is to decide whether to push or pull events on a per producer/consumer basis. This technique minimizes system cost both for workloads with a high query rate and those with a high event rate. It also exposes a knob, the push threshold, that we can tune to reduce latency in return for higher system cost.

I learned about this paper from Tumblr's Blake Matheny, in an interview with him for a forthcoming post. This is broadly how they handle the inbox problem at Tumblr. More details later.

Abstract from the paper:

> Near real-time event streams are becoming a key feature of many popular web applications. Many web sites allow users to create a personalized feed by selecting one or more event streams they wish to follow. Examples include Twitter and Facebook, which allow a user to follow other users' activity, and iGoogle and My Yahoo, which allow users to follow selected RSS streams. How can we efficiently construct a web page showing the latest events from a user's feed? Constructing such a feed must be fast so the page loads quickly, yet reflects recent updates to the underlying event streams. The wide fanout of popular streams (those with many followers) and high skew (fanout and update rates vary widely) make it difficult to scale such applications.
> 
> We associate feeds with consumers and event streams with producers. We demonstrate that the best performance results from selectively materializing each consumer's feed: events from high-rate producers are retrieved at query time, while events from lower-rate producers are materialized in advance. A formal analysis of the problem shows the surprising result that we can minimize global cost by making local decisions about each producer/consumer pair, based on the ratio between a given producer's update rate (how often an event is added to the stream) and a given consumer's view rate (how often the feed is viewed). Our experimental results, using Yahoo!'s web-scale database PNUTS, shows that this hybrid strategy results in the lowest system load (and hence improves scalability) under a variety of workloads.

## Related Articles 

*   [Feeding Frenzy: Selectively Materializing Users’ Event Feeds](http://research.yahoo.com/pub/3203) by Silberstein, A.; Terrace, J.; Cooper, B.F.; Ramakrishnan, R

    