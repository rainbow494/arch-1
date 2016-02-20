## [Learn How to Think at Scale](/blog/2009/7/30/learn-how-to-think-at-scale.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, July 30, 2009 at 3:10PM</span></div>

<div class="body">

Aaron Kimball of Cloudera gives a wonderful 23 minute presentation titled _Cloudera Hadoop Training: Thinking at Scale Cloudera_ which talks about "common challenges and general best practices for scaling with your data." As a company Cloudera offers "enterprise-level support to users of Apache Hadoop." Part of that offering is a really useful series of [tutorial videos on the Hadoop ecosystem](http://www.cloudera.com/hadoop-training).  

Like TV lawyer Perry Mason (or is it Harmon Rabb?), Aaron gradually builds his case. He opens with the problem of storing lots of data. Then a blistering cross examination of the problem of building distributed systems to analyze that data sets up a powerful closing argument. With so much testimony behind him, on closing Aaron really brings it home with why shared nothing systems like map-reduce are the right solution on how to query lots of data. They jury loved it.  

Here's the video [Thinking at Scale](http://www.cloudera.com/content/www/en-us/resources/training/thinking-at-scale.html). And here's a summary of some of the lessons learned from the talk:

## Lessons Learned

*   We can process data much faster than we can read it and much faster than we can write results back to disk.  
    * Say 32 GB of RAM available to a machine. You can get 1-2 TB of data on disk. The amount of data a machine can store is greater than the amount it can manipulate in memory so you have to swap out RAM.  
    * With an average job size of 180 GB it would take 45 minutes to read that data off of disk sequentially. Random access would be much slower.  
    * An individual SATA drive can read at 75 MB/sec. To process 180 GB you would have to read at 75 MB/sec which leaves the CPU doing very little with the data.*   The solution is to parallelize the reads. Have a 1000 hard drives working together and you can read 75 GB/sec.*   With a parallel system in place the next step is to move computation to where the data is already stored.  
    * Grids moved data to computation. Data was typically stored on a large filer/SAN.  
    * The new large scale computing approach is to move computation to where the data is already stored. A file has limited processing power relative to storage size so not useful.  
    * So move processing to individual nodes that store only a small amount of the data at a time.  
    * This gets around implementation complexity and bandwidth limitations of a centralized filer. Distributed systems can drown themselves if they start sharing data.*   Large distributed systems must be able to support partial failure and adapt to new additional capacity.  
    * Failure with large systems is inevitable so partial progress must be kept for long jobs and jobs must be restarted when a failure is detected. Complex distributed systems make job restarting difficult because of the state that must be maintained.  
    * Processing should be close to linear with the number of nodes. Losing 5% of nodes should not end up with a 50% loss in throughput. Doubling the size of the cluster should double the number of jobs that can be processed. No job should be able to nuke the system.  
    * Workload should be transferred as new nodes are added and failures occur.  
    * Node changes (failures, additions, new hard drives, more memory, etc) should be transparent to jobs. Users shouldn't have to deal with changes, the system should handle them transparently.*   Solution to large scale data processing problems is to build a shared nothing architecture.  
    * To get around the limits faced by[MPI](http://en.wikipedia.org/wiki/Message_Passing_Interface) (message passing interface) based systems nothing is shared.  
    * In map-reduce (MR) systems data is read locally and processed locally. Results are written back locally.  
    * Nodes do not talk to each other.  
    * Data is paritioned onto machines in advance and computations happen where data is stored.  
    * In MPI communication is explicit. Programs know who they are talking to and what they are talking about. In MR communication is implicit. It is taken care of by the system. Data is routed where it needs to go. This simplifies programs by removing the complexity of explicit coordination. Allows developers to concentrate on solving their problem without know level network stack and programming details.  
    * On multi-core computers each core would be treated as a separate node.  
    * Goal is to have locality of reference. Tasks are processed on the same node as where the data is stored or at least on the same rack. This removes a load step. The data isn't loaded onto a filer. It's not then loaded onto a processing machines. It's already where spreat around the cluster where it needs to be used upfront.  
    * In standard MR it's processing large files of data, typically 1 GB or more. This allows streaming reads from this disk. Typical file system block sizes are 4K, for MR they are 64MB to 256MB, which allows writing large linear chunks which reduces seeks on reading.  
    * Tasks are restarted transparently on failure because tasks are independent of each other.  
    * Data is replicated across nodes for fault tolerance purposes.  
    * Task independence allows speculative task execution. The same task can be started on difference nodes and the fastest result can be used. This allows problems like broken disk controllers to be worked around.  
    * If necessary inputs can be processed on another machine. There's a big penalty for going off node and off rack.  
    * Nodes have no identity to the programmer. Nodes can run multiple jobs.  

    ## Related Articles

    *   [Researchers: Databases still beat Google's MapReduce](http://www.computerworld.com/s/article/9131526/Researchers_Databases_still_beat_Google_s_MapReduce) by Eric Lai*   [Relational Database Experts Jump The MapReduce Shark](http://typicalprogrammer.com/programming/mapreduce/) by Greg Jorgensen*   [Hadoop and HBAse vs RDBMS](http://www.docstoc.com/docs/2996433/Hadoop-and-HBase-vs-RDBMS) by Jonathan Gray*   [Database Technology for the Web: Part 1 – The MapReduce Debate](http://www.b-eye-network.com/view/10786#) by Colin White</div>