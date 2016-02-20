## [If You're Programming a Cell Phone Like a Server You're Doing it Wrong](/blog/2013/9/18/if-youre-programming-a-cell-phone-like-a-server-youre-doing.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, September 18, 2013 at 8:55AM</span></div>

<div class="body">

![](http://farm4.staticflickr.com/3709/9802216885_7c09005ca8_m.jpg)

Power on a cell phone is like water in a desert. It’s the very stuff of life. If you take the same naive programming techniques you learned when programming on a server in a datacenter your cell phone will die of thirst.

This is dramatically shown by [<span>Reto Meier</span>](https://plus.google.com/+RetoMeier/posts)<span>, Tech Lead for the Android Developer Relations Team, in a remarkable series of instructional videos:</span>

***   [DevBytes: Efficient Data Transfers - Understanding the Cell Radio](https://www.youtube.com/watch?v=cSIB2pDvH3E)

    *   [<span>DevBytes: Efficient Data Transfers - Analyzing Your Transfer Profile</span>](https://www.youtube.com/watch?v=cLqWYeQcG94)

    *   [<span>DevBytes: Efficient Data Transfers - Effective Prefetching</span>](https://www.youtube.com/watch?v=Rk1u7VVmadE%20)

    *   [DevBytes: Efficient Data Transfers - Batching, Bundling, and SyncAdapters](https://www.youtube.com/watch?v=5onKZcJyJwI)

    **

Remarkable because these videos are short, to the point, and chocked full of useful ideas and techniques. Though Android is targeted specifically, most of content should be generally useful.

As an example of how server programming differs from mobile programming, on a server copying a file from one server to another server is usually a one or two liner. Read a block of data from a file descriptor and write it to another file descriptor. It's a synchronous process. You can mess with block sizes and other tricks, but that's the basics. Using this naive natural as nature “sipping” type logic on a cell phone is wrong, it's the _Little Cookie_ approach we'll talk about later. The problem is it drains the phone battery. Why? The cell radio will be on continuously. Why? You’ll learn that a bit later, but what you need to do is minimize radio usage by batching and properly scheduling transfers. You are not in Kansas anymore. 

Which brings us to the...

**Key idea**: The cell radio is one of the biggest battery drains on a phone. Every time you send data, no matter how small, the radio is powered on for up for 20-30 seconds. Every decision you make should be based on minimizing the number of times the radio powers up. Battery life can be dramatically improved by changing the way your apps handle data transfers. Users want their data now, the trick is balancing user experience with transferring data and minimizing power usage. A balance is achieved by apps carefully bundling all repeating and intermittent transfers together and then aggressively prefetching the intermittent transfers.

## <span style="font-weight: normal;">How does a cell radio work?</span>

<span>To minimize radio usage and thus minimize power usage you must first understand how a cell radio works. The videos do a great job explaining all this, but here’s the gist:</span>

*   <span>Cell radios are controlled by a state machine that tries to balance low latency and longer battery life. Cell radios are not kept on permanently. They enter different states as a means of saving battery life, but must go to full power when sending data over the radio.</span>

*   <span>Begins in Standby mode where it draws minimum power until an app initiates a data transfer.</span>

*   To send data the radio transitions to Full Power mode, a process that takes 2 seconds **before** performing the transfer. It will remain in full power mode for a set tail time just in case more data needs to be transferred. This avoids the ramp up time to Full Power state. State transitions themselves are a significant power drain so need to minimized.

*   <span>If nothing happens in a 5 to 10 second tail state it transitions to an intermediate Low Power state where it uses less power and has a shorter transition time to Full Power.</span>

*   <span>If nothing happens for another 30-60 seconds it will drop back down to Standby.</span>

*   <span>Exact latencies and tail times vary by carrier, network, and device.</span>

<span>It should be clear that if you send data naively on a cell phone you are screwed. Which brings us to the  two models of how to transfer data:</span>

*   <span>**Big Cookie Model**</span><span>: When scheduling downloads download as much as you can, as infrequently as possible, minimizing the number of transfers and maximizing bandwidth usage.</span>

*   <span>**Little Cookie Model**</span><span>: Transfer as little data as possible and perform transfers more frequently.</span>

<span>Who is the winner?</span> <span>**Big Cookie**</span><span>. Little Cookie heavily fragments radio use. For every data transfer the radio stays on for 5 seconds at full power followed by 10-60 seconds at a lower power state before returning to Standby. Every time you transfer data you are powering the radio for at least 20 seconds. So sending small amounts of data frequently is the best way to drain a battery. Let’s say you send analytics data every 15 seconds and the the user clicks on a link intermittently. The result will be the radio is on continuously. So don’t do that.</span>

## <span style="font-weight: normal;">What should your app do?</span>

<span>The upside of knowing about the cell state machine is that if you work with it you can balance application latency and power usage. The videos go into a number of techniques to bring about balance:</span>

*   <span>Minimize the number of radio state transitions.</span>

*   <span>Determine your applications battery usage profile</span>

    *   <span>Generate graphs using: Logcat logging / Application Resource Optimizer / Network Statistics in DDMS</span>

*   <span>Analyze graph for battery inefficiencies</span>

    *   <span>Look for regular pattern of transfers. This will cause regular radio usage and power drain. Shorter the period between updates the greater the power drain.</span>

    *   <span>Look for short spikes in height or duration. These could be be batched together or prefetched.</span>

*   <span>Prefetch data for the next 2-5 mins (1-5mb)</span>

    *   <span>Decreases latency and improves battery performance. By downloading all the data a user is likely to need in a single burst over a single connection at full capacity, the number of radio activations is significantly reduced.</span>

    *   <span>Challenge is figuring out what to download and when and not wasting power by downloading data that’s never used.</span>

    *   <span>On a 3G you can prefetch in 6 seconds enough information for a user session of 2 to 5 minutes of app usage, which is 1 to 5mb of data. If that data has a 50% chance of being used in the current session the cost of downloading the unused data matches the potential savings lost by not downloading that data to begin with. As the likelihood of the data being used increases the value of prefetching more data increases.</span>

    *   <span>Not every network transfers data at the same rate  so the equation will change based on the speed and efficiency of the network. The size of the prefetch cache must be increased or decreased based on the speed and cost of each network. On a faster 4G network significantly more data must be prefetched to account for the higher amount of data that can be downloaded and the higher battery cost of 4G.</span>

    *   <span>It’s more efficient to have transfers occur when a radio is in its active state, so if a time sensitive transfer is initiated, look to preempt, by transferring the data now, any transfers that will need to occur in a few minutes.</span>

    *   <span>For example, if an article needs to be fetched for a user to read, it’s a good time to also prefetch other content the user might read in the next few minutes.</span>

    *   <span>For a music player, maintain a buffer of 1 song + the song being played. This balances against downloading an entire album because it probably won’t be listened too.</span>

    *   <span>For a news reader, the naive way is to download top level and thumbnails, this keeps the radio constantly busy. Instead, download the first set of headlines and thumbnails and article text and the get the next batches later. You can try either depth first  or breadth first strategies. A better approach is to use science. Keep track of what your users and their friends read to predict what they might read and therefore what you should prefetch. Or you can prefetch everything, which is expensive if the data is never used.</span>

    *   <span>Use Device State to schedule downloads when battery life and bandwidth aren’t as important like when it’s charging.</span>

    *   <span>Use the current activity of the user to modify the aggressiveness of prefetching. When the app is open and the user is standing still then prefetch more.</span>

    *   <span>Generally don’t prefetch when an app is in the background.</span>

    *   <span>Don’t delay an app from starting. Don’t use a splash page. Process data in the background concurrently to minimize startup latency.</span>

    *   <span>Use HTTP live streaming where possible. It transfers data in bursts rather than a continuous stream which would keep the radio constantly active.</span>

    *   <span>Batch and bundle all non-time critical transfers.</span>

    *   <span>Transfer the batch the next time a time sensitive operation is performed.</span>

    *   <span>When repeating events must be sent randomize the periodicity</span>

    *   <span>If an operation is not time sensitive, say uploading an image, it might be better to wait 30 seconds just in case another image or piece of data also needs to be uploaded (see SyncAdapter)</span>

*   <span>Eliminate client-side polling</span>

    *   <span>Use Google Cloud Messaging. Data is only sent to your device when there’s data to send. So no polling loops. Gives lower latency and better battery usage.</span>

*   <span>Transfer less data, less often</span>

*   <span>Eliminate the (need for a) refresh button.</span>

*   <span>Reduce updates based on app use.</span>

*   <span>Create a batch queue to which you can add delay tolerant transfers. The next time you execute an on demand transfer you can also transfer all the queued data. Data can be lost if the app is closed before transfers occur. The way to get around this is add the data to a local database and query the database for pending transfers. The data in the database will stick around after the app is closed. Android has something called SyncAdapter to make this process much easier on the programmer. It implements all these best practices.</span>

<span>The videos go into all of this in more detail, especially on the SyncAdapter, and they were well worth watching.</span>

<span>These videos make clear what may have not been clear before: programming cell radio based mobile devices is a specialized domain that takes some specialized knowledge and techniques to do well. If you've always wondered why apps use so much power and your battery doesn't last as long as it should, it's easy to see why.</span>

## From the Comments

### iOS7 Notes

[Plorkyeran](http://www.reddit.com/user/Plorkyeran): iOS finally added background data fetching in iOS 7, but it's very heavily restricted (the OS chooses when apps get woken up to fetch, not the app, and in practice it can be as rare as once a day). Other than that, only a very limited number of categories of apps get to do stuff in the background (such as music players, voip apps, and location trackers), and they actually do reject apps which claim to qualify for the exemption but don't.

[dzamir](http://www.reddit.com/user/dzamir): In fact iOS7 uses the same strategy described in this post: it wake ups all the applications that require a background download at the same time to minimize the time the radio is on.

### Isn't Worrying About Power Just a Case of Premature Optimization?

[miratrix](https://news.ycombinator.com/item?id=6414445): The difference between DCH (the full-on high power state) and PCH (the lowest power, waiting-for-paging state) is about 2-orders of magnitude. Last time I measured, it was about ~100mA vs ~1mA (at ~3.7V) used by the radio. So, it's not couple of minutes of battery life, but rather _many_ hours of standby life instead. People usually aren't very happy when a fully charged phone dies overnight.

I've seen apps do some crazy things and it really has a significant effect in over-all battery life. A popular Android weather clock widget woke the phone up every minute to update the minute number on the graphics and updated the weather information every ~15 minutes (gps + radio!) which single handily crushed the standby battery life from multiple days to less than 8 hours.

Yes, I don't think _every_ decision should be based on minimizing the wake ups... but on the other hand, all developers should at least try to have as much understanding of the platforms that they're working on so that they know what trade-offs they're making with each feature they're adding.

[kintamanimatt](https://news.ycombinator.com/item?id=6414665): Respecting known limitations and working within best practices is absolutely not premature optimization. If you know that using the mobile radio in a certain way is a source of excessive battery usage, it's silly to just disregard this information. It's not premature optimization if you know where the performance issues are from the outset, and these performance issues are so incredibly common that the Android team put out videos about them.

The mobile radio will eat your battery very quickly, and probably chews through as much power as your screen. If you want to see how expensive it actually is, disable fast dormancy†. A slightly less brutal demonstration can be had by opening an OpenVPN connection. The keep-alive packets will keep your mobile radio in a higher power state, pretty much in the same way some disrespectful apps do and you'll (quite unsurprisingly) see your battery drain faster.

A badly written app can drain hours of battery charge, not minutes.

[kevin_nisbet](https://news.ycombinator.com/item?id=6414665):  I just want to add in from a network carrier perspective. I work for a mobile operator, and have had to work on several network issues related to poorly designed apps. I agree with kintamanimatt on this, it's not premature optimization, you need to know the cost of what you're doing. There is an incredible resource cost, to all the naive developers making apps these days, that treat the wireless interface as an always on connection.

1\. Server polling every minute We actually sent one customer an invoice for over $100,000 because they had a particularly bad application that polled a server every minute. This was only an internal app used by a few hundred people, but they used it at their office, and it caused constant blocking on the cell site covering their office. This actually broke cellular coverage near their office, and caused issues for all the other customers in the same area. Ultimately, we said, we can help you fix you're app, you can pay the invoice to upgrade cell coverage to you're office, or get off our network. In this instance, we helped them fix their app, so it only interacted with the server when something needed to be changed (the server pushed changes), which was actually quite easy to do. This simply isn't the same thing, as writing an inner loop in assembly, to shave off a few microseconds in runtime, which would be premature optimization, and actually making robust software that works well wherever it is used.

2\. Synchronized network access This one has been the bane of us on a few occasions. If you write a network access to be like a cron entry, that run's not only every 15 minutes, but does so at exactly the same time between all devices. We get to hunt you down, and scream, as our wireless network crumbles. When every device, wakes up at the same time, and asks for a higher power state, at exactly the same time, the network will only process so many. The funny thing is, you'll test this yourself and not see anything, but then when you release your app, you might find 30% of those checks are failing for some reason, and struggle to figure out why. For any one who is thinking, well the operator should add more capacity, the answer is we do, but ultimately the customer pays for the network build.

3\. High usage apps What happens is, your device vendor, is in the interest of protecting it's customers as well. We once had an app on our network, that would treat all reject codes, as a cause to retry. This caused it, to every time it was run, begin uploading data, over and over again, leading to thousands of dollars in bills to the customers for excessive data usage. The device vendor, pulled the app from their stores.

Now, how do you feel, about having to plead the case why you should be allowed to continue to sell your product, not only to the device vendor, but the carrier who found the problem, prove that you fixed it, have our bureaucracy test that you actually fixed it, and maybe if we feel like it, let you sell again. It's not as simple as fix the bugs, you might actually lose you're ability to make money of you're software, for months while this get's sorted out.

What do you think you're store rating will be, when all your customers get sent a $5000 bill for using your app?

Now the question is, should there be a better way, and I'd like to see more from mobile vendors, in making it stupid easy to be smart about this. In the phone API's, if I need to do a background update, I should be able to register with the OS, and say, I have an update to do, within the next 15 minutes, let me know when you have an active radio connection. You're phone goes into a high powered radio connection, and bam, all you're background updates now go out together. Need to send notifications, here is our push API, only interact with the mobile when an update needs to be done. If the API's you're using, are designed for mobile, and take alot of this network stuff that you shouldn't need to know into account, it's a lot easier for the naive developers to do better by blind luck.

-- * my views in this post are my own, and do not reflect those of my employer.

## Related Articles

*   [On Reddit](http://www.reddit.com/r/programming/comments/1mngrg/if_youre_programming_a_cell_phone_like_a_server/) / [On Hacker News](https://news.ycombinator.com/item?id=6413395)
*   [Optimizing Downloads for Efficient Network Access](http://developer.android.com/training/efficient-downloads/efficient-network-access.html)
*   [Mobile Performance from the Radio Up: 4G Architecture and its Implications](https://www.youtube.com/watch?v=a4SbDZ9Y-I4)
*   [Breaking the 1000ms Time to Glass Mobile Barrier](https://www.youtube.com/watch?v=Il4swGfTOSM) 

</div>