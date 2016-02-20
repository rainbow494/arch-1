## [Is your cloud as scalable as you think it is?](/blog/2008/9/25/is-your-cloud-as-scalable-as-you-think-it-is.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, September 25, 2008 at 5:06AM</span></div>

<div class="body">An unstated assumption is that clouds are scalable. But are they? Stick thousands upon thousands of machines together and there are a lot of potential bottlenecks just waiting to choke off your scalability supply. And if the cloud is scalable what are the chances that your application is really linearly scalable? At 10 machines all may be well. Even at 50 machines the seas look calm. But at 100, 200, or 500 machines all hell might break loose. How do you know?  

You know through real life testing. These kinds of tests are brutally hard and complicated. who wants to do all the incredibly precise and difficult work of producing cloud scalability tests? [GridDynamics](http://griddynamics.com) has stepped up to the challenge and has just released their [Cloud Performance Reports](http://blog.griddynamics.com/2008/09/cloud-performance-reports.html#links).  

The report is quite detailed so I'll just cover what I found most interesting. GridDynamics in this report test three configurations:  

*   **[GridGain](http://highscalability.com/gridgain-one-compute-grid-many-data-grids) running a Monte-Carlo simulation on EC2**. This test is a CPU only test, a data grid is not accessed. This scenario tests the scalability of EC2 and GridGain.  
    * GridGain provided near linear scalability end-to-end on a 512 node EC2 hosted grid.  
    * EC2 is ready for production usage on large-scale stateless computations exhibiting good price for performance and a strong linear scaling curve.  
    *   **GigaSpaces running a risk management simulation on EC**2\. This is a data-driven test. GigaSpaces is used in a configuration where the compute grid and the data grid are separated, even though GigaSpaces supports an in-memory data grid.  
    * GigaSpaces provided near linear scalability from 16 to 256 nodes. There was a 28% degradation from 256 to 512 nodes because only four data grid servers were used. More were needed. The compute grid and data grid must each be sized to independently to scale properly.  
    * EC2 is ready for production usage for classes of large-scale data-driven applications.  
    *   **Windows HPC Server and Velocity running an analytics application in Microsoft's grid testbed**. This test was more complicated than the others. It tested the performance implications of data "in the cloud" vs. "outside the cloud" for data-intensive analytics applications.  
    * Keeping data close to the business logic matters. Performance improved up to 31 times over "outside the cloud."  
    * Velocity failed on 50 node clusters with 200 concurrent clients.  
    * Local caches provided significant performance gains over distributed caches. The local cache took load off the distributed cache.  

    They are currently running more tests with different configurations. Hopefully we'll see those results later.  

    All-in-all a generally optimistic report. EC2 scales. Mot of the tested grid frameworks scaled. What's also clear is it may take a while before deploying cloud based grids is an easy process. It still takes a lot of work to install, configure, start, stop, monitor, and debug bottlenecks in cloud based grids.  

    Thanks to GridDynamics for putting in all this work and I look forward to their next set of reports.  
    </div>