## [How Facebook Makes Mobile Work at Scale for All Phones, on All Screens, on All Networks](/blog/2014/9/22/how-facebook-makes-mobile-work-at-scale-for-all-phones-on-al.html)

    

    

![](https://farm4.staticflickr.com/3845/15115553160_e6e67e3405_n.jpg)

Update: [Instagram Improved Their App's Performance. Here's How](http://highscalability.com/blog/2014/9/29/instagram-improved-their-apps-performance-heres-how.html).

When you find your mobile application that ran fine in the US is slow in other countries, how do you fix it? That’s a problem Facebook talks about in a couple of enlightening videos from the [@scale conference](http://atscaleconference.com/). Since mobile is eating the world, this is the sort of thing you need to consider with your own apps.

    In the US we may complain about our mobile networks, but that’s more     [    #firstworldproblems    ](https://twitter.com/search?q=%23firstworldproblems&src=typd)     talk than reality. Mobile networks in other countries can be much slower and cost a lot more. This is the conclusion from Chris Marra, Project Manager at Facebook, in a really interesting talk titled     [    Developing Android Apps for Emerging Market    ](https://www.youtube.com/watch?v=GHTO2WKDO6I#t=7230)    .    

    Facebook found in the US there’s 70.6% 3G penetration with 280ms average latency. In India there’s 6.9% 3G penetration with 500ms latency. In Brazil there’s 38.6% 3G penetration with more than 850ms average latency.    

    Chris also talked about Facebook’s comprehensive research on who uses Facebook and what kind of phones they use. In summary they found         **not everyone is on a fast phone, not everyone has a large screen, and not everyone is on a fast network**        .    

    It turns out the typical phone used by Facebook users is from circa 2011, dual core, with less than 1GB of RAM. By designing for a high end phone Facebook found all their low end users, which is the typical user, had poor user experiences.    

    For the slow phone problem Facebook created a separate application that used lighter weight animations and other strategies to work on lower end phones. For the small screen problem Facebook designers made sure applications were functional at different screen sizes.    

    Facebook has moved to a product organization. A single vertical group is responsible for producing a particular product rather than having, for example, an Android team try to create all Android products. There’s also a horizontally focussed Android team trying to figure out best practices for Android, delving deep into the details of what makes a platform tick.    

    Each team is responsible for the end-to-end performance and reliability for their product. There are also core teams looking at and analyzing general performance problems and helping where needed to improve performance.    

    Both core teams and product teams are needed. The core team is really good at instrumentation and identifying problems and working with product teams to fix them. For mobile it’s important that each team owns their full product end-to-end. Owning core engagement metrics, core reliability, and core performance metrics including daily usage, cold start times, and reliability, while also knowing how to fix problems.     

To solve the slow network problem there’s a whole other talk. This time the talk is given by Andrew Rogers, Engineering Manager at Facebook, and it’s titled [    Tuning Facebook for Constrained Networks    ](https://www.youtube.com/watch?v=GHTO2WKDO6I#t=8066). Andrew talks about three methods to help deal with network problems: **Image Download Sizes, Network Quality Detection, Prefetching Content**.

Overall, please note the immense effort that is required to operate at Facebook scale. Not only do you have different phones like Android and iOS, you have different segments within each type of phone you must code and design for. This is crazy hard to do.

##     Reducing Image Sizes -  WebP saved over 30% JPEG, 80% over PNG    

*       Image data dominates the number bytes that are downloaded from Facebook applications. Accounts for 85% of total data download in Facebook for Android and 65% in Messenger.    

*       Reducing image sizes would reduce the amount of data downloaded and result in quicker downloads, especially on high latency networks (which we learned were typical).    

*   **    Request an appropriate image size for the viewport    **

    *   Resize on the server. Don’t send a large image to the client and then have the client scale the image down to a smaller size. This wastes a lot of bandwidth and takes a lot of time.

    *       Send a thumbnail (for profile pictures) and a small preview image (this is seen in the newsfeed), and then a full image (seen in photo stories) when a user asks to zoom in it. A low-res phone may never need a full image. Most of the time a thumbnail and a small preview is all you need.    

    *       Significant savings. Scaling a 960 pixel image that is 79KB is size to 240 pixels wide yields a 86% size reduction, to 480 pixels is a 58% size reduction, to 720 pixels is at 23% size reduction.    

    *   With screen sizes becoming**larger scaling images isn’t as effective** as it used to be. Still worth doing, but the payoff isn’t as much.

*       **Change the image format**    

    *       90% of images sent to Facebook and Messenger for Android use the     [    WebP format    ](http://en.wikipedia.org/wiki/WebP)    .    

    *       WebP format released by Google in 2010\.    

    *       WebP saves 7% download size over JPEG for equivalent quality.    

    *       WebP saved over 30% download size over JPEG by tuning quality and compression parameters with no noticeable quality differences. A big savings.    

    *       WebP also supports transparency and animation, which is useful for their Stickers product.    

    *       WebP saves 80% over PNG.    

    *       For older versions of Android the images are transported using WebP but transcoded on the client to JPEG for rendering on the device.    

    ##     Network Quality Detection - Adjust Behavior to Network Quality             

    *       On the same network technology 2G, 3G, LTE, WiFi, connections speeds in the         US were 2-3x faster         than in India and Brazil.    

    *       LTE is often faster than WiFi, so you can’t just base the connection speed on technology.    

    *       Built into the client the ability to:    

        *       Measure throughput on all large network transfers    

        *       Facebook servers provide a Round Trip Time (RDD) estimate in the HTTP header in every response    

        *       The client maintains a moving average of throughput and RTT times to determine network quality.    

    *       Connections are bucketized into the following groups: Poor is < 150kbps. Moderate is 150-600kbps. Good is 600-2000kbps. Excellent is > 2000kbps.    

    *       Feature developers can adjust their behavior depending on the connection quality.    

    *   **    Some possible responses based on quality        :    **

        *       Increase/decrease compression.    

        *       Issue more/fewer parallel network requests. Don’t saturate the network.    

        *       Disable/enable auto-play video. Don’t cause more traffic on slow networks.    

        *       Pre-fetch more content.    

    *       A tool developed at Facebook called         **Air Traffic Control**         supports the simulation of different traffic profiles. Each profile can be configured: bandwidth, packet loss, packet loss-correlation, delay, delay correlation, delay jitter. Extremely helpful in finding unexpected behaviour on slow networks.    

    *       There are buckets for videos, but not sure what they are.    

    ##     Prefetching Content    

    *       Issuing network requests for content ahead of when the content is actually needed on the device.    

    *       Prefetching is especially important on networks with high latency. Waiting to issue a download request for an image the user will be looking at a blank screen.    

    *       **Issue network requests for feeds early in the app startup process**         so the data will be present when the feed is displayed to the user.  The download can occur in parallel with other initialization tasks.    

    *       **Keep a priority queue of network requests**        . Don’t block foreground network requests on background network requests or the user won’t see the data they are currently interested in.    

    *       **Monitor for overfetching and excess consumption of device resources**        . Over fetching could fill up the disk or waste a lot of money on the user’s data plan.    

## Miscellaneous

*   Client uploading to servers. The idea is to send fewer bytes over the wire from the client to the server, which means resizing images on the client side before they are sent to the server. If an upload fails retry quickly. It’s usually just a problem in the pipe.

*   There are 20 different APKs (Android application package) for the Facebook app, cut by API level, screen size, and processor architecture.

## Related Articles

*   [Facebook's iOS Infrastructure - @Scale 2014 Mobile](https://www.youtube.com/watch?v=XhXC4SKOGfQ) - the above article is on the UI, this great talk is about the code on the phone behind the UI.

    