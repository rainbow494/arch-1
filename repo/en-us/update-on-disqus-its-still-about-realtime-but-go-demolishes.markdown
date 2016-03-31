## [Update on Disqus: It's Still About Realtime, But Go Demolishes Python](/blog/2014/5/7/update-on-disqus-its-still-about-realtime-but-go-demolishes.html)

    

    

[![](https://farm3.staticflickr.com/2937/14110625651_9ccec7d022_n.jpg)](https://farm3.staticflickr.com/2937/14110625651_4d66420224_o.png)Our last article on Disqus: [How Disqus Went Realtime With 165K Messages Per Second And Less Than .2 Seconds Latency](http://highscalability.com/blog/2014/4/28/how-disqus-went-realtime-with-165k-messages-per-second-and-l.html), was a little out of date, but the folks at Disqus have been busy implementing, not talking, so we don't know a lot about what they are doing now, but we do have a short update in [C1MM and NGINX](https://www.youtube.com/watch?v=yL4Q7D4ynxU) by John Watson and an article [Trying out this Go thing](http://blog.disqus.com/post/51155103801/trying-out-this-go-thing).

So Disqus has grown a bit:

*   1.3 billion unique visitors
*   10 billion page views
*   500 million users engaged in discussions
*   3 million communities
*   25 million comments

They are still all about realtime, but Go replaced Python in their Realtime system:

*   Original Realtime backend was written in a pretty lightweight Python + gevent.
*   The realtime service is a hybrid of CPU intensive tasks + lots of network IO. Gevent was handling the network IO without an issue, but at higher contention, the CPU was choking everything. Switching over to Go removed that contention, which was the primary issue that was being seen.
*   Still runs on 5 machines Nginx machines. 
    *   Uses NginxPushStream, which supprts EventSource, WebSocket, Long Polling, and Forever Iframe.
    *   All users are connected to these machines.
    *   On a normal day each machine sees 3200 connections/s, 1 million connections, 150K packets/s TX and 130K packets/s RX, 150 mbits/s TX and 80 mbits/s RC, with <15ms delay end-to-end (which is faster than Javascript can render a comment)
    *   Had many issues with resource exhaustion at first. The configuration for Nginx and the OS are given that help alleviate the problems, tuning them to handle a scenario with many connections moving little data.
*   Ran out of network bandwidth before anything else. 
    *   Using 10 gigabit network interface cards helped a lot. 
    *   Enabling gzip helped a lot, but Nginx preallocates a lot of memory per connection for gzip, but since comments are small this was overkill. Ruducing Nginx buffer sizes reduced out of memory problems.
*   As message rates increased, at peak processing 10k+ messages per second, the machines maxed out, and end-to-end latency went to seconds and minutes in the worst case.
*   Switched to Go. 
    *   Liked Go because of its performance, native concurrency, and familiarity for Python programmers.
    *   In only a week a replacement system was built with impressive results:
        *   End-to-end latency is on average, less than 10ms.
        *   Currently consuming roughly 10-20% of available CPU. A huge reduction.
    *   Node was not selected because it does not handle CPU intensive tasks well
    *   Go does not directly access the database. It consumes a queue from RabbitMQ and publishes to the Nginx frontends.
    *   A Go framework is not being used. This is a tiny component and the rest of Disqus is still Django.
*   They wanted to use resources better, not add more machines:
    *   For the amount of work that was being done, we didn't want to horizontally scale more. Throwing more and more hardware at a problem isn't always the best solution. In the end, having a faster product yields its own benefits as well.

## Related Articles

*   [On Hacker News](https://news.ycombinator.com/item?id=7711110)  / [On Reddit](http://www.reddit.com/r/Python/comments/2504ni/update_on_disqus_its_still_about_realtime_but_go/)

    