## [DuckDuckGo Architecture - 1 Million Deep Searches a Day and Growing](/blog/2013/1/28/duckduckgo-architecture-1-million-deep-searches-a-day-and-gr.html)

    

    

![](http://farm9.staticflickr.com/8227/8424385194_15feca732d_o.png)_This is an interview with [Gabriel Weinberg](https://twitter.com/yegg), founder of [Duck Duck Go](https://duckduckgo.com/) and general [all around startup guru](http://www.gabrielweinberg.com/blog/), on what DDG’s architecture looks like in 2012._

Innovative search engine upstart DuckDuckGo had [    30 million searches    ](https://duckduckgo.com/traffic.html) in February 2012 and averages over 1 million searches a day. It’s being positioned by [    super investor Fred Wilson    ](http://www.avc.com/a_vc/2012/02/duck-duck-go-passed-1mm-searches-per-day.html) as a clean, private, impartial and fast search engine. After talking with Gabriel I like what Fred Wilson said earlier, it seems closer to the heart of the matter: [    We invested in DuckDuckGo for the Reddit, Hacker News anarchists    ](http://venturebeat.com/2012/05/21/fred-wilson-duckduckgo-reddit-hacker-news/).  

Choosing DuckDuckGo can be thought of as not just a technical choice, but a vote for revolution. In an age when knowing your essence is not about about love or friendship, but about more effectively selling you to advertisers, DDG is positioning themselves as the [    do not track alternative    ](http://whatisdnt.com/), keepers of the [privacy flame](https://duckduckgo.com/privacy). You will still be monetized of course, but in a more civilized and anonymous way.   

Pushing privacy is a good way to carve out a competitive niche against Google et al, as by definition they can never compete on privacy. I get that. But what I found most compelling is DDG’s strong vision of a crowdsourced network of plugins giving broader search coverage by tying an army of vertical data suppliers into their search framework. For example, there's a specialized Lego plugin for searching against a complete Lego database. Use the name of a spice in your search query, for example, and DDG will recognize it and may trigger a deeper search against a highly tuned recipe database. Many different plugins can be triggered on each search and it’s all handled in real-time.  

Can’t searching the Open Web provide all this data? No really. This is structured data with semantics. Not an HTML page. You need a search engine that’s capable of categorizing, mapping, merging, filtering, prioritizing, searching, formatting, and disambiguating richer data sets and you can’t do that with a keyword search. You need the kind of smarts DDG has built into their search engine. One problem of course is now that data has become valuable many grown ups don’t want to share anymore.  

Being ad supported puts DDG in a tricky position. Targeted ads are more lucrative, but ironically DDG’s do not track policies means they can’t gather targeting data. Yet that’s also a selling point for those interested in privacy. But as search is famously intent driven, DDG’s technology of categorizing queries and matching them against data sources is already a form of high value targeting.  

It will be fascinating to see how these forces play out. But for now let’s see how DuckDuckGo implements their search engine magic...

##     Information Sources    

*       Personal interview with Gabriel Weinberg.    
*       Resources listed under         **Related Articles**        .    

## Stats

*   30 million searches in February 2012
*   Averages over 1 million searches a day
*   12M API requests per day

##     The Platform    

*       EC2    
*       Ubuntu    
*       Perl & CPAN    
*   [    Server Density    ](http://www.serverdensity.com/)     - monitoring    
*       Solr    
*       PostgreSQL    
*       Memcached    
*   [    Bucardo    ](http://bucardo.org/)     - asynchronous PostgreSQL replication system    
*   [    Global Traffic Director    ](http://www.dnsmadeeasy.com/services/global-traffic-director/)     - load balancing between regions    
*       Nginx    
*   [    getFavicon    ](http://getfavicon.appspot.com/)     - serves favicons    
*   [    daemontools    ](http://en.wikipedia.org/wiki/Daemontools)     - free tools for managing Unix services    
*       Git    
*   [    Asana    ](http://asana.com/)     - project management.    
*       HipChat - internal communication    
*       Yammer    
*       JavaScript    
*       YUI (moving to jQuery)    

##     What's Inside    

*       What Gabriel likes about their system:    
    *       Very simple, though they have become more complex over time. Complexity is  mitigated by everything being modular. Daemontools is used to keep services running, everything runs through Nginx. There are lots of different services, but the architecture is such that they all stay up.    
    *       Main goals have been 100% uptime and speed. Reducing complexity helps with both goals.    

##     Out of the Basement and into AWS (mostly)    

*       DuckDuckGo used to run out of Gabriel’s basement. Most components, including all front-end components, are now on AWS.    
    *   Some “persistent machines” are still in the basement as there has been no compelling reason to move these components, like the Git repository, crawling, and the real-time Wikipedia index that updates [zero-click connects](http://dhruvbird.com/ddb/zc.html).
    *       Linode is used to host some community functionality, like translations.    
    *       Moved to Ubuntu from FreeBSD as part of the move to AWS.    
*       Moved to multiple regions for better front-end performance. Make DDG fast everywhere and the users will come.    
    *       One complaint users had is speed. By running in 4 AWS datacenters (California, Virginia, Singapore, Ireland) DDG is able to be closer to its users around the world.    
    *       Identical software runs in all datacenters.    
    *   Global Traffic Director is used for their DNS and to load balance users across regions. DDG would like to use more regions (South America and another Asian datacenter) but the Traffic Director currently only works in four regions.
*       Got off EBS completely due to EBS being involved in most of the large failures. Also experienced performance variability with EBS.    
    *       The new Provisioned IOPs is supposed to handle that, but having fewer extra moving pieces in the architecture is better.    
    *       May experiment with PIOPs in the future, though the current non-EBS architecture is working well.    
*       Run mainly on extra large machines as they seem to be the sweet spot for high IO for network and they have 4 ephemeral disks.    
    *       Attracted by benchmarks showing the extra large machines have the lowest performance variance.    
    *       Found 4 ephemeral disks performed better than striping 8 EBS disks.    
    *       Not interested in high IO instance types because the goal is to keep as much data as in memory as possible. High transaction rates are not a problem and data is cached as fast as possible. Plus machines don’t show high IO utilization rates.    
*       Medium instances are used for developer machines. Everyone has their own medium dev instance. Staging instances are used for testing, which are sufficient to mimic the live environment.    
*       Datacenter syncing.    
    *       Primarily deal with read-only data. Updates are pushed out all the time, but they don’t have to be immediate. Means syncing between datacenters is not a datacenter problem. There aren’t really any consistency issues.    
    *       Backend systems (non-AWS) have a master databases that pushout updates to the regions.    
*       Distributed caching system uses memcached.    
    *       High memory m2.xlarge are used for caching.    
    *       Approximately 100GB in size, sharded across m2.xlarge machines.    
    *       When something is cached it pushes out to all the other caching systems. There’s no master.    
    *       Custom Perl solution.    
    *       Caching is routed through Nginx so requests bypass the Perl backend completely if the data is in cache.    
    *       Not constrained by size, they can add as many cache machines as needed, the challenge is to figure out what to put in the cache so it will be useful and not to give bad results.    
    *       Working on smarter cache aging algorithms. Look at the organic links that are coming back. You can tell alot about about a link from their associated snippets and domain. The problem is this signal is only available after the results come back, so it’s slow for the UI, but it’s great for caching.    
    *       Priority has been to get syncing correct in all the regions. Now they’ll be working on making smarter cache.    
*       Many databases are used, including PostgreSQL, Solr, Berkeley, and flat files.    
    *       Bucardo is used for PostgreSQL replication.    
    *       Solr syncing is via their own process, not the built in replication.    
    *       PostgreSQL masters are in the basement and the slaves are in each region.    
    *       PostgreSQL holds instant answer and entity data. If you type in “duckduckgo”, for example, you get something from Wikipedia and that comes from PostgreSQL.    
    *       The PostgreSQL database has something like a 100 sources of data. These are crawled by the backend and stored in the database.    

##     Searching - It's Complicated    

*       High level: try to figure what a query is about  and then route it to the appropriate datastores and APIs that are appropriate for it. And in some cases bring it back, parallelize it, and mix it together.    
*       The zero-click, instance answer box, could be powered by PostgreSQL, Solr, on the fly APIs, flat files, or a combination.    
*       Link results are API driven, though top links may come from other sources.      [    Link sources include    ](http://help.duckduckgo.com/customer/portal/articles/216399-sources)    : Bing, Yahoo, Yandex, Blekko, WolframAlpha, and many others.    
*       All parsing and merging logic is in Perl.    
*       Two levels of merging: backend and client.    
    *       On the backend asynchronous requests for the link results from different APIs are merged before they are returned to the client.    
    *       On the client async JavaScript requests to modularized components on potentially different machines are made. Including requests for advertisements, search results, search suggestions. They are kept separate because they have different latencies.    
    *       Each of these requests are cached independently. Everything that can be cached is cached. Anything that involves external requests can be slow. Instant Answer returns very quickly if it’s from their datastores.    
    *       Requests are made faster by pipelining and caching.    
    *       Caching servers are per region and they try to fill them up as much as they can. Vastly improves response time.    
    *       Target cache hit rate is 50%. Currently at 30%. Issue is the length of time results are cached. The results change so you don’t want to have a 5 day old results when there’s breaking news. They are trying to get better at         Differential Caching        , learning when to cache something for a week versus a lot longer or a lot shorter.    
    *       Search suggestions have better cache hit rates because they don’t happen for every query and they can be cached for much longer.    
    *       Advertising is not cached. Providers have click fraud detection. Space is kept open for an ad so when it does become available the page display doesn’t glitch. Internal metrics about a search are cached so the page can be rendered better.    
*       Nginx still serves everything.    
    *       Because of privacy concerns they don’t want to make calls outside their domain so everything is routed and proxies through their Nginx servers. Biggest example is favicons for URLs. The getFavicon service is used to get favicons. They are cached for about a day so is both fast and so search results are not     [    leaked to providers    ](http://www.gabrielweinberg.com/blog/2011/01/search-leakage-is-not-fud-google-et-al-please-fix-it.html)    .    
    *       Nginx cache is limited because of a bug. Sized at about a one gig.    
*   [    DuckDuckHack    ](http://duckduckhack.com/)     is a new Instant Answer platform.    
    *       4 plugin types. The Fathead type (fat head of the query space) is the PostgreSQL store, which is essentially a keyword database.    
    *       Matches don’t have to hit exactly. It’s a big datastore of definitions, links categories, disambiguation pages, and the many other features they have.    
    *       Dream is to appeal to more niche audiences to better serve people who care about a particular topic. For example: lego parts. There’s a database of Lego parts, for example. Pictures of parts and part numbers can be automatically displayed from a search.    
    *       Difficult to integrate plugins so adoption has been slower than they would like, though there has been a lot of demand. Still learning how best to make it work.    
    *       Two levels of filtering on results. If a tight trigger is used and the probability of the plugin returning something is high, then space will be left on the page for the result and filled in whenever the result returns. If the relevance is low and results are often filtered out, then a space won’t be left because it would look like there’s a big gap on the page. There’s a lot of case by case logic involved.    
*       Core of the engine is deciding how to route searches to the correct backend components.    
    *       There’s an Intellectual Property Wall on this topic, but there are still many details.    
    *       Two kinds of queries: long tail and fat tail. The fat tail queries go against PostgreSQL and the long tail queries go against Solr. For shorter queries PostgreSQL takes precedence. Long tail fills in Instance Answers where nothing else catches.    
    *       Not  machine learning centric like other search engines. Heuristics are used  to partition the query space into different sections and they concentrate on those particular sections to figure out how best to classify them.    
    *       Example: type in Tofu Ginger. A spice plugin pulls in results on the fly based on the fact that DDG thinks it’s a recipe search. DDG works with the plugin provider to pull out good ingredient keywords from their crawl. A very specific classifier was built for their datastore that they run on every query.    
    *       Sometimes multiple categories will trigger and their has to be precedence rules to order the results. Can get very complicated.    
    *       Some classifiers are much simpler. The plugin interface supports keyword triggers, which are relatively simple. The matches do not have to be exact. Matching can be on the beginning or ending of word, for example. It’s hash system. You look at all the words and match based on hash. Really fast.    
    *       Regex plugins are slower. Try to convert to a hash or use a broader hash with regex underneath it.    
    *       Core code runs before plugins to perform the categorization. One word queries like “tofu” rely more on the fathead. That gets run first and short circuits all the rest.    
    *       There are so many cases that you get into edge cases very quickly.    
    *       A query like “60 minutes” gets a disambiguation, which means it asks which "60 minutes" do you mean, this comes from PostgreSQL. Also triggered is a plugin to give you when the show actually airs.    
    *       A meta-language exists to tune plugins. For example, to say if the data matches this plugin then the data is important enough to show even if there is another Instant Answer.    
    *       Sponsored links are syndicated to Microsoft Yahoo! Search Alliance.    
    *       In the page for our example query the Instant Answer box may go to the PostgreSQL and Solr database and other stores. A “60 minutes” section, which went out to a service in real-time to pull the data using a JavaScript API.    
    *       Dream is to have 80% coverage in Instant Answers and every startup will create a plugin to improve search results with the data they are experts in. The payoff is traffic as instant answers is showed even above the ads. Not all information is showed in the box. Expect a 50% clickthrough.    
*       Search suggestions come from a whole different homegrown component.    
    *       Some people just use different words for things. Goal is not to rewrite the query, but give suggestions on how to do things better.    
    *       “phone reviews” for example, will replace phone with telephone. This happens through an NLP component that tries to figure out what phone you meant and if there are any synonyms that should be used in the query.    
    *       Would like to explore more of this query building component, but haven’t found the best UI for it yet.    
*       Many blind users write in about usability issues. It’s easy to accidently break usability because the information is buried inside scripts. Screen readers can get confused about what to render because proper header tags are needed. It’s easy to forget putting an ALT tag in, for example.    
*       Goal is to get to 80% search coverage by adding 1000s of sources to provide Instant Answers on everything. Wikipedia gets you to 20%. Add more long tail you might get up to 30%. Long tail means it doesn’t match anything exactly, but there’s a paragraph in Wikipedia that  will match the question exactly. It doesn’t hit a Wikipedia topic directly, but it’s in Wikipedia.    
    *       Google acquired MetaWeb as means of filling out their knowledge graph.    
    *       Google has more information on the right hand side of the page and shows more information. DDG pushes information to the top and has less information.    
    *       Metric of when to show Instant Answers is that they are supposed to be significantly better than the links. Throwing lots of stuff on the right hand side that might be of interest to someone doesn’t interest DDG.    
*       Filter Bubble. When you click on a link you are shown more links like that link. Your click and search history locks you into a filter bubble. You see more and more stuff you agree with. Search and click history is not used to target results. The filter bubble is burst. They may offer this feature in the future, but it would have to be opt-in.    

##     Development    

*       Team is 50% remote. 10-15 people full time. 20-25 people are regular contributors. Some people are part-time doing very specific features. Works great for them.    
*       Git for source control. Make releases with tags. A deployment system can go out to all the machines and install software. More complicate since Plugins started.    
*       Every developer has a medium cloud instance.    
*       Asana is used for project management.    
*       HipChat and Yammer are used for internal communication    
*       Front-end development uses a lot low level JavaScript. Thinking of moving from YUI to jQuery.    

##     Monetization strategy -         [Advertising](http://help.duckduckgo.com/customer/portal/articles/216405-advertising)    

*       Goal is to have a lack of clutter, so try to keep ads minimal. If an ad is useful then they can actually improve results.    
*       Until ads become better targeted there won’t be more ads. This will require a better ad network backend.    
*       Not a lot of good search ad feeds so there’s not much choice. To actually deliver ads across the search space you need a lot of advertisers. Could group together whole classes of queries, like for finance, but then the ads become less relevant.    
*       Incentive for advertisers to run campaigns on DDG is still low given their volume relative to other search engines.    
*       More open data than ever before, but there are still categories of data that are locked up and aren’t available via plugins: flights, movies, and sports. There are companies that have monopolies on data and that’s their business. Startups are generally more willing to share data.    
*       No requirement by their search suppliers to use a specific ad network.    

##     Audience Questions    

*       Latency management?    
    *       Pipeline requests when possible.    
    *       Limited by Amazon’s network, but the main thing was to split things up into different regions.    
    *       Use async.    
    *       Use a memory cache.    
    *       Evaluating SPDY for Nginx.    
*       Biggest scaling challenge?    
    *       Hasn’t been that big a deal, mainly because they’ve modularized the architecture. Taking down a component and putting on its own stack is straightforward. Front-end requires are done separately. Just change a hostname and point somewhere else.    
    *       Reduce complexity in the necessary replication of datasets and try to keep it read-only as much as possible.    
*       What happens if Yahoo and Bing shut you down?    
    *       There are other providers so there’s no immediate issue.    
    *       In general, the risk has decreased over time because DDG is an asset to these companies because DDG is taking share from Google. And DDG is using their ad network.    
    *       Not perceived as threat, so it’s not a likely outcome.    
*       Will you make enough money to survive?    
    *       Don’t worry!    
    *       Search ads are lucrative. And they hope by making them less crappy they’ll be more lucrative.    
    *       Their approach has not been that costly. They don’t need to make a gazillion dollars to stay in business.    
*       How do you plan on differentiating yourself from Google in the long run?    
    *       Concentrate on features Google can’t copy easily, not for technical reasons, but for business and cultural reasons.    
        *       Long tail answer feature.    
        *       Real privacy.    
        *       Lack of clutter - link results are not cluttered with insertions from other properties, so they’ve kept it clean.    
        *       More aggressively remove spam. Google doesn’t do that well.    
    *       Instant Answer is great on Mobile. Building new mobile apps, but mobile is more challenging in terms of distribution. By distribution what is meant is that phones have built-in search providers, which are the path to least resistance to use. Even with a really nice search app it’s hard to get people to use them. Siri is another example of making it hard for your search to be used.    

##     Lessons Learned     

*       **Keep it simple**        . They don’t have tons of load balancers and lots of subsystems. Modularize components so they are independent.    
*       **Users care about performance**        . Moving closer to users by replicating to multiple regions and including a caching layer greatly improved performance.    
*       **Caching is just the beginning of the story**        . Once you cache you have to figure out how best to structure the cache and when to age out data. Keep data in too long and the users get bad results. So data must be carefully categorized to specific caching policies.    
*       **Read-mostly architectures are nice**        . A lot of the robustness of DDG’s approach is because they have a largely read-only dataset.    
*   [    **Crowd sourcing plugins**    ](http://idealab.talkingpointsmemo.com/2012/05/duckduckgo-wants-developers-to-hack-its-search-results.php) **to get greater search coverage is a great idea**. It will be quite the challenge to integrate so many data source in real-time, but the vision of having a search engine that properly handle so many different types of data is wonderful. Let’s hope the world doesn’t ruin the plan.
*       **Using so many third party services is a strength and weakness**        . Strength because these alliances give DDG access to more data than they ever could before. It’s like a weaker country allying with other weaker countries to stand together against a bigger country. The downside is that make designing the system harder because the sources of information have higher latencies and must be merged together to make everything look like a unified whole.    
*       **Heuristics work**        . You end up up with a complex rule based architecture, but a heuristic approach can let you fine tune the relationship between all the different search result sources. The trick is taking a query and mapping it to all the plugins correctly. Done well that’s a very valuable solution when compared to a general machine learning approach that when it hits it’s great, but when it misses you are stuck.    
*       **You need an angle when competing with giants**        . DDG is pursuing features they don’t think Google will want to copy and they are keeping their costs low so they can afford to keep running on more reasonable revenues.    
*       **Mobile is a challenge and opportunity**        . Search engine lock-in on mobile devices makes it hard to compete. The irony is Instant Answers is a great mobile feature because you don’t need to page through a lot of text to get to what you want.    

    Thanks very much to Gabriel Weinberg for taking the time to do the interview. He was very patient and open with his answers. We went a little long, but I think the results were worth it.     DuckDuckGo is looking for mobile developers if you are interested.

##     Related Articles    

*   [On Hacker News](http://news.ycombinator.com/item?id=5129530)
*       [Duck Duck Go on Twitter](https://twitter.com/duckduckgo)    
*   [    2009 Duck Duck Go Architecture    ](http://www.gabrielweinberg.com/blog/2009/03/duck-duck-go-architecture.html)     (    [    Hacker News    ](http://news.ycombinator.com/item?id=525048)    )    
*   [    2011 Architecture Update    ](http://help.duckduckgo.com/customer/portal/articles/216392-architecture)
*   [    DuckDuckGo used to run out of my basement    ](http://www.gabrielweinberg.com/blog/2011/12/duckduckgo-used-to-run-out-of-my-basement.html)
*   [    Duck Duck Go written in Perl    ](http://news.ycombinator.com/item?id=1500487)        
*   [    Replicating PostgreSQL with Bucardo    ](http://www.gabrielweinberg.com/blog/2011/05/replicating-postgresql-with-bucardo.html)
*   [    PostgreSQL tips and tricks    ](http://www.gabrielweinberg.com/blog/2011/05/postgresql.html)
*   [    nginx JSON hacks    ](http://www.gabrielweinberg.com/blog/2011/07/nginx-json-hacks.html)
*   [    I am the founder of a search engine (Duck Duck Go) that I run by myself, AMA    ](http://www.reddit.com/r/IAmA/comments/bbqw7/i_am_the_founder_of_a_search_engine_duck_duck_go/)
*   [    DuckDuckGo is blowing up    ](http://news.ycombinator.com/item?id=3770958)     - Hacker News discussion on DDG traffic increases    
*   [    Gabriel Weinberg’s Blog    ](http://www.gabrielweinberg.com/blog/)     - often quite interesting things to say on startups and other topics    
*       Tech Spot     [    Interview with DuckDuckGo founder Gabriel Weinberg    ](http://www.techspot.com/article/559-gabriel-weinberg-interview/)        
*   [    DuckDuckGo Founder Gabriel Weinberg Talks About Creating a More Private Search Engine    ](http://techland.time.com/2012/03/23/duckduckgo-founder-gabriel-weinberg-talks-about-creating-a-more-private-search-engine/)
*   [    Ducking Google in search engines    ](http://articles.washingtonpost.com/2012-11-09/business/35505935_1_duckduckgo-search-engine-search-results)
*   [    A Track Free Alternative to Google with No Targeted Ads    ](http://www.heavychef.com/search-engines-a-track-free-alternative-to-google-with-no-targeted-ads/)
*   [    Duck Duck Go Open Source    ](https://github.com/duckduckgo/duckduckgo/wiki)     - DDG is closed source but has some open source components    
*       [DataSift Architecture: Realtime Datamining At 120,000 Tweets Per Second](http://highscalability.com/blog/2011/11/29/datasift-architecture-realtime-datamining-at-120000-tweets-p.html)    
*   [Google and the future of search: Amit Singhal and the Knowledge Graph](http://m.guardiannews.com/technology/2013/jan/19/google-search-knowledge-graph-singhal-interview)
*       [Can DuckDuckGo Challenge the Reigning Champions of Search?](http://www.business2community.com/seo/can-duckduckgo-challenge-the-reigning-champions-of-search-0382883)    

    