## [Strategy: Google Sends Canary Requests into the Data Mine](/blog/2010/11/22/strategy-google-sends-canary-requests-into-the-data-mine.html)

    

    

![](http://farm5.static.flickr.com/4121/4811648148_ec2174d906_o.jpg)

Google runs queries against thousands of in-memory index nodes in parallel and then merges the results. One of the interesting problems with this approach, explains Google's Jeff Dean in this [lecture at Stanford](http://stanford-online.stanford.edu/courses/ee380/101110-ee380-300.asx), is the **Query of Death**.

A query can cause a program to fail because of bugs or various other issues. This means that a single query can take down an entire cluster of machines, which is not good for availability and response times, as it takes quite a while for thousands of machines to recover. Thus the Query of Death. New queries are always coming into the system and when you are always rolling out new software, it's impossible to completely get rid of the problem.

Two solutions:

*   **Test against logs**. Google replays a month's worth of logs to see if any of those queries kill anything. That helps, but Queries of Death may still happen.
*   **Send a canary request**. A request is sent to one machine. If the request succeeds then it will probably succeed on all machines, so go ahead with the query. If the request fails the only one machine is down, no big deal. Now try the request again on another machine to verify that it really is a query of death. If the request fails a certain number of times then the request if rejected and logged for further debugging.

The result is only a few servers are crashed instead of 1000s. This is a pretty clever technique, especially given the combined trends of scale-out and continuous deployment. It could also be a useful strategy for others. 

    