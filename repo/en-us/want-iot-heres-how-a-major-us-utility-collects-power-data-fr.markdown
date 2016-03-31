## [Want IoT? Here's How a Major US Utility Collects Power Data from Over 5.5 Million Meters](/blog/2015/9/7/want-iot-heres-how-a-major-us-utility-collects-power-data-fr.html)

    

    

[![](https://farm1.staticflickr.com/735/20975791628_74875669c6_m.jpg)](https://farm1.staticflickr.com/735/20975791628_7ef7fcd3f3_o.jpg)

_    I serendipitously found this fascinating reply by     [    Richard Farley    ](https://www.linkedin.com/in/drfarley)    , your friendly neighborhood meter reader, in a local email list giving a rare first-hand account of how the     [    Advanced Metering Infrastructure    ](https://en.wikipedia.org/wiki/Smart_meter)     works in California. This is real Internet of Things territory. So if it doesn't have a typical post structure that is why. He generously allowed it to be reposted with a few redactions. When you see “A Major US Utility”, please replace it with the most likely California power company.    _

    Old mechanical meters had bearings that over time wore out and caused friction that threw off readings. That friction would cause the analog gauge to spin slower than it should, resulting in lower readings than actual usage -- hence "free power". It's like a clock falling behind over     time as the gears wear down.

    For         _A Major US Utility_         "estimated billing" happens when your meter, for whatever reason, was not able to be read. The algorithms approved by the     [    CPUC    ](http://www.cpuc.ca.gov/PUC/documents/codelawspolicies.htm)     and are almost always favorable to the consumer.         _A Major US Utility_         hates to have to do estimated billing because they almost always have to underestimate based on the algorithms and CPUC rules. Not 100% sure about this, but if they underestimate, they have to eat the cost. In the rare case they overestimate (i.e., you were on vacation during the missed period), you will get "trued up" in the next billing cycle.    

    _A Major US Utility_         does not see your actual use in "real time". For those interested in the nuts and bolts, here's how         _A Major US Utility_        ’s AMI system works (AMI is short for Advanced Metering Infrastructure):    

*       Every hour your residential meter measures your power usage for the past hour and stores it (encrypted) in your meter. For commercial meters, the measurement is taken every 15 minutes.    

*       Depending on the actual meter manufacturer (        _A Major US Utility_         uses several) it may  also record other sensor data like temperature and voltage.    

*       Your meter is connected to a mesh network that includes all of your neighbor's meters as well as multiple "access points" similar to what you might be running at home for your WiFi network. Communication is line-of-sight, the range is on the order of hundreds of yards.      

*       Your meter has a globally unique IPv6 address, which is part of a VERY large IPv6 network.    

*   Access points are usually located at the top of power poles and service a fairly large area -- _A Major US Utility_ has around 2,000 of these servicing ~5.5 million homes and businesses throughout northern and central Ca.

*       If your meter can't connect directly to the access point, it will relay through neighboring meters. In fact, most meters take at least a few hops to get to the access point, and a very large percentage of the transmission from your meter is actually relaying neighboring traffic. The mesh is very resilient, automatically reforming around failures in the network.    

*       The access points connect usually via cellular (sometimes satellite, sometimes Ethernet) to multiple data centers primarily on the West Coast.    

*       Every 4 hours the data center makes a connection to your meter and downloads all the metering data recorded since the last download. These communications are all encrypted.    

*   This all happens very quickly -- within 30 minutes _A Major US_ _Utility_ has read over 95% of those 5.5 million meters. Each meter usually takes less than a second to read.

*       The AMI system feeds the billing system.         _A Major US Utility_         sends out bills to around 5% of its customers every day, using the latest data that was  downloaded from the meter within hours. With manual meter reading it was many days or even weeks after the latest read before         _A Major US Utility_         was able to send a bill out and get their money.  "Meter-to-cash" time.    

    Your meter also has a capacitor which gives it time to send out a "last gasp" message if you lose utility power.         _A Major US Utility_         (usually) knows when you lose power. Generally these last gasp messages get through >80% of the time, but it varies by area density and topology.  Success rate partly depends on how many neighboring meters the message has to get relayed through. When your power goes out, it usually also does for most of your neighbors, so the relay chain might get cut off. Success is better at the edges of outages where neighboring meters still have power.    

    Many utilities measure residential power usage every 15 minutes and some pilot projects are working with measurements up to every 15 seconds, enabling something called "load dis-aggregation" analysis, which is the ability to identify usage of individual appliances based on their power signatures, and even identify when you have an appliance problem such as your refrigerator compressor going out. As you might imagine, this generates A LOT of data, as well as some serious privacy concerns!     

## Related Articles

*   [On  HackerNews](https://news.ycombinator.com/item?id=10182054)

    