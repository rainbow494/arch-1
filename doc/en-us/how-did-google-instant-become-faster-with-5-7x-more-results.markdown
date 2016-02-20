## [How did Google Instant become Faster with 5-7X More Results Pages?](/blog/2010/9/9/how-did-google-instant-become-faster-with-5-7x-more-results.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, September 9, 2010 at 1:53PM</span></div>

<div class="body">

![](http://www.google.com/images/logos/ps_logo2.png)

We don't have a lot of details on how Google pulled off their technically very impressive Google Instant release, but in [Google Instant behind the scenes](http://googleblog.blogspot.com/2010/09/google-instant-behind-scenes.html), they did share some interesting facts:

*   Google was serving more than a billion searches per day.
*   With Google Instant they served 5-7X more results pages than previously.
*   Typical search results were returned in less than a quarter of second.
*   A team of 50+ worked on the project for an extended period of time.

Although [Google](http://highscalability.com/display/Search?searchQuery=google) is associated with muscular data centers, they just didn't throw more server capacity at the problem, they worked smarter too. What were their general strategies?

*   Increase backend server capacity.
*   Add new caches to handle high request rates while keeping results fresh while the web is continuously crawled and re-indexed.
*   Add User-state data to the back-ends to keep track of the results pages already shown to a given user, preventing the same results from being re-fetched repeatedly.
*   Optimize page-rendering JavaScript code to help ensure web browsers could keep up with the rest of the system.

We see here the merging of various caching strategies with the real-time web. A major evolution from the early days of long web crawling cycles and disk dominated storage. The world is faster now and fast requires memory and events working in perfect harmony. Hopefully over time we'll learn more of the nitty gritty details. It should be fascinating. 

## Related Articles

*   [Google search index splits with MapReduce](http://www.theregister.co.uk/2010/09/09/google_caffeine_explained/). _With Caffeine, Google can update its index by making direct changes to the web map already stored in BigTable_.
*   [Our new search index: Caffeine](http://googleblog.blogspot.com/2010/06/our-new-search-index-caffeine.html). _Caffeine lets us index web pages on an enormous scale. In fact, every second Caffeine processes hundreds of thousands of pages in parallel. If this were a pile of paper it would grow three miles taller every second. Caffeine takes up nearly 100 million gigabytes of storage in one database and adds new information at a rate of hundreds of thousands of gigabytes per day_. _You would need 625,000 of the largest iPods to store that much information; if these were stacked end-to-end they would go for more than 40 miles._

</div>