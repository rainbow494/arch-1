## [Uber Goes Unconventional: Using Driver Phones as a Backup Datacenter](/blog/2015/9/21/uber-goes-unconventional-using-driver-phones-as-a-backup-dat.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, September 21, 2015 at 8:56AM</span></div>

<div class="body">

![](https://c2.staticflickr.com/6/5766/20975061403_7fa4e71579_m.jpg)

In [How Uber Scales Their Real-Time Market Platform](http://highscalability.com/blog/2015/9/14/how-uber-scales-their-real-time-market-platform.html) one of the most intriguing hints was how Uber handles datacenter failovers using driver phones as an external distributed storage system for recovery.

Now we know a lot more about how that system works from Uber's [Nikunj Aggarwal](https://www.linkedin.com/pub/nikunj-aggarwal/20/878/3b4) and [Joshua Corbin](https://www.linkedin.com/in/joshuatcorbin), who gave a very interesting talk at the [@Scale](http://www.atscaleconference.com/) conference: [How Uber Uses your Phone as a Backup Datacenter](https://www.youtube.com/watch?v=0EhTOKcwRok).

Rather than use a traditional backend replication scheme where databases sync state between datacenters to achieve a measure of [k-safety](https://my.vertica.com/docs/5.0/HTML/Master/10730.htm), Uber did something different, what they do is store enough state on driver phones so that if a datacenter failover occurs trip information can not be lost on the failover.

Why choose this approach? The traditional approach would be much simpler. I think it is to make sure the customer always has a **good customer experience** and losing trip information for an active trip would make for a horrible customer experience. 

By building their syncing strategy around the phone, even thought it's complicated and takes a lot work, Uber is able to preserve trip data and make for a seamless customer experience even on datacenter failures. And making the customer happy is what counts, especially in a market with **near zero switching costs**.

So the goal is not to lose trip information, even on a datacenter failover. Using a traditional database replication strategy it would not be possible to make this guarantee for reasons that have parallels to how [network management systems](http://whatis.techtarget.com/definition/network-management-system) have always had to work. Let me explain.

In a network devices are the **authoritative source for state information** like packet errors, alarms, packets sent and received, and so on. The network management system is authoritative for configuration data like alarm thresholds and customer information. The complication is devices and the network management system are not always in contact, so they get out of sync because they work independently of each other. Which means on bootup, failover, and communication reconnection all this information has to be merged in both directions using a complicated dance that ensures correctness and consistency. 

Uber has the same problem, only the devices are smart phones and the authoritative state the phone contains is trip information. So on bootup, failover, and communication reconnection the trip information must be preserved because **the phone is the authoritative source for trip information**.

Even when connectivity is lost the phone has an accurate record all trip data. So you wouldn't want to sync trip data from the datacenter down to the phone because that would wipe out the correct data on the phone. The correct information must come from the phone.

Uber also takes another trick from network management systems. They periodically query phones to test the integrity of information in the datacenter. 

Let's see how they do it...

## <span>Motivation for Using Phones as Storage for Datacenter Failure</span>

*   <span>Not long ago a failed datacenter would cause customer trips to be lost. That’s now fixed. On a datacenter failure the customer is right back on their trip with almost no noticeable downtime.</span>

*   The request of a trip, offering it to driver, the acceptance of a trip, picking up the rider, and ending the trip are called **state change transitions**. A trip transaction lasts as long as the trip lasts.

*   <span>From the moment a trip is started trip data is created in a back-end datacenter. It appears there’s a designated datacenter per city.</span>

*   **The typical solution for datacenter failure**: replicate data from the active datacenter to the backup datacenter. Well understood and works pretty well depending on your database. Drawbacks:

    *   <span>Gets complicated beyond two backup datacenters.</span>

    *   <span>Replication lag between datacenters.</span>

    *   <span>It requires a constant high bandwidth between datacenters, especially if you have a database that doesn’t have good support for datacenter replication or if you haven’t tuned your business model to optimize deltas.</span>

    *   <span>(A benefit that's not talked about, that probably doesn't matter for Uber, but may matter for smaller players, is that the driver phone plan is subsidizing bandwidth costs by not having to pay as much for inter-datacenter bandwidth.)</span>

*   **The creative application aware solution**: since there’s constant communication with driver phones just save the data to driver phones. Advantages:

    *   <span>Can failover to any datacenter.</span>

    *   <span>Sidesteps the problem of a phone failing over to the wrong datacenter, which would cause all the trips to be lost.</span>

*   <span>Using driver phones to hold datacenter backups requires a replication protocol.</span>

    *   <span>All the state transitions occur when communicating with the datacenter. There’s a Begin Trip or Begin Drive request, for example, which is the perfect opportunity to exchange state data with the phone and have a phone store data.</span>

    *   <span>On a datacenter failover when the phone pings the new datacenter the trip data is requested off of the phone. The downtime is very minimal. (no information on how datacenter maps are handled).</span>

*   <span>Challenges:</span>

    *   <span>Not all the saved trip information should be accessible to the driver. A trip has a lot of information on all the riders, for example, which should not be exposed.</span>

    *   <span>Have to assume driver phones can be compromised which means the data must be made tamper proof. So all the data is encrypted on the phone.</span>

    *   <span>Want to keep the replication protocol as simple as possible to make it easy to reason about and easy to debug.</span>

    *   <span>Minimize extra bandwidth. With a phone based approach it’s possible to tune what data is serialized and what deltas are kept in order to minimize traffic over the mobile network.</span>

*   <span>The Replication Protocol</span>

    *   <span>A simple key-value store model is used with get, set, delete, list of keys operations.</span>

    *   <span>A key can only be set once to prevent accidental overwrites and out of order message problems.</span>

    *   <span>With the set once rule versioning had to move into the key space. Updating a stored trip happens like: set(“trip1, version2”, “yyu”); delete(“trip1, version1”). The advantage is if there’s a failure between the set and delete there will be two values stored instead of nothing stored.</span>

    *   Failover resolution just a matter of merging keys between the phone and the new datacenter by: comparing stored keys to any known ongoing trips for the driver; maybe sending one or more _get_ operations to the phone for any missing data.

## <span>How They Got the Reliability of the System to Work at Scale</span>

### <span>Goals</span>

*   **Ensure the system is non-blocking while still providing eventual consistency**. Any back-end application in the system should be able to make progress, even when the system is down. The only tradeoff the application should be making is that it may take time for the data to be stored on the phone.

*   **Be able to move between datacenters without having to worry about the data already there**. There needs to be a way to reconcile the data between the driver and the servers.

    *   <span>When failing over to a datacenter that datacenter has a view of active drivers and trips, no service in the datacenter is aware a failure occurred.</span>

    *   <span>On failing back to the original datacenter the driver and trip data is stale, which makes for a bad customer experience.</span>

*   **Make it testable**. Datacenter failures are rare, so it’s typically a hard feature to test. They want to be able to constantly measure the success of the system so they can be confident a failover will succeed when it happens.

### <span>The Flow</span>

*   <span>A driver makes an update/state change, for example, picking up a passenger. That update comes as a request to the Dispatch Service.</span>

*   <span>The Dispatch Service updates the trip model for the trip. The update is sent to the Replication Service.</span>

*   <span>The Replication Service queues the request and returns success.</span>

*   <span>The Dispatch Service updates its own datastore and returns success to the mobile client. Other data might be returned as well, for example, if it’s an Uber Pool trip another passenger might need to be picked up.</span>

*   <span>In the background the Replication Service encrypts the data and sends it to the Messaging Service.</span>

*   <span>The Messaging Service maintains a bidirectional channel with all drivers. This channel is separate from the original request channel that drivers use to communicate with services. This ensures normal business operations are not impacted by the backup process.</span>

*   <span>The Messenger Service sends the backup to the phone.</span>

*   <span>The benefits of this design:</span>

    *   **Applications have been isolated from replication latencies and failures**. The Replication Service returns immediately. And the application only has to make an a cheap call (within the same datacenter) to have the data replicated.

    *   **The Messaging Service supports arbitrary querying of the phone without impacting normal business operations**. The phone can be treated as a basic key-value store.

### <span>Moving Between Datacenters</span>

*   First approach was to **manually run scripts on failover** to clean up old states from the database. This approach had **operational pain** as someone had to do it. And since it’s possible to failover by city or multiple cities at a time, the scripts became way too complicated.

*   Recall that keys in they key-value database contain a trip ID and a version number. The version number used to be an incrementing number. That was changed to a **modified vector clock**. Using the vector clock data on the **phone can be compared to data on the server**. Any causality violations can be detected and resolved. This solves the problem reconciling of in-progress trips.

*   Traditionally completed trips were deleted from the phone so replication data would not grow without bounds. The problem is that when failing back to the original datacenter that datacenter will have stale data, which can cause scheduling anomalies. The fix is on trip completion a special [tombstone](https://en.wikipedia.org/wiki/Tombstone_(data_store)) key is used. The version has a flag that says the trip has been completed. When the Replication Service sees the flag it can tell the Dispatch Service that the trip has completed.

*   <span>Storing trip data is expensive because it’s a huge encrypted blob of JSON data. Completed trips require much less storage. A weeks worth of completed trips can be stored in the same space as one active trip.</span>

### <span>Ensuring 99.99% Reliability</span>

*   <span>The failover system constantly tested to establish the confidence that it works and that a failover will be successful.</span>

*   First approach was **manual failovers of individual cities**. Then look at the success rate of the restoration and debug problems by looking at the logs.

    *   **High operational pain**. Performing this process manually every week didn’t work.

    *   **Poor customer experience**. Fares had to be adjusted for the few trips that did not restore correctly.

    *   **Low coverage**. Only a few cities at a time could be tested and since some problems hit only specific cities, perhaps because of a new city specific feature,  those bugs would be missed.

    *   **No idea if a backup datacenter could handle the load**. There’s a primary and backup datacenter. Even if they are configured the same how do you know the backup datacenter can handle the thundering herd problem, that is the flood of requests that occur on a failover.

*   To fix these problems they **looked at the key concepts** in the system they wanted to test.

    *   <span>**Ensure all mutations in the Dispatching Service are actually stored on the phone**. For example, a driver right after picking up a passenger may lose connectivity so replication data may not be sent to the phone immediately. Need to ensure the data eventually makes it to the phone.</span>

    *   **Ensure stored data can be used for replication**. Are there any encryption/decryption issues, for example. Are their any problems merging in the backup data?

    *   **Ensure the backup datacenter can handle the load**.

*   <span>To monitor the health of the system a Monitoring Service was born.</span>

    *   <span>Every hour the service gets a list of all active drivers and trips from the dispatch service. For all drivers the Messaging Service is used to get the replication data.  </span>

    *   <span>The data is then compared to see if the data is as expected. This yields a lot of good health metrics, like what percentage of failed.</span>

    *   <span>Breaking down the metrics by region and app version was a big help in pinpointing problems.</span>

*   A **shadow restoration** is used to test the backup datacenter.

    *   <span>Data collected by the Monitoring Service is sent to the backup datacenter for a shadow restoration.</span>

    *   The **success rate** is calculated by using the Dispatch Service to query and compare a snapshot from the primary datacenter to the number of active drivers and trips from the backup datacenter.  

    *   <span>Metrics around how well the backup datacenter handled the load are also calculated.</span>

    *   <span>Any configuration issues in the backup datacenter can be caught by this approach.</span>

## Related Articles

*   [On Hacker News 3](https://news.ycombinator.com/item?id=10446835) / [On HackerNews](https://news.ycombinator.com/item?id=10253158) / [On HackerNews 2](https://news.ycombinator.com/item?id=10271850)  / [On reddit](https://www.reddit.com/r/programming/comments/3mp4al/uber_goes_unconventional_using_driver_phones_as_a/)
*   [Data Serving Taking Storage for a Ride with Uber](https://www.youtube.com/watch?v=Dg76cNaeB4s)

</div>