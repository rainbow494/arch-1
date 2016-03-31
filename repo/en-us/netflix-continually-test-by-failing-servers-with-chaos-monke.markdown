## [Netflix: Continually Test by Failing Servers with Chaos Monkey](/blog/2010/12/28/netflix-continually-test-by-failing-servers-with-chaos-monke.html)

    

    

![](http://farm6.static.flickr.com/5004/5300815358_7bcb67efe1_m.jpg)

In [5 Lessons We’ve Learned Using AWS](http://techblog.netflix.com/2010/12/5-lessons-weve-learned-using-aws.html#), Netflix's John Ciancutti says the **best way to avoid failure is to fail constantly**. In the cloud it's expected instances can fail at any time, so you always have to be prepared. In the real world we prepare by running drills. Remember all those exciting fire drills? It's not just fire drills of course. The military, football teams, fire fighters, beach rescue, virtually any entity that must react quickly and efficiently to disaster hones their responsiveness by running drills.

Netflix aggressively moves this strategy into the cloud by randomly failing servers using a tool they built called Chaos Monkey. The idea is:

> If we aren’t constantly testing our ability to succeed despite failure, then it isn’t likely to work when it matters most – in the event of an unexpected outage.

They respond to failures by **degrading service**, but they always respond:

*   If the recommendations system is down they'll show popular titles instead.
*   If the search system is slow then they'll switch to showing streaming titles.

[FarmVille](http://highscalability.com/blog/2010/2/8/how-farmville-scales-to-harvest-75-million-players-a-month.html) uses a similar strategy for degrading services when link latency increases or components fail. 

This strategy fits nicely with recent practices like continuous integration, continuous deployment, and continuous testing as part of the build process. Extending this idea into a production system takes some really huge huevos and a lot of careful coding. But they are right. To really prepare you have to run drills and there's nothing better than learning from real-life experience.

## Related Articles

*   [Netflix: Use Less Chatty Protocols In The Cloud - Plus 26 Fixes](http://highscalability.com/blog/2010/12/20/netflix-use-less-chatty-protocols-in-the-cloud-plus-26-fixes.html)

    