## [Wide Fast SATA: the Recipe for Hot Performance](/blog/2013/9/4/wide-fast-sata-the-recipe-for-hot-performance.html)

    

    

![](http://farm9.staticflickr.com/8319/7952195310_8078e8c9df_m.jpg)    This is a guest post by     [Brian Bulkowski](http://www.aerospike.com/brian-bulkowski/)    , CTO and co-founder of     [Aerospike](http://www.aerospike.com/)    , a leading clustered NoSQL database, has worked in the area of high performance commodity systems since 1989\.    

This blog post will tell you exactly how to build a multi-terabyte high throughput datacenter server. A fast, reliable multi-terrabyte data tier can be used for recent behavior (messages, tweets, plays, actions), or anywhere that today you use Redis or Memcache.

You need to know:

*   Which SSDs work
*   Which chassis work
*   How to configure your RAID cards

Intel’s SATA solutions – combined with a high capacity storage server like the Dell R720xd and a host bus adapter based on the LSI 2208, and a Flash optimized database like [Aerospike](http://www.aerospike.com/), enables high throughput and low latency.

In a wide configuration, with 12 to 20 drives per 2U server, individual servers can cost-effectively serve at high throughput with 16T at $2.50 per GB with the s3700, or $1.25 with the s3500\. Other SSD offerings – from Crucial (Micron) and Samsung (S843) – are at other densities and price-performance points.

This is **in-memory computing at a stunningly new, accessible price level** – but there are some details you need to know.

How fast? How about **250,000 network database transactions per second** against that multi-terabyte store. You’ll also need to properly configure your network layer as Russ Sullivan disclosed in the following [High Scalability blog post](http://highscalability.com/blog/2012/9/10/russ-10-ingredient-recipe-for-making-1-million-tps-on-5k-har.html). 

In order to achieve low latency and high performance, add more SATA devices. A customer of ours who is deploying this calculated a 12 drive minimum. A 2U system with front panel attach is the way to go – and the Dell R720xd has 24 2.5” drive bays on the front, far more than systems that are still 2U but allow only 8 drive bays.

Enterprise SATA SSDs have supply problems – simply because popularity. Currently, I see supply problems at higher densities (like the Intel s3700 800G). Using more and smaller drives gives higher performance at the same price per byte, and the smaller drives (200G) are available now – and at 22 bays per 2U, that’s still 4.4T per server.

When deploying wide SATA Flash solutions, the RAID card (HBA and Host Bus Adapter) easily becomes the bottleneck. We struggled for months with performance problems created by the RAID card before finding the following recipe. High performance cards, like the Dell H710p, use the LSI 2208, which has a “fast path” that must be invoked. Without invoking the fast path, the LSI 2208 will bottleneck at 4 drives. With the “fast path”, you’ll get the full speed of your wide SATA system.

The LSI fast path is discussed here: [http://www.lsi.com/channel/products/storagesw/Pages/MegaRAIDFastPathSoftware.aspx](http://www.lsi.com/channel/products/storagesw/Pages/MegaRAIDFastPathSoftware.aspx)

Steps to high speed:

1.  Get the right SSDs.
2.  Make sure your card has an LSI 2208 or similar.
3.  Get the MegaCli configuration tool.
4.  Configure each drive as a separate RAID0 volume, and Write Through (WT) and NO Read Ahead (NORA).
5.  Use a database which directly uses devices and manages parallel IO (like Aerospike). The Right SSDs

This blog post focuses on the Intel S3700 and S3500\. Other exciting SATA drives are the Samsung 843 and the Crucial (Micron) M500\. Drives from integrated Chip / Drive providers turn out to have the highest integration, and quality.

Our ACT ([https://github.com/aerospike/act](https://github.com/aerospike/act)) latency results (which stabilized after the 3rd hour of testing) for the 400G S3500 with 20% overprovisioning (required!) with “3x” profile (6,000 1.5K reads per second, 18MB/second streaming writes) are:

<table>

<tbody>

<tr>

<td>Hour</td>

<td>% < 1ms</td>

<td>% < 2 ms</td>

<td>% < 4 ms</td>

</tr>

<tr>

<td>3</td>

<td>1.6</td>

<td>0.67</td>

<td>0.00</td>

</tr>

</tbody>

</table>

Intel S3700 (400G) did not require overprovisioning, thus allowed all capacity, and had similar but slightly higher performance with the same profile:

<table>

<tbody>

<tr>

<td>Hour</td>

<td>% < 1ms</td>

<td>% < 2 ms</td>

<td>% < 4 ms</td>

</tr>

<tr>

<td>6</td>

<td>1.58</td>

<td>0.12</td>

<td>0.00</td>

</tr>

</tbody>

</table>

With a properly configured wide system, these numbers scale laterally. LSI 2208 or similar

The LSI 2208 is used in the Dell H710p, which is available for the R720xd. Other Dell cards, notably the H310 series, are particularly bad. Get the MegaCli configuration tool

The MegaCli tool is sometimes hard to find. We were able to find it eventually with Google. You can search at LSI’s website for the tool, by going to this URL: [http://www.lsi.com/support/Pages/Download-Results.aspx?keyword=MegaCli](http://www.lsi.com/support/Pages/Download-Results.aspx?keyword=MegaCli).

From here, open the "Management Software and Tools" bar and the latest MegaCLI shows up. Configure drives properly

The commands to configure RAID0 volumes are:

<pre>     MegaCli64 -CfgLdAdd -r0 [:] -a

           is likely 32
           will vary depending on the SSD you wish to set up as RAID 0.
           this is the ID of the RAID adapter (likely 0)
</pre>

Then, you need to put the drives in no write-through and no read ahead:

<pre> 
     MegaCli64 -LDSetProp WT -L -aALL; MegaCli64 -LDSetProp NORA -L -aALL
           will vary depending on the SSD you wish to set up as RAID 0.</pre>

These speeds are available when using direct device access. When you go through the Linux page cache, and through any of the Linux file systems, latency increases. Latency deviation increases dramatically.

These tests also require a high level of parallelism. The software layer must be capable of a high level of parallelism – levels of 128 to 512 simultaneous IOs per device is required.

    