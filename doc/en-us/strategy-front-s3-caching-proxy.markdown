## [Strategy: Front S3 with a Caching Proxy](/blog/2008/7/20/strategy-front-s3-with-a-caching-proxy.html)

    

    

Given S3's [recent failure](http://developer.amazonwebservices.com/connect/thread.jspa?threadID=19714&tstart=0) ([Cloud Status](http://www.cloudstatus.com/) tells the tale) [Kevin Burton](http://feedblog.org/2008/07/20/twitter-screws-up-again/) makes the [excellent suggestion of fronting S3](http://feedblog.org/2008/07/20/twitter-screws-up-again/) with a [caching proxy server](http://en.wikipedia.org/wiki/Proxy_server).  

A caching proxy server _can reply to service requests without contacting the specified server, by retrieving content saved from a previous request, made by the same client or even other clients. This is called caching. Caching proxies keep local copies of frequently requested resources._ In normal operation when an asset (a user's avatar, for example) is requested the cache is tried first. If the asset is found in the cache then it's returned. If the asset is not in the cache it's retrieved from S3 (or wherever) and cached. So when S3 goes down it's likely you can ride out the down time by serving assets out of the cache.  

This strategy only works when using S3 as a [CDN](http://highscalability.com/tags/cdn). If you are using S3 for its "real" purpose, as a storage service, then a caching proxy can't help you...

Amazon doesn't used S3 as a CDN either [Amazon Not Building Out AWS To Compete With CDNs](http://blog.streamingmedia.com/the_business_of_online_vi/2008/07/amazon-not-buil.html). They use Limelight Networks.  

Some proxy options are: [Squid](http://www.squid-cache.org/), [Nginx](http://nginx.net/), [Varnish](http://varnish.projects.linpro.no/).  

Planaroo shares [how a small startup responds to an S3 outage](http://webspeed.typepad.com/planaroo/2008/07/rethinking-what.html) (summarized):

*   **Up-to-date backups are a good thing.** Keep current backups such that you can switch to a new URL for your assets. Easier said than done I think.*   **Switch it, don't fix it.** Switch to your backup rather than wait for the system to come up quickly, because it may not.*   **Serve CSS, JavaScript, icons, and Google AJAX libraries** from alternate sources. Don't rely S3 or Google to always be able to server your crown jewels.    