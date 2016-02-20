## [Paper: Network Stack Specialization for Performance ](/blog/2014/2/12/paper-network-stack-specialization-for-performance.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, February 12, 2014 at 8:56AM</span></div>

<div class="body">

![](http://farm4.staticflickr.com/3720/12470482324_272e9ef2e6_n.jpg)

In the scalability is specialization department here is an interesting paper presented at [HotNets '13](http://conferences.sigcomm.org/hotnets/2013/) on high performance networking: [Network Stack Specialization for Performance](http://conferences.sigcomm.org/hotnets/2013/papers/hotnets-final43.pdf).

The idea is generalizing a service so it fits in the kernel comes at a high performance cost. So move TCP into user space.  The result is a web server with ~3.5x the throughput of Nginx "while experiencing low CPU utilization, linear scaling on multicore systems, and saturating current NIC hardware."

Here's a good description of the paper published [on Layer 9](http://www.layer9.org/2013/11/hotnets-13-network-stack-specialisation.html):

> Traditionally, servers and OSes have been built to be general purpose. However now we have a high degree of specialization. In fact, in a big web service, you might have thousands of machines dedicated to one function. Therefore, there's scope for specialization. This paper looks at a specific opportunity in that space. Network stacks today are good for high throughput with large transfers, but not small files (which are common in web browsing). For example one of their experiments showed **with 8KB files, ~85% CPU load for just ~50% throughput (5 Gbps)**.
> 
> The goal of this paper is to design a specialized network stack that addresses that problem, by reducing system overhead and providing free flow of memory between the NIC and applications. The paper introduces a **zero copy web server** called Sandstorm which accomplishes that.
> 
> Key design decisions include putting apps and the network stack in the same address space; static content pre-segmented into packets; batching of data; and bufferless operation. In performance evaluation, the most impressive improvements are in flow completion time when there are a small number of connections, and **CPU load reduction of about 5x**. When moving to 6x10GbE NICs, the thoughput improvement also becomes impressive: about 3.5x higher throughput than FreeBSD+nginx and even bigger improvement over Linux+nginx. Overall, the paper's contribution is a new programming model that tightly couples the application with the network stack, **reaching 55 Gbps at 72% load**.

Even if you didn't want to change the stack on your front-end facing components, you might consider it for your services running inside the datacenter:

> We have described Sandstorm, a high-performance webserver based on a specialized network stack exploiting a total-system zerocopy design, aggressive cross-layer amortization and information ﬂow, and a synchronous structure simultaneously promoting simple design and extremely high performance. The performance impact of these specializations is **particularly noticeable in data center environments due to a dramatic drop in the effective round-trip time** (across inter-connect and server-side software) allowing short TCP connections to ﬁll the available pipe much more quickly than conventional designs.

> General-purpose operating system stacks have been around a long time, and have demonstrated the ability to transcend multiple generations of hardware. We believe the same should be true of special-purpose stacks, but that tuning for particular hardware should be easier. We examined performance on servers manufactured seven years apart, and demonstrated that although the performance bottlenecks were now in different places, the same design delivered signiﬁcant beneﬁts on both platforms. It is our belief that a blend of specialized network stacks with performance-critical applications such as web services, can dramatically improve the scalability of current hardware, reducing costs and energy demands, offering a practical alternative to general-purpose designs.

## <span>Related Articles</span>

<div>

*   [Is It Time To Get Rid Of The Linux OS Model In The Cloud?](http://highscalability.com/blog/2012/1/19/is-it-time-to-get-rid-of-the-linux-os-model-in-the-cloud.html)

</div>

</div>