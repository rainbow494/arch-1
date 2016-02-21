## [How will new memory technologies impact in-memory databases?](/blog/2015/9/23/how-will-new-memory-technologies-impact-in-memory-databases.html)

    

    

![](https://c1.staticflickr.com/1/641/21032774823_11d8e08cce_m.jpg)

_This is a guest post by [Yiftach Shoolman](https://redislabs.com/company/redis-labs-team#board-Yiftach-Shoolman), Co-founder & CTO of [redislabs](https://redislabs.com/). Will 3D XPoint change everything? Not as much as you might hope..._

Recently, investors, analysts, partners and customers have asked me how the announcement from Intel and Micron about their new [3D XPoint memory technology](http://newsroom.intel.com/community/intel_newsroom/blog/2015/07/28/intel-and-micron-produce-breakthrough-memory-technology) will affect the in-memory databases market. In these discussions, a common question was “**Who needs an in-memory database if all the non in-memory databases will achieve similar performance with 3D XPoint technology?**” Well, I think that's a valid question so I've decided to take a moment to describe how we think this technology will influence our market.

    First, a little background...    

The motivation of Intel and Micron is clear -- **DRAM is expensive and hasn’t changed much during the last few years** (as shown below). In addition, there are currently only three major makers of DRAM on the planet (Samsung Electronics, Micron and SK Hynix), which means that the competition between them is not as cutthroat as it used to be between four and five major manufacturers several years ago.

    ![](https://lh5.googleusercontent.com/-o69IJGNepA-GP4eV1NNQEf1FSBlF3QCDpgVyXVGzCsQCfQlNiF4fNwjtwMKyKBWNDqGq5YtxMqusBS9AzM6Lbh4E8bRnc1BWUnpq5OjaKLtspOTQaprHOIajRmMGIctAAo6c6U)    

Now let’s talk about speed. If everything goes as planned, the **3D XPoint is going to be 1000 times faster** **than Flash** (GA is planned in Q1/16). That said, it will **still be 10x slower than DRAM**, as shown here:

    ![Performance of Memory Technologies.png](https://lh5.googleusercontent.com/9cJ9eQBnbgPsJgyEFcrcamAthy4YoB4bmter2Rt2TNfnU6cNdBHxL_nt_tB1yDmszJDegVpTAydyrZ7HLxSt3HUlr1Ka-0wCmmz1gSwd02VXXzCvUcw9cffmRWPfKFAo1_c9ysA)    

A non-in-memory database with the new **3D Xpoint technology can never reach the speed of an optimized in-memory databases** like Redis, as even existing DRAM-based databases lag behind. This year at the [    In-Memory Computing Summit    ](http://imcsummit.org/), I [    explained    ](http://www.slideshare.net/imcsummit/imcs2015-2-bus4-myth-about-inmemory-databases-busted?qid=234683ec-1e5a-41af-a7b4-3e5fe956ba16&v=default&b=&from_search=1) why all in-memory (DRAM-based) databases are NOT equally fast, and in fact differ in performance by between 2X and 20X, even if all databases run on the same HW/VMs and are benchmarked by the same load-generating tool that simulates the same use case.

    There are a number of reasons for this so I'll just mention a few (the rest can be found     [    here in slides 6-20    ](http://www.slideshare.net/imcsummit/imcs2015-2-bus4-myth-about-inmemory-databases-busted?qid=234683ec-1e5a-41af-a7b4-3e5fe956ba16&v=default&b=&from_search=1)    ): computation complexity, query efficiency, network overhead, shared-nothing vs. shared-something/everything, multithreaded vs. single-threaded architectures, built-in acceleration components, etc.    

Another perspective is by doing a quick thought experiment - let's assume that your Redis database is using solely the 3D Xpoint technology, **no data in RAM whatsoever** - what would be the impact on performance? We've performed that experiment on slower than RAM technologies and verified that **the impact is dramatic as you'd expect it to be: an order of magnitude decrease in performance** means that instead of being able to serve 100K ops/sec at sub-millisecond latency from a modest cloud instance, you'd be getting at most 10K ops/sec with double-figure millisecond latency. This degradation in performance will slow down your application and will force you to look for a different in-memory solution.

    So where can a technology like 3D XPoint benefit Redis (or other in-memory databases)?    

        

    A few months ago we did a soft launch for our codename "Redis-on-Flash" technology. We began developing this technology on IBM P8 servers embedded with     [    CAPI    ](http://www-304.ibm.com/webapp/set2/sas/f/capi/home.html)     and IBM FlashSystem, but now we’ve extended the technology to run on the top of any x86 platform including public cloud VMs with a relatively fast IOPS capabilities (the EC2 i2 family of instances for example).    

        

Redis-on-Flash treats Flash, **3D Xpoint and other types of Storage Class Memory (SCM) technologies like a RAM extender (or Slower RAM)** **rather than as persistent storage**, allowing us to rely on the existing data-persistence model of Redis without worrying about consistency when writing data to the Slower RAM.

To avoid the speed penalty associated with switching to a slower memory, our **design continues to maintain the Redis Dictionary (an internal data-structure for storing the  ‘key’ part of the key/value pairs) in DRAM**, letting Redis' main loop access it without degrading performance. In cases where the DRAM has additional free capacity, we use it to store hot values (which we fetch from Flash/SCM/3D Xpoint).

    This was one of the the most important decisions we took when we designed Redis for Slower RAM. Whenever the Redis main loop has to do something it first accesses its internal dictionary - if part of this dictionary had been stored on the Slower RAM, it would have slowed down Redis considerably.    

To further improve performance, we've **modified Redis to use an asynchronous multi-threaded architecture** **when accessing Slower RAM**. So for instance, if a request on a given connection requires access to Slower RAM, the main Redis thread delegates this request to a Slower RAM thread so it continues to process requests from other connections. This design also lets us we also ensure Redis’ consistency, because on a given connection requests are processed in the order they arrive in.

We were glad to learn from the Intel guys we met with that the **3D Xpoint technology allows the application to decide** (probably via an SDK) **which part of the dataset to store on DRAM and which part to store on Slower RAM**. We think this is the right (and probably the only) approach that enables benefiting from the speed of the fastest DRAM while keeping most of your data in Slower RAM.

Supporting an asynchronous multi-threaded architecture when accessing the Flash/SCM/3D Xpoint devices, does not sacrifice consistency, as shown in the figure below:

    ![RELC-RAM-Flash-website.png](https://lh3.googleusercontent.com/LSruaPf1NegJxZ3pBU-MM45Q8LXJU7kfTUP1yWe8gy1Z9rQSdhFoEaqtX0p3gNs-dcCkw8fdgosy09DrmPkzh5cYIx60L1IhMuNZsoDBJ7OgqBjpHu6a0cNUsYDEKhkbMhwDejk)    

Another important capability that we added is **a simple way to control the RAM:Flash ratio by adding a simple UI slider** (as illustrated below). This gives you the flexibility to configure Redis on the spectrum between boosting performance (by storing more data in RAM) and reducing cost (using Flash/SCM/3D Xpoint for storing data).

    ![RoF slider.png](https://lh5.googleusercontent.com/b3f0x3Jnux_kuQ40T_wfJUZk2LiYxVUaitS5i9kRj9V2GE8rXy4IACTDkNf3Yj6gqCvJJD5AH5J8Sv3E_JEjDXAIW4dHHoUQbYturyZerCQuxOakAlTV_QRikJ6ZDP0n8n91kFI)    

###     Some performance numbers    

    The table below shows the performance numbers you can get when running our Redis-on-Flash on a single IBM P8 server embedded with CAPI and IBM FlashSystem.    

    We used 1:1 read to write ratio with simple     [    GET    ](http://redis.io/commands/get)     and     [    SET    ](http://redis.io/commands/set)     operations while storing 10% of the dataset on RAM (and 90% on Flash). The RAM Hit Ratio represents the percentage of the requests that were served entirely from the RAM. The table is divided to two parts - the upper Low Latency section shows the throughput numbers at sub-millisecond latency whereas the lower High Throughput section  demonstrates the throughput at under 10msec latency:    

    

<table><colgroup><col width="293"><col width="288"></colgroup>

<tbody>

<tr>

<td>

    RAM Hit Ratio    

</td>

<td>

    Ops/Sec @ Latency    

</td>

</tr>

<tr>

<td colspan="2">

    Low Latency Scenarios    

</td>

</tr>

<tr>

<td>

    100%    

</td>

<td>

    1.35m ops/sec @ 1.0 msec    

</td>

</tr>

<tr>

<td>

    95%    

</td>

<td>

    569k ops/sec @ 0.96 msec    

</td>

</tr>

<tr>

<td>

    80%    

</td>

<td>

    340k ops/sec @ 1.07 msec    

</td>

</tr>

<tr>

<td>

    50%    

</td>

<td>

    200k ops/sec @ 0.96 msec    

</td>

</tr>

<tr>

<td>

    20%    

</td>

<td>

    160k ops/sec @ 1.0 msec    

</td>

</tr>

<tr>

<td colspan="2">

    High Throughput Scenarios    

</td>

</tr>

<tr>

<td>

    100%    

</td>

<td>

    2.0m ops/sec @ 2.4 msec    

</td>

</tr>

<tr>

<td>

    95%    

</td>

<td>

    1.02m ops/sec @ 3.14 msec    

</td>

</tr>

<tr>

<td>

    80%    

</td>

<td>

    671k ops/sec @ 6.2ms latency    

</td>

</tr>

<tr>

<td>

    50%    

</td>

<td>

    483k ops/sec @ 10ms latency    

</td>

</tr>

<tr>

<td>

    20%    

</td>

<td>

    366k ops/sec @ 14.5ms latency    

</td>

</tr>

</tbody>

</table>

    

Based on our experience from managing over 110,000 Redis databases, **these numbers fit over 95% of the Redis use cases**; and with newer technologies, such as the 3D XPoint, we expect this numbers to be even better.

    