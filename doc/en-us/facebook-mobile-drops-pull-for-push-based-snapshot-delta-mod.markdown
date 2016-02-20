## [Facebook Mobile Drops Pull For Push-based Snapshot + Delta Model](/blog/2014/10/20/facebook-mobile-drops-pull-for-push-based-snapshot-delta-mod.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, October 20, 2014 at 8:56AM</span></div>

<div class="body">

![](https://farm4.staticflickr.com/3954/15340556948_f2df0cf4b7_m.jpg)

We've learned mobile is different. In [If You're Programming A Cell Phone Like A Server You're Doing It Wrong](http://highscalability.com/blog/2013/9/18/if-youre-programming-a-cell-phone-like-a-server-youre-doing.html) we learned programming for a mobile platform is its own specialty. In [How Facebook Makes Mobile Work At Scale For All Phones, On All Screens, On All Networks](http://highscalability.com/blog/2014/9/22/how-facebook-makes-mobile-work-at-scale-for-all-phones-on-al.html) we learned bandwidth on mobile networks is a precious resource. 

Given all that, how do you design a protocol to sync state (think messages, comments, etc.) between mobile nodes and the global state holding servers located in a datacenter?

Facebook recently wrote about their new solution to this problem in [Building Mobile-First Infrastructure for Messenger](https://code.facebook.com/posts/820258981365363/building-mobile-first-infrastructure-for-messenger/). They were able to reduce bandwidth usage by 40% and reduced by 20% the terror of hitting send on a phone.

That's a big win...that came from a protocol change.

**Facebook Messanger went from a traditional notification triggered full state pull:**

> The original protocol for getting data down to Messenger apps was pull-based. When receiving a message, the app first received a lightweight push notification indicating new data was available. This triggered the app to send the server a complicated HTTPS query and receive a very large JSON response with the updated conversation view.

**To a more sophisticated push-based snapshot + delta model:**

> Instead of this model, we decided to move to a push-based snapshot + delta model. In this model, the client retrieves an initial snapshot of their messages (typically the only HTTPS pull ever made) and then subscribes to delta updates, which are immediately pushed to the app through MQTT (a low-power, low-bandwidth protocol) as messages are received. When the client is pushed an update, it simply applies them to its local copy of the snapshot. As a result, without ever making an HTTPS request, the app can quickly display an up-to-date view.
> 
> We further optimized this flow by moving away from JSON encoding for the messages and delta updates. JSON is great if you need a flexible, human-readable format for transferring data without a lot of developer overhead. However, JSON is not very efficient on the wire. Compression helps some, but it doesn’t entirely compensate for the inherent inefficiencies of JSON’s wire format. We evaluated several possibilities for replacing JSON and ultimately decided to use Thrift. Switching to Thrift from JSON allowed us to reduce our payload size on the wire by roughly 50%.

**On the server side Facebook also innovated:** 

> Iris is a totally ordered queue of messaging updates (new messages, state change for messages read, etc.) with separate pointers into the queue indicating the last update sent to your Messenger app and the traditional storage tier. When successfully sending a message to disk or to your phone, the corresponding pointer is advanced. When your phone is offline, or there is a disk outage, the pointer stays in place while new messages can still be enqueued and other pointers advanced. As a result, long disk write latencies don't hinder Messenger's real-time communication, and we can keep Messenger and the traditional storage tier in sync at independent rates. Effectively, this queue allows a tiered storage model based on recency.

Delta based syncing strategies are very common in Network Management systems where devices keep a north bound management system in sync by sending deltas. The problem with a delta based approach is it's easy for clients to get out of sync. What if changes happen at a rate faster than than deltas can be moved through the system? Or queues get full? Or the network drops updates? The client will get out of sync and it won't even know it. Sequence numbers can be used to trigger a full sync pull if there's an out of sync situation.

It's also good to see Facebook got rid of JSON. The amount of energy and bandwidth wasted on moving and parsing fat JSON packets is almost criminal.

</div>