## [Cloud Architecture Revolution](/blog/2014/6/5/cloud-architecture-revolution.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, June 5, 2014 at 4:56AM</span></div>

<div class="body">

<span>The introduction of cloud technologies is not a simple evolution of existing ones, but a real revolution.  Like all revolutions, it changes the points of views and redefines all the meanings. Nothing is as before.  This post wants to analyze some key words and concepts, usually used in traditional architectures, redefining them according the standpoint of the cloud.  Understanding the meaning of new words is crucial to grasp the essence of a pure cloud architecture.</span>

> <<_There is no greater impediment to the advancement of knowledge than the ambiguity of words._>>

> THOMAS REID, Essays on the Intellectual Powers of Man

<span>Nowadays, it is required to challenge the limits of traditional architectures that go beyond the normal concepts of scalability and support millions of users (What's Up 500 Million) billions of transactions per day (Salesforce 1.3 billion), five 9s of availability (99.999 AOL).  I wish all of you the success of the examples cited above, but do not think that it is completely impossible to reach mind-boggling numbers. Using cloud technology, everyone can create a service with a little investment and immediately have a world stage.  If successful, the architecture must be able to scale appropriately.</span>  

<span>Using the same design criteria or move the current configuration to the cloud simply does not work and it could reveal unpleasant surprises.</span>  

**Infrastructure - commodity HW instead of high-end HW**  
<span>This is the first shocking concept to understand about the cloud architectures. Forget about high-available and high-performance hardware. The largest cloud services providers like Amazon or Azure use commodity hardware so prone to breakdowns and other problems more than the traditional environments.</span>  
<span>Anyway, this kind of hardware does not prevent the use of the cloud for the enterprise applications with high availability and high-performance requirements, it is simply necessary to redesign the architecture according to different criteria of resilience and distribution.</span>  

**Scalability - dynamic instead of static**  
<span>The scalability is one of the important features of a good architecture design even in the traditional approach. Anyways, the scaling of an architecture was an exceptional event as it requires planning, budget allocation, new HW purchase and systems reconfigurations. This leads to implement very stable architectures over the time and sized for the maximum load that the application or service must support.</span>  
<span>In the cloud all the resources have been dematerialized and they may be requested on-demand and paid by the hour. The HW resources are managed to a high level of abstraction through the API (Application Program Interface) so you can automate the provisioning and de-provisioning of any resource.</span>  
<span>Nowadays architecture is required to adapt according to the demands and be able to expand by adding new nodes or contract. It must be possible to use a high number of nodes during peak hours or some periods of the year and fewer during low usage. Obviously, the architecture must be aware of these variations and change accordingly.</span>  

**Availability - resiliency instead of no failure**  
<span>The traditional architectures consider the resources always available. The unavailability of a resource is neither a desirable event nor planned, in other words, it is an anomaly. In the cloud, the unavailability of a resource is no longer an exceptional event due to the use of commodity HW or because of a resource, such as an application server, is not available due to a sudden and wanted downsize.</span>  
<span>The architecture must be able to handle the transient failure: in case of problems should be programmed to react properly and complete the transaction. In other words, it must be resilient.</span>  
<span>In the most advanced cloud architectures, there are some agents designed to generate failures deliberately in the production environments to ensure that the application is able to respond appropriately. Famous are the Chaos Monkey of Netflix.</span>  
<span>A good way to get the resilience is to implement a system of queues in order to make the components of the application loosely coupled and autonomous.</span>  
<span>In the example below, the web role puts the requests in the queue that are handled by the worker role. When the worker role takes over a request, it is not completely removed even if it is no longer available for processing by other worker roles. In case the processing worker role does not provide feedback within a certain timeout, the worker role ended suddenly, the request appears in the queue again and it is available for another worker role.</span>

<div class="separator">[![](http://2.bp.blogspot.com/-C7mFr9O6lvA/U2v8EIV9sNI/AAAAAAAACZ0/pjInaJHDK7U/s1600/Queue.png)](http://2.bp.blogspot.com/-C7mFr9O6lvA/U2v8EIV9sNI/AAAAAAAACZ0/pjInaJHDK7U/s1600/Queue.png)</div>

**Performance - decomposition and distribution instead of stress**  
<span>The cloud architectures are quite sensitive to performance constraints inherent in the technology itself such as the non-proximity of resources, the commodity hardware or the resource sharing. Methods related to the stress of the resource are no longer usable like increasing the number of IOPS, bandwidth, processor speed or the size of ram. In the cloud, the scale-up is very limited and in some cases impossible. Rather than strengthen a single component of the architecture, the decomposition of the architecture in different modules deployed across multiple nodes is the key to improve the performance.</span>  
<span>Traditionally, a system is considered scalable if and only if, increasing the requests and the nodes, the performances remain the same. Scalability is defined as a function of performance.</span>  
<span>In the new cloud architectures, the ratio should be reversed and the scalability must be folded to achieve better performance. The idea is to subdivide computational cost in order to perform better.</span>  
<span>This approach must be adopted not only on the application layer, but also on the data layer by means of database sharding that allow distribution of the database and consequently the load on multiple nodes that take charge of the management of specific portions data.</span>  
<span>Even a wise use of cache systems at all levels (application with memcache or Amazon's ElasticCache, network with cache on firewalls and contents with CDN) will certainly help to improve the performance of the architecture.</span>

<div class="separator">[![](http://1.bp.blogspot.com/-OdWvtthXrg4/U2v8hVLpnsI/AAAAAAAACZ8/zDCYNrnoJoQ/s1600/decomposition.png)](http://1.bp.blogspot.com/-OdWvtthXrg4/U2v8hVLpnsI/AAAAAAAACZ8/zDCYNrnoJoQ/s1600/decomposition.png)</div>

**Reliability - MTTR instead of MTBF**  
<span>Traditionally, the reliability of an architecture is related to the concept of the MTBF, the mean time between a failure and another, in other words, how much wider the period the more reliable the system. By using commodity servers, this can no longer be considered an achievable goal. Hence, it is necessary to review the concept of reliability by connecting it to the resiliency. A cloud architecture is much more reliable when lower is the MTTR i.e. mean time to recovery. MTTR equal or close to zero would give a highly reliable architecture and resilient to any failure.</span>

<div class="separator">[![](http://4.bp.blogspot.com/-g4sMRwfrnvc/U2v87O7NyeI/AAAAAAAACaE/eE52Z9wBZ5c/s1600/MTTR-MTBF.png)](http://4.bp.blogspot.com/-g4sMRwfrnvc/U2v87O7NyeI/AAAAAAAACaE/eE52Z9wBZ5c/s1600/MTTR-MTBF.png)</div>

**Capacity Planning - scale unit ****planning ****instead of  worst case planning**  
<span>The capacity planning in traditional architecture is designed to estimate the sizing required to support the maximum load so that the system is able to respond well enough even in the worst situations. This inevitably leads to waste a lot of resources for the most part of the life cycle of systems. In contrast, the capacity planning in the cloud architecture is oriented to determine the scale unit, i.e. the load enough to scale up or down, possibility automatically.</span>

<div class="separator">[![](http://2.bp.blogspot.com/-u2VJyYAUa8o/U2v9Unt3McI/AAAAAAAACaM/qWofwtOWwJM/s1600/capacity+planning.png)](http://2.bp.blogspot.com/-u2VJyYAUa8o/U2v9Unt3McI/AAAAAAAACaM/qWofwtOWwJM/s1600/capacity+planning.png)</div>

<span>We have seen how cloud architectures diverge from everything done in the past because it is necessarily respond to different scenarios. Many of the approaches used in the past are no longer usable and the fundamental concepts underlying architectures such as scalability, performance, reliability, and capacity planning are deeply reviewed. To summarize the golden rules for a correct design of a cloud architecture, we can definitely identify:</span>  

**Enable Scaling**<span> </span>_- be able to adapt to environmental conditions_  

**Expect Failure**_ - be able to adopt a resilient attitude_  

<span>Always keep in mind the above rules during the design and the implementation of real cloud architecture.</span>  

<span>I conclude here this long post, but in the future it could be interesting to analyze some design techniques typical of the cloud world.</span>  
<span>Have fun!</span>

</div>