## [How Google Taught Me to Cache and Cash-In](/blog/2009/9/12/how-google-taught-me-to-cache-and-cash-in.html)

    

    

A user named Apathy[](http://highscalability.com/links/goto/759/643/links_weblink) on how Reddit scales some of their features, shares some advice he learned while working at Google and other major companies.  

_To be fair, I [Apathy] was working at Google at the time, and every job I held between 1995 and 2005 involved at least one of the largest websites on the planet. I didn't come up with any of these ideas, just watched other smart people I worked with who knew what they were doing and found (or wrote) tools that did the same things. But the theme is always the same:_

1.  Cache everything you can and store the rest in some sort of database (not necessarily relational and not necessarily centralized).
2.  Cache everything that doesn't change rapidly. Most of the time you don't have to hit the database for anything other than checking whether the users' new message count has transitioned from 0 to (1 or more).
3.  Cache everything--templates, user message status, the front page components--and hit the database once a minute or so to update the front page, forums, etc. This was sufficient to handle a site with a million hits a day on a couple of servers. The site was sold for $100K.
4.  Cache the users' subreddits. Blow out the cache on update.
5.  Cache the top links per subreddit. Blow out cache on update.
6.  Combine the previous two steps to generate a menu from cached blocks.
7.  Cache the last links. Blow out the cache on each outlink click.
8.  Cache the user's friends. Append 3 characters to their name.
9.  Cache the user's karma. Blow out on up/down vote.
10.  Filter via conditional formatting, CSS, and an ajax update.
11.  Decouple selection/ranking algorithm(s) from display.
12.  Use Google or something like Xapian or Lucene for search.
13.  Cache "for as long as memcached will stay up." That depends on how many instances you're running, what else is running, how stable the Python memcached hooks are, etc.
14.  The golden rule of website engineering is that you don't try to enforce partial ordering simultaneously with your updates.
15.  When running a search engine operate the crawler separately from the indexer.
16.  Ranking scores are used as necessary from the index, usually cached for popular queries.
17.  Re-rank popular subreddits or the front page once a minute. Tabulate votes and pump them through the ranker.
18.  Cache the top 100 per subreddit. Then cache numbers 100-200 when someone bothers to visit the 5th page of a subreddit, etc.
19.  For less-popular subreddits, you cache the results until an update comes in.
20.  With enough horsepower and common sense, almost any volume of data can be managed, just not in realtime.
21.  Never ever mix your reads and writes if you can help it.
22.  Merge all the normalized rankings and cache the output every minute or so. This avoids thousands of queries per second just for personalization.
23.  It's a lot cheaper to merge cached lists than build them from scratch. This delays the crushing read/write bottleneck at the database. But you have to write the code.
24.  Layering caches is a clasisc strategy for milking your servers as much as possilbe. First look for an exact match. If that's not found, look for the components and build an exact match.
25.  The majority of traffic on almost all websites comes from the default, un-logged-in front page or from random forum/comment/result pages. Make sure those are cached as much as possible.. If one or more of the components aren't found, regenerate those from the DB (now it's cached!) and proceed. Never hit the database unless you have to.
26.  You (almost) always have to hit the database on writes. The key is to avoid hitting it for reads until you're forced to do so.

    