## [Egnyte Architecture: Lessons Learned in Building and Scaling a Multi Petabyte Distributed System](/blog/2016/2/15/egnyte-architecture-lessons-learned-in-building-and-scaling.html)

    

    

![](https://c2.staticflickr.com/2/1521/25002347816_f48153605c_o.png)

_    This is a guest post by     [    Kalpesh Patel    ](https://www.linkedin.com/in/kpatelatwork)    , an Architect, who works from home. He and his colleagues spends their productive hours scaling one of the largest distributed file-system out there. He works at     [    Egnyte    ](https://www.egnyte.com/)    , an Enterprise File Synchronization Sharing and Analytics startup and you can reach him at     [    @kpatelwork    ](https://twitter.com/kpatelwork)    .    _

    Your Laptop has a filesystem used by hundreds of processes, it is limited by the disk space, it can’t expand storage elastically, it chokes if you run few I/O intensive processes or try sharing it with 100 other users. Now take this problem and magnify it to a file-system used by millions of paid users spread across world and you get a roller coaster ride scaling the system to meet monthly growth needs and meeting SLA requirements.    

**Egnyte is an Enterprise File Synchronization and Sharing startup** founded in 2007, when Google drive wasn't born and AWS S3 was cost prohibitive. Our only option was to roll our sleeves and build an object store ourselves, overtime costs for S3 and GCS became reasonable and because our storage layer was based on a plugin architecture, we can now plug-in any storage backend that is cheaper. We have re-architected many of the core components multiple times and in this article I will try to share what is the current architecture and what are the lessons  we learned scaling it and what are the things we can still improve upon.

##     The Platform    

[Click to read more ...](/blog/2016/2/15/egnyte-architecture-lessons-learned-in-building-and-scaling.html)

    