## [Strategy: Three Techniques to Survive Traffic Surges by Quickly Scaling Your Site](/blog/2014/3/19/strategy-three-techniques-to-survive-traffic-surges-by-quick.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, March 19, 2014 at 8:56AM</span></div>

<div class="body">

![](http://farm4.staticflickr.com/3755/13254531933_f62ee7387c_m.jpg)

[Matthew Might](https://twitter.com/mattmight), as a first responder to a surprise [traffic surge](http://matt.might.net/articles/how-to-emergency-web-scaling/) on his inexpensive linode hosted blog, took emergency steps that you might find useful in a similar situation:

1.  **Find the bottleneck.** Reloading the page in firebug showed the first page took 24 seconds to load and after that everything else loaded quickly. In retrospect this burst meant the site was thread limited as the CPU was idle.
2.  **Cut image sizes in half** with a shell script using ImageMagick's convert. Load time is now 12 seconds.
3.  **Turn dynamic content into static content** using a static index.html file  copied using the browser's "view source" feature. Load time is now 6 seconds.
4.  **Added threads** to the Apache configuration file. Load time is now 2 seconds. Crises averted.

Because of this quick thinking and quick action the patient survived to serve pages another day.

And in fine post-mortem tradition some of the future changes are: 

1.  **Run a cron job to trigger an earlier alert**. Email when requests exceed a per second threshold as calculated from a httpd log file. Google analytics did not alert on the problem because the pages didn't get to fully load and thus didn't generate statistics. A linode iPhone app also enables checking server load and bandwidth utilization in real time.
2.  **Use AWS as a temporary mirror** in times of stress. Given there's no charge until needed it's free insurance. No details on how this is done.
3.  **Use lighttpd, a high-performance event-based httpd server, for static content**. Serving files is a high-IO, low-CPU activity, so event-based httpd servers aren't crippled artificially by having too few threads.

It's a well written article and worth a read. 

Some other ideas from comments made on the post: 

*   [jasonkester](https://news.ycombinator.com/item?id=2382653): Never ever scale your blog. It sounds like the author was making the same mistake that pretty much everybody makes: Treating your blog as though it were dynamic content. But it's not. It's static HTML, and you should never have to make any modifications to anything to make it scale.
    *   Have your blog export all entries to plain HTML.
    *   Move your imagery out to S3/Cloudfront.
*   Outsource comments to Disqus (or others).
*   Use a blog service that may or may not scale.
*   Ditch Apache on a VPS, switch to Nginx (or others).
*   Use mod_rewrite to redirect people to a static version of a page. 

## Related Articles

<div>

*   [Surviving a traffic surge: Three techniques to scale your site fast](http://matt.might.net/articles/how-to-emergency-web-scaling/) by Matthew Might
*   [On Reddit](http://www.reddit.com/r/programming/comments/20288i/surviving_a_traffic_surge_three_techniques_to/) / [On Hacker News](https://news.ycombinator.com/item?id=2382185)
*   [Bulletproof your blog: a guide for surviving traffic spikes](http://www.maxmasnick.com/articles/bulletproof_your_blog/)
*   [Running Apache On A Memory-Constrained VPS](http://www.kalzumeus.com/2010/06/19/running-apache-on-a-memory-constrained-vps/)

</div>

</div>