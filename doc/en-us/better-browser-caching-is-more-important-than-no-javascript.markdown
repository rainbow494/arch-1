## [Better Browser Caching is More Important than No Javascript or Fast Networks for HTTP Performance](/blog/2013/1/30/better-browser-caching-is-more-important-than-no-javascript.html)

    

    

<iframe align="right" width="300" height="169" src="http://www.youtube.com/embed/HKNZ-tQQnSY" frameborder="0" allowfullscreen=""></iframe>

Performance guru [Steve Souders](https://twitter.com/souders) gave his keynote presentation, [Cache is King!](http://www.youtube.com/watch?v=HKNZ-tQQnSY) ([slides](http://www.slideshare.net/souders/cache-is-king)), at the HTML5DevCon, besides being an extremely clear explanation of how caching works on the Internet and how to optimize your use of HTTP to get the best performance, Steve ran experiments that found some surprising results on what gave the best web site performance improvements.

In his base line test, page loads took 7.65 seconds (median of three runs). What change--Fast Network, No Javascript, or Primed Cache--would make the biggest performance improvement? It was Primed Cache.

*   Fast Network - Using a fast FIOS network the load time was 4.13 seconds. Steve was surprised how big a difference this made, given how much work must happen in the browser. 
*   No JavaScript - 4.74 seconds after disabling JavaScript. Both reduces transfers and skips parsing by the browser. Steve thought the effect would have been larger.
*   **Primed Cache** - 3.46 seconds using a warm cache, less than half than the empty cache page view time because it reduced the number of HTTP requests and reduced the total transfer times. Key for mobile where higher latencies are common.

The implication being that caching is important so you must understand how HTTP caching works and how to make the best use of it. That's the rest of the talk.

### Some key takeaways: 

*   **Use HTTP cache control mechanisms**: max-age, etag, last-modified, if-modified-since, if-none-match, no-cache, must-revalidate, no-store. Want to prevent HTTP sending conditional GET requests, especially over high latency mobile networks. Use a long max-age and change resource names any time the content changes so that it won't be cached improperly.
*   **Be explicit**. Don't rely on heuristic caching. Decide on your caching policy for each resource. Don't leave it up to the whims of caching intermediaries or proxys to determine your cache policy. Be decisive. If something does not change very much or you can change the file name when it changes then make it cacheable for a year. If something shouldn't be cached the say no-cache. Always specify Cache-Control, no-cache, or max-age.
*   **The cache header crisis is not improving**. In practice the top 1000 sites are better configured than the long tale of websites, some are quite good, many are horrible, and it looks like there's a lot of room for improvement. 
*   **Approximately 70% of users do not have a full cache.** Caches fill in 4 hours of browsing. Mobile devices have smaller caches. Since** users anchor on a negative experience** empty caches make your users unhappy.
*   **Gather more cache stats for your users using** the [transparent pixel technique](http://www.stevesouders.com/blog/2010/03/23/p3pc-quantcast/). Monitor, feedback, and control. 

### Some room for improvement:

*   [Application Cache](https://developer.mozilla.org/en-US/docs/HTML/Using_the_application_cache) - an HTML5 mechanism for smarter caching and minimizing network traffic, seems to have a lot of developer difficulties
*   Local Storage - used to store JavaScript and CSS locally.
*   Smarter browsers. Bigger caches. Smarter purging. Prioritizing web sites.
*   DNS Prefetch and TCP Preconnect. See [The Clever Ways Chrome Hides Latency By Anticipating Your Every Need](http://highscalability.com/blog/2012/6/18/the-clever-ways-chrome-hides-latency-by-anticipating-your-ev.html).

## Related Articles 

*   [stevesouders.com](http://stevesouders.com/) - Steve's home page
*   [httparchive.org](http://httparchive.org/) - tracks how the Web is built
*   [www.webpagetest.org](http://www.webpagetest.org/) - quick and easy performance testing tool 

    