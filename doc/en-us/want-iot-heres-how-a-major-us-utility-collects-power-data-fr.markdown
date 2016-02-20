## [Want IoT? Here's How a Major US Utility Collects Power Data from Over 5.5 Million Meters](/blog/2015/9/7/want-iot-heres-how-a-major-us-utility-collects-power-data-fr.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, September 7, 2015 at 8:56AM</span></div>

<div class="body">

[![](https://farm1.staticflickr.com/735/20975791628_74875669c6_m.jpg)](https://farm1.staticflickr.com/735/20975791628_7ef7fcd3f3_o.jpg)

_<span>I serendipitously found this fascinating reply by</span> [<span>Richard Farley</span>](https://www.linkedin.com/in/drfarley)<span>, your friendly neighborhood meter reader, in a local email list giving a rare first-hand account of how the</span> [<span>Advanced Metering Infrastructure</span>](https://en.wikipedia.org/wiki/Smart_meter) <span>works in California. This is real Internet of Things territory. So if it doesn't have a typical post structure that is why. He generously allowed it to be reposted with a few redactions. When you see “A Major US Utility”, please replace it with the most likely California power company.</span>_

<span>Old mechanical meters had bearings that over time wore out and caused friction that threw off readings. That friction would cause the analog gauge to spin slower than it should, resulting in lower readings than actual usage -- hence "free power". It's like a clock falling behind over </span>time as the gears wear down.

<span>For</span> <span>_A Major US Utility_</span> <span>"estimated billing" happens when your meter, for whatever reason, was not able to be read. The algorithms approved by the</span> [<span>CPUC</span>](http://www.cpuc.ca.gov/PUC/documents/codelawspolicies.htm) <span>and are almost always favorable to the consumer.</span> <span>_A Major US Utility_</span> <span>hates to have to do estimated billing because they almost always have to underestimate based on the algorithms and CPUC rules. Not 100% sure about this, but if they underestimate, they have to eat the cost. In the rare case they overestimate (i.e., you were on vacation during the missed period), you will get "trued up" in the next billing cycle.</span>

<span>_A Major US Utility_</span> <span>does not see your actual use in "real time". For those interested in the nuts and bolts, here's how</span> <span>_A Major US Utility_</span><span>’s AMI system works (AMI is short for Advanced Metering Infrastructure):</span>

*   <span>Every hour your residential meter measures your power usage for the past hour and stores it (encrypted) in your meter. For commercial meters, the measurement is taken every 15 minutes.</span>

*   <span>Depending on the actual meter manufacturer (</span><span>_A Major US Utility_</span> <span>uses several) it may  also record other sensor data like temperature and voltage.</span>

*   <span>Your meter is connected to a mesh network that includes all of your neighbor's meters as well as multiple "access points" similar to what you might be running at home for your WiFi network. Communication is line-of-sight, the range is on the order of hundreds of yards.  </span>

*   <span>Your meter has a globally unique IPv6 address, which is part of a VERY large IPv6 network.</span>

*   Access points are usually located at the top of power poles and service a fairly large area -- _A Major US Utility_ has around 2,000 of these servicing ~5.5 million homes and businesses throughout northern and central Ca.

*   <span>If your meter can't connect directly to the access point, it will relay through neighboring meters. In fact, most meters take at least a few hops to get to the access point, and a very large percentage of the transmission from your meter is actually relaying neighboring traffic. The mesh is very resilient, automatically reforming around failures in the network.</span>

*   <span>The access points connect usually via cellular (sometimes satellite, sometimes Ethernet) to multiple data centers primarily on the West Coast.</span>

*   <span>Every 4 hours the data center makes a connection to your meter and downloads all the metering data recorded since the last download. These communications are all encrypted.</span>

*   This all happens very quickly -- within 30 minutes _A Major US_ _Utility_ has read over 95% of those 5.5 million meters. Each meter usually takes less than a second to read.

*   <span>The AMI system feeds the billing system.</span> <span>_A Major US Utility_</span> <span>sends out bills to around 5% of its customers every day, using the latest data that was  downloaded from the meter within hours. With manual meter reading it was many days or even weeks after the latest read before</span> <span>_A Major US Utility_</span> <span>was able to send a bill out and get their money.  "Meter-to-cash" time.</span>

<span>Your meter also has a capacitor which gives it time to send out a "last gasp" message if you lose utility power.</span> <span>_A Major US Utility_</span> <span>(usually) knows when you lose power. Generally these last gasp messages get through >80% of the time, but it varies by area density and topology.  Success rate partly depends on how many neighboring meters the message has to get relayed through. When your power goes out, it usually also does for most of your neighbors, so the relay chain might get cut off. Success is better at the edges of outages where neighboring meters still have power.</span>

<span>Many utilities measure residential power usage every 15 minutes and some pilot projects are working with measurements up to every 15 seconds, enabling something called "load dis-aggregation" analysis, which is the ability to identify usage of individual appliances based on their power signatures, and even identify when you have an appliance problem such as your refrigerator compressor going out. As you might imagine, this generates A LOT of data, as well as some serious privacy concerns!</span> 

## Related Articles

*   [On  HackerNews](https://news.ycombinator.com/item?id=10182054)

</div>