## [Google Paper: Large-scale Incremental Processing Using Distributed Transactions and Notifications](/blog/2010/10/1/google-paper-large-scale-incremental-processing-using-distri.html)

    

    

This paper, [Large-scale Incremental Processing Using Distributed Transactions and Notifications](http://research.google.com/pubs/pub36726.html#) by Daniel Peng and Frank Dabek, is Google's much anticipated description of Percolator, their new [real-time indexing](http://highscalability.com/blog/2010/9/11/googles-colossus-makes-search-real-time-by-dumping-mapreduce.html) system.

The abstract:

>     Updating an index of the web as documents are crawled requires continuously transforming a large repository of existing documents as new documents arrive. This task is one example of a class of data processing tasks that transform a large repository of data via small, independent mutations. These tasks lie in a gap between the capabilities of existing infrastructure. Databases do not meet the storage or throughput requirements of these tasks: Google’s indexing system stores tens of petabytes of data and processes billions of updates per day on thousands of machines. MapReduce and other batch-processing systems cannot process small updates individually as they rely on creating large batches for efﬁciency.
> 
>     We have built Percolator, a system for incrementally processing updates to a large data set, and deployed it to create the Google web search index. By replacing a batch-based indexing system with an indexing system based on incremental processing using Percolator, we process the same number of documents per day, while reducing the average age of documents in Google search results by 50%.     
> 
>     

    