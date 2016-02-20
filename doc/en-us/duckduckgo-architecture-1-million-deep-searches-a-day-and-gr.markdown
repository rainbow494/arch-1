## [DuckDuckGo Architecture - 1 Million Deep Searches a Day and Growing](/blog/2013/1/28/duckduckgo-architecture-1-million-deep-searches-a-day-and-gr.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, January 28, 2013 at 9:30AM</span></div>

<div class="body">

![](http://farm9.staticflickr.com/8227/8424385194_15feca732d_o.png)_This is an interview with [Gabriel Weinberg](https://twitter.com/yegg), founder of [Duck Duck Go](https://duckduckgo.com/) and general [all around startup guru](http://www.gabrielweinberg.com/blog/), on what DDG’s architecture looks like in 2012._

Innovative search engine upstart DuckDuckGo had [<span>30 million searches</span>](https://duckduckgo.com/traffic.html) in February 2012 and averages over 1 million searches a day. It’s being positioned by [<span>super investor Fred Wilson</span>](http://www.avc.com/a_vc/2012/02/duck-duck-go-passed-1mm-searches-per-day.html) as a clean, private, impartial and fast search engine. After talking with Gabriel I like what Fred Wilson said earlier, it seems closer to the heart of the matter: [<span>We invested in DuckDuckGo for the Reddit, Hacker News anarchists</span>](http://venturebeat.com/2012/05/21/fred-wilson-duckduckgo-reddit-hacker-news/).  

Choosing DuckDuckGo can be thought of as not just a technical choice, but a vote for revolution. In an age when knowing your essence is not about about love or friendship, but about more effectively selling you to advertisers, DDG is positioning themselves as the [<span>do not track alternative</span>](http://whatisdnt.com/), keepers of the [privacy flame](https://duckduckgo.com/privacy). You will still be monetized of course, but in a more civilized and anonymous way.   

Pushing privacy is a good way to carve out a competitive niche against Google et al, as by definition they can never compete on privacy. I get that. But what I found most compelling is DDG’s strong vision of a crowdsourced network of plugins giving broader search coverage by tying an army of vertical data suppliers into their search framework. For example, there's a specialized Lego plugin for searching against a complete Lego database. Use the name of a spice in your search query, for example, and DDG will recognize it and may trigger a deeper search against a highly tuned recipe database. Many different plugins can be triggered on each search and it’s all handled in real-time.  

Can’t searching the Open Web provide all this data? No really. This is structured data with semantics. Not an HTML page. You need a search engine that’s capable of categorizing, mapping, merging, filtering, prioritizing, searching, formatting, and disambiguating richer data sets and you can’t do that with a keyword search. You need the kind of smarts DDG has built into their search engine. One problem of course is now that data has become valuable many grown ups don’t want to share anymore.  

Being ad supported puts DDG in a tricky position. Targeted ads are more lucrative, but ironically DDG’s do not track policies means they can’t gather targeting data. Yet that’s also a selling point for those interested in privacy. But as search is famously intent driven, DDG’s technology of categorizing queries and matching them against data sources is already a form of high value targeting.  

It will be fascinating to see how these forces play out. But for now let’s see how DuckDuckGo implements their search engine magic...

## <span>Information Sources</span>

*   <span>Personal interview with Gabriel Weinberg.</span>
*   <span>Resources listed under</span> <span>**Related Articles**</span><span>.</span>

## Stats

*   30 million searches in February 2012
*   Averages over 1 million searches a day
*   12M API requests per day

## <span>The Platform</span>

*   <span>EC2</span>
*   <span>Ubuntu</span>
*   <span>Perl & CPAN</span>
*   [<span>Server Density</span>](http://www.serverdensity.com/) <span>- monitoring</span>
*   <span>Solr</span>
*   <span>PostgreSQL</span>
*   <span>Memcached</span>
*   [<span>Bucardo</span>](http://bucardo.org/) <span>- asynchronous PostgreSQL replication system</span>
*   [<span>Global Traffic Director</span>](http://www.dnsmadeeasy.com/services/global-traffic-director/) <span>- load balancing between regions</span>
*   <span>Nginx</span>
*   [<span>getFavicon</span>](http://getfavicon.appspot.com/) <span>- serves favicons</span>
*   [<span>daemontools</span>](http://en.wikipedia.org/wiki/Daemontools) <span>- free tools for managing Unix services</span>
*   <span>Git</span>
*   [<span>Asana</span>](http://asana.com/) <span>- project management.</span>
*   <span>HipChat - internal communication</span>
*   <span>Yammer</span>
*   <span>JavaScript</span>
*   <span>YUI (moving to jQuery)</span>

## <span>What's Inside</span>

*   <span>What Gabriel likes about their system:</span>
    *   <span>Very simple, though they have become more complex over time. Complexity is  mitigated by everything being modular. Daemontools is used to keep services running, everything runs through Nginx. There are lots of different services, but the architecture is such that they all stay up.</span>
    *   <span>Main goals have been 100% uptime and speed. Reducing complexity helps with both goals.</span>

## <span>Out of the Basement and into AWS (mostly)</span>

*   <span>DuckDuckGo used to run out of Gabriel’s basement. Most components, including all front-end components, are now on AWS.</span>
    *   Some “persistent machines” are still in the basement as there has been no compelling reason to move these components, like the Git repository, crawling, and the real-time Wikipedia index that updates [zero-click connects](http://dhruvbird.com/ddb/zc.html).
    *   <span>Linode is used to host some community functionality, like translations.</span>
    *   <span>Moved to Ubuntu from FreeBSD as part of the move to AWS.</span>
*   <span>Moved to multiple regions for better front-end performance. Make DDG fast everywhere and the users will come.</span>
    *   <span>One complaint users had is speed. By running in 4 AWS datacenters (California, Virginia, Singapore, Ireland) DDG is able to be closer to its users around the world.</span>
    *   <span>Identical software runs in all datacenters.</span>
    *   Global Traffic Director is used for their DNS and to load balance users across regions. DDG would like to use more regions (South America and another Asian datacenter) but the Traffic Director currently only works in four regions.
*   <span>Got off EBS completely due to EBS being involved in most of the large failures. Also experienced performance variability with EBS.</span>
    *   <span>The new Provisioned IOPs is supposed to handle that, but having fewer extra moving pieces in the architecture is better.</span>
    *   <span>May experiment with PIOPs in the future, though the current non-EBS architecture is working well.</span>
*   <span>Run mainly on extra large machines as they seem to be the sweet spot for high IO for network and they have 4 ephemeral disks.</span>
    *   <span>Attracted by benchmarks showing the extra large machines have the lowest performance variance.</span>
    *   <span>Found 4 ephemeral disks performed better than striping 8 EBS disks.</span>
    *   <span>Not interested in high IO instance types because the goal is to keep as much data as in memory as possible. High transaction rates are not a problem and data is cached as fast as possible. Plus machines don’t show high IO utilization rates.</span>
*   <span>Medium instances are used for developer machines. Everyone has their own medium dev instance. Staging instances are used for testing, which are sufficient to mimic the live environment.</span>
*   <span>Datacenter syncing.</span>
    *   <span>Primarily deal with read-only data. Updates are pushed out all the time, but they don’t have to be immediate. Means syncing between datacenters is not a datacenter problem. There aren’t really any consistency issues.</span>
    *   <span>Backend systems (non-AWS) have a master databases that pushout updates to the regions.</span>
*   <span>Distributed caching system uses memcached.</span>
    *   <span>High memory m2.xlarge are used for caching.</span>
    *   <span>Approximately 100GB in size, sharded across m2.xlarge machines.</span>
    *   <span>When something is cached it pushes out to all the other caching systems. There’s no master.</span>
    *   <span>Custom Perl solution.</span>
    *   <span>Caching is routed through Nginx so requests bypass the Perl backend completely if the data is in cache.</span>
    *   <span>Not constrained by size, they can add as many cache machines as needed, the challenge is to figure out what to put in the cache so it will be useful and not to give bad results.</span>
    *   <span>Working on smarter cache aging algorithms. Look at the organic links that are coming back. You can tell alot about about a link from their associated snippets and domain. The problem is this signal is only available after the results come back, so it’s slow for the UI, but it’s great for caching.</span>
    *   <span>Priority has been to get syncing correct in all the regions. Now they’ll be working on making smarter cache.</span>
*   <span>Many databases are used, including PostgreSQL, Solr, Berkeley, and flat files.</span>
    *   <span>Bucardo is used for PostgreSQL replication.</span>
    *   <span>Solr syncing is via their own process, not the built in replication.</span>
    *   <span>PostgreSQL masters are in the basement and the slaves are in each region.</span>
    *   <span>PostgreSQL holds instant answer and entity data. If you type in “duckduckgo”, for example, you get something from Wikipedia and that comes from PostgreSQL.</span>
    *   <span>The PostgreSQL database has something like a 100 sources of data. These are crawled by the backend and stored in the database.</span>

## <span>Searching - It's Complicated</span>

*   <span>High level: try to figure what a query is about  and then route it to the appropriate datastores and APIs that are appropriate for it. And in some cases bring it back, parallelize it, and mix it together.</span>
*   <span>The zero-click, instance answer box, could be powered by PostgreSQL, Solr, on the fly APIs, flat files, or a combination.</span>
*   <span>Link results are API driven, though top links may come from other sources.  </span>[<span>Link sources include</span>](http://help.duckduckgo.com/customer/portal/articles/216399-sources)<span>: Bing, Yahoo, Yandex, Blekko, WolframAlpha, and many others.</span>
*   <span>All parsing and merging logic is in Perl.</span>
*   <span>Two levels of merging: backend and client.</span>
    *   <span>On the backend asynchronous requests for the link results from different APIs are merged before they are returned to the client.</span>
    *   <span>On the client async JavaScript requests to modularized components on potentially different machines are made. Including requests for advertisements, search results, search suggestions. They are kept separate because they have different latencies.</span>
    *   <span>Each of these requests are cached independently. Everything that can be cached is cached. Anything that involves external requests can be slow. Instant Answer returns very quickly if it’s from their datastores.</span>
    *   <span>Requests are made faster by pipelining and caching.</span>
    *   <span>Caching servers are per region and they try to fill them up as much as they can. Vastly improves response time.</span>
    *   <span>Target cache hit rate is 50%. Currently at 30%. Issue is the length of time results are cached. The results change so you don’t want to have a 5 day old results when there’s breaking news. They are trying to get better at</span> <span>Differential Caching</span><span>, learning when to cache something for a week versus a lot longer or a lot shorter.</span>
    *   <span>Search suggestions have better cache hit rates because they don’t happen for every query and they can be cached for much longer.</span>
    *   <span>Advertising is not cached. Providers have click fraud detection. Space is kept open for an ad so when it does become available the page display doesn’t glitch. Internal metrics about a search are cached so the page can be rendered better.</span>
*   <span>Nginx still serves everything.</span>
    *   <span>Because of privacy concerns they don’t want to make calls outside their domain so everything is routed and proxies through their Nginx servers. Biggest example is favicons for URLs. The getFavicon service is used to get favicons. They are cached for about a day so is both fast and so search results are not</span> [<span>leaked to providers</span>](http://www.gabrielweinberg.com/blog/2011/01/search-leakage-is-not-fud-google-et-al-please-fix-it.html)<span>.</span>
    *   <span>Nginx cache is limited because of a bug. Sized at about a one gig.</span>
*   [<span>DuckDuckHack</span>](http://duckduckhack.com/) <span>is a new Instant Answer platform.</span>
    *   <span>4 plugin types. The Fathead type (fat head of the query space) is the PostgreSQL store, which is essentially a keyword database.</span>
    *   <span>Matches don’t have to hit exactly. It’s a big datastore of definitions, links categories, disambiguation pages, and the many other features they have.</span>
    *   <span>Dream is to appeal to more niche audiences to better serve people who care about a particular topic. For example: lego parts. There’s a database of Lego parts, for example. Pictures of parts and part numbers can be automatically displayed from a search.</span>
    *   <span>Difficult to integrate plugins so adoption has been slower than they would like, though there has been a lot of demand. Still learning how best to make it work.</span>
    *   <span>Two levels of filtering on results. If a tight trigger is used and the probability of the plugin returning something is high, then space will be left on the page for the result and filled in whenever the result returns. If the relevance is low and results are often filtered out, then a space won’t be left because it would look like there’s a big gap on the page. There’s a lot of case by case logic involved.</span>
*   <span>Core of the engine is deciding how to route searches to the correct backend components.</span>
    *   <span>There’s an Intellectual Property Wall on this topic, but there are still many details.</span>
    *   <span>Two kinds of queries: long tail and fat tail. The fat tail queries go against PostgreSQL and the long tail queries go against Solr. For shorter queries PostgreSQL takes precedence. Long tail fills in Instance Answers where nothing else catches.</span>
    *   <span>Not  machine learning centric like other search engines. Heuristics are used  to partition the query space into different sections and they concentrate on those particular sections to figure out how best to classify them.</span>
    *   <span>Example: type in Tofu Ginger. A spice plugin pulls in results on the fly based on the fact that DDG thinks it’s a recipe search. DDG works with the plugin provider to pull out good ingredient keywords from their crawl. A very specific classifier was built for their datastore that they run on every query.</span>
    *   <span>Sometimes multiple categories will trigger and their has to be precedence rules to order the results. Can get very complicated.</span>
    *   <span>Some classifiers are much simpler. The plugin interface supports keyword triggers, which are relatively simple. The matches do not have to be exact. Matching can be on the beginning or ending of word, for example. It’s hash system. You look at all the words and match based on hash. Really fast.</span>
    *   <span>Regex plugins are slower. Try to convert to a hash or use a broader hash with regex underneath it.</span>
    *   <span>Core code runs before plugins to perform the categorization. One word queries like “tofu” rely more on the fathead. That gets run first and short circuits all the rest.</span>
    *   <span>There are so many cases that you get into edge cases very quickly.</span>
    *   <span>A query like “60 minutes” gets a disambiguation, which means it asks which "60 minutes" do you mean, this comes from PostgreSQL. Also triggered is a plugin to give you when the show actually airs.</span>
    *   <span>A meta-language exists to tune plugins. For example, to say if the data matches this plugin then the data is important enough to show even if there is another Instant Answer.</span>
    *   <span>Sponsored links are syndicated to Microsoft Yahoo! Search Alliance.</span>
    *   <span>In the page for our example query the Instant Answer box may go to the PostgreSQL and Solr database and other stores. A “60 minutes” section, which went out to a service in real-time to pull the data using a JavaScript API.</span>
    *   <span>Dream is to have 80% coverage in Instant Answers and every startup will create a plugin to improve search results with the data they are experts in. The payoff is traffic as instant answers is showed even above the ads. Not all information is showed in the box. Expect a 50% clickthrough.</span>
*   <span>Search suggestions come from a whole different homegrown component.</span>
    *   <span>Some people just use different words for things. Goal is not to rewrite the query, but give suggestions on how to do things better.</span>
    *   <span>“phone reviews” for example, will replace phone with telephone. This happens through an NLP component that tries to figure out what phone you meant and if there are any synonyms that should be used in the query.</span>
    *   <span>Would like to explore more of this query building component, but haven’t found the best UI for it yet.</span>
*   <span>Many blind users write in about usability issues. It’s easy to accidently break usability because the information is buried inside scripts. Screen readers can get confused about what to render because proper header tags are needed. It’s easy to forget putting an ALT tag in, for example.</span>
*   <span>Goal is to get to 80% search coverage by adding 1000s of sources to provide Instant Answers on everything. Wikipedia gets you to 20%. Add more long tail you might get up to 30%. Long tail means it doesn’t match anything exactly, but there’s a paragraph in Wikipedia that  will match the question exactly. It doesn’t hit a Wikipedia topic directly, but it’s in Wikipedia.</span>
    *   <span>Google acquired MetaWeb as means of filling out their knowledge graph.</span>
    *   <span>Google has more information on the right hand side of the page and shows more information. DDG pushes information to the top and has less information.</span>
    *   <span>Metric of when to show Instant Answers is that they are supposed to be significantly better than the links. Throwing lots of stuff on the right hand side that might be of interest to someone doesn’t interest DDG.</span>
*   <span>Filter Bubble. When you click on a link you are shown more links like that link. Your click and search history locks you into a filter bubble. You see more and more stuff you agree with. Search and click history is not used to target results. The filter bubble is burst. They may offer this feature in the future, but it would have to be opt-in.</span>

## <span>Development</span>

*   <span>Team is 50% remote. 10-15 people full time. 20-25 people are regular contributors. Some people are part-time doing very specific features. Works great for them.</span>
*   <span>Git for source control. Make releases with tags. A deployment system can go out to all the machines and install software. More complicate since Plugins started.</span>
*   <span>Every developer has a medium cloud instance.</span>
*   <span>Asana is used for project management.</span>
*   <span>HipChat and Yammer are used for internal communication</span>
*   <span>Front-end development uses a lot low level JavaScript. Thinking of moving from YUI to jQuery.</span>

## <span>Monetization strategy -</span> <span>[Advertising](http://help.duckduckgo.com/customer/portal/articles/216405-advertising)</span>

*   <span>Goal is to have a lack of clutter, so try to keep ads minimal. If an ad is useful then they can actually improve results.</span>
*   <span>Until ads become better targeted there won’t be more ads. This will require a better ad network backend.</span>
*   <span>Not a lot of good search ad feeds so there’s not much choice. To actually deliver ads across the search space you need a lot of advertisers. Could group together whole classes of queries, like for finance, but then the ads become less relevant.</span>
*   <span>Incentive for advertisers to run campaigns on DDG is still low given their volume relative to other search engines.</span>
*   <span>More open data than ever before, but there are still categories of data that are locked up and aren’t available via plugins: flights, movies, and sports. There are companies that have monopolies on data and that’s their business. Startups are generally more willing to share data.</span>
*   <span>No requirement by their search suppliers to use a specific ad network.</span>

## <span>Audience Questions</span>

*   <span>Latency management?</span>
    *   <span>Pipeline requests when possible.</span>
    *   <span>Limited by Amazon’s network, but the main thing was to split things up into different regions.</span>
    *   <span>Use async.</span>
    *   <span>Use a memory cache.</span>
    *   <span>Evaluating SPDY for Nginx.</span>
*   <span>Biggest scaling challenge?</span>
    *   <span>Hasn’t been that big a deal, mainly because they’ve modularized the architecture. Taking down a component and putting on its own stack is straightforward. Front-end requires are done separately. Just change a hostname and point somewhere else.</span>
    *   <span>Reduce complexity in the necessary replication of datasets and try to keep it read-only as much as possible.</span>
*   <span>What happens if Yahoo and Bing shut you down?</span>
    *   <span>There are other providers so there’s no immediate issue.</span>
    *   <span>In general, the risk has decreased over time because DDG is an asset to these companies because DDG is taking share from Google. And DDG is using their ad network.</span>
    *   <span>Not perceived as threat, so it’s not a likely outcome.</span>
*   <span>Will you make enough money to survive?</span>
    *   <span>Don’t worry!</span>
    *   <span>Search ads are lucrative. And they hope by making them less crappy they’ll be more lucrative.</span>
    *   <span>Their approach has not been that costly. They don’t need to make a gazillion dollars to stay in business.</span>
*   <span>How do you plan on differentiating yourself from Google in the long run?</span>
    *   <span>Concentrate on features Google can’t copy easily, not for technical reasons, but for business and cultural reasons.</span>
        *   <span>Long tail answer feature.</span>
        *   <span>Real privacy.</span>
        *   <span>Lack of clutter - link results are not cluttered with insertions from other properties, so they’ve kept it clean.</span>
        *   <span>More aggressively remove spam. Google doesn’t do that well.</span>
    *   <span>Instant Answer is great on Mobile. Building new mobile apps, but mobile is more challenging in terms of distribution. By distribution what is meant is that phones have built-in search providers, which are the path to least resistance to use. Even with a really nice search app it’s hard to get people to use them. Siri is another example of making it hard for your search to be used.</span>

## <span>Lessons Learned</span> 

*   <span>**Keep it simple**</span><span>. They don’t have tons of load balancers and lots of subsystems. Modularize components so they are independent.</span>
*   <span>**Users care about performance**</span><span>. Moving closer to users by replicating to multiple regions and including a caching layer greatly improved performance.</span>
*   <span>**Caching is just the beginning of the story**</span><span>. Once you cache you have to figure out how best to structure the cache and when to age out data. Keep data in too long and the users get bad results. So data must be carefully categorized to specific caching policies.</span>
*   <span>**Read-mostly architectures are nice**</span><span>. A lot of the robustness of DDG’s approach is because they have a largely read-only dataset.</span>
*   [<span>**Crowd sourcing plugins**</span>](http://idealab.talkingpointsmemo.com/2012/05/duckduckgo-wants-developers-to-hack-its-search-results.php) **to get greater search coverage is a great idea**. It will be quite the challenge to integrate so many data source in real-time, but the vision of having a search engine that properly handle so many different types of data is wonderful. Let’s hope the world doesn’t ruin the plan.
*   <span>**Using so many third party services is a strength and weakness**</span><span>. Strength because these alliances give DDG access to more data than they ever could before. It’s like a weaker country allying with other weaker countries to stand together against a bigger country. The downside is that make designing the system harder because the sources of information have higher latencies and must be merged together to make everything look like a unified whole.</span>
*   <span>**Heuristics work**</span><span>. You end up up with a complex rule based architecture, but a heuristic approach can let you fine tune the relationship between all the different search result sources. The trick is taking a query and mapping it to all the plugins correctly. Done well that’s a very valuable solution when compared to a general machine learning approach that when it hits it’s great, but when it misses you are stuck.</span>
*   <span>**You need an angle when competing with giants**</span><span>. DDG is pursuing features they don’t think Google will want to copy and they are keeping their costs low so they can afford to keep running on more reasonable revenues.</span>
*   <span>**Mobile is a challenge and opportunity**</span><span>. Search engine lock-in on mobile devices makes it hard to compete. The irony is Instant Answers is a great mobile feature because you don’t need to page through a lot of text to get to what you want.</span>

<span>Thanks very much to Gabriel Weinberg for taking the time to do the interview. He was very patient and open with his answers. We went a little long, but I think the results were worth it. </span>DuckDuckGo is looking for mobile developers if you are interested.

## <span>Related Articles</span>

*   [On Hacker News](http://news.ycombinator.com/item?id=5129530)
*   <span>[Duck Duck Go on Twitter](https://twitter.com/duckduckgo)</span>
*   [<span>2009 Duck Duck Go Architecture</span>](http://www.gabrielweinberg.com/blog/2009/03/duck-duck-go-architecture.html) <span>(</span>[<span>Hacker News</span>](http://news.ycombinator.com/item?id=525048)<span>)</span>
*   [<span>2011 Architecture Update</span>](http://help.duckduckgo.com/customer/portal/articles/216392-architecture)
*   [<span>DuckDuckGo used to run out of my basement</span>](http://www.gabrielweinberg.com/blog/2011/12/duckduckgo-used-to-run-out-of-my-basement.html)
*   [<span>Duck Duck Go written in Perl</span>](http://news.ycombinator.com/item?id=1500487)<span></span>
*   [<span>Replicating PostgreSQL with Bucardo</span>](http://www.gabrielweinberg.com/blog/2011/05/replicating-postgresql-with-bucardo.html)
*   [<span>PostgreSQL tips and tricks</span>](http://www.gabrielweinberg.com/blog/2011/05/postgresql.html)
*   [<span>nginx JSON hacks</span>](http://www.gabrielweinberg.com/blog/2011/07/nginx-json-hacks.html)
*   [<span>I am the founder of a search engine (Duck Duck Go) that I run by myself, AMA</span>](http://www.reddit.com/r/IAmA/comments/bbqw7/i_am_the_founder_of_a_search_engine_duck_duck_go/)
*   [<span>DuckDuckGo is blowing up</span>](http://news.ycombinator.com/item?id=3770958) <span>- Hacker News discussion on DDG traffic increases</span>
*   [<span>Gabriel Weinberg’s Blog</span>](http://www.gabrielweinberg.com/blog/) <span>- often quite interesting things to say on startups and other topics</span>
*   <span>Tech Spot</span> [<span>Interview with DuckDuckGo founder Gabriel Weinberg</span>](http://www.techspot.com/article/559-gabriel-weinberg-interview/)<span></span>
*   [<span>DuckDuckGo Founder Gabriel Weinberg Talks About Creating a More Private Search Engine</span>](http://techland.time.com/2012/03/23/duckduckgo-founder-gabriel-weinberg-talks-about-creating-a-more-private-search-engine/)
*   [<span>Ducking Google in search engines</span>](http://articles.washingtonpost.com/2012-11-09/business/35505935_1_duckduckgo-search-engine-search-results)
*   [<span>A Track Free Alternative to Google with No Targeted Ads</span>](http://www.heavychef.com/search-engines-a-track-free-alternative-to-google-with-no-targeted-ads/)
*   [<span>Duck Duck Go Open Source</span>](https://github.com/duckduckgo/duckduckgo/wiki) <span>- DDG is closed source but has some open source components</span>
*   <span>[DataSift Architecture: Realtime Datamining At 120,000 Tweets Per Second](http://highscalability.com/blog/2011/11/29/datasift-architecture-realtime-datamining-at-120000-tweets-p.html)</span>
*   [Google and the future of search: Amit Singhal and the Knowledge Graph](http://m.guardiannews.com/technology/2013/jan/19/google-search-knowledge-graph-singhal-interview)
*   <div id="_mcePaste">[Can DuckDuckGo Challenge the Reigning Champions of Search?](http://www.business2community.com/seo/can-duckduckgo-challenge-the-reigning-champions-of-search-0382883)</div>

</div>