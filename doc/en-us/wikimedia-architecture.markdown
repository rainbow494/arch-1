## [Wikimedia architecture](/blog/2007/8/22/wikimedia-architecture.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, August 22, 2007 at 9:56AM</span></div>

<div class="body">

Wikimedia is the platform on which Wikipedia, Wiktionary, and the other seven wiki dwarfs are built on. This document is just excellent for the student trying to scale the heights of giant websites. It is full of details and innovative ideas that have been proven on some of the most used websites on the internet.

Site: http://wikimedia.org/

## Information Sources

*   [Wikimedia architecture](http://www.nedworks.org/~mark/presentations/san/Wikimedia%20architecture.pdf)*   http://meta.wikimedia.org/wiki/Wikimedia_servers*   [scale-out vs scale-up](http://oracle2mysql.wordpress.com/2007/08/22/12/) in the _from Oracle to MySQL_ blog.  

    ## Platform

    *   Apache*   Linux*   MySQL*   PHP*   Squid*   LVS*   Lucene for Search*   Memcached for Distributed Object Cache*   Lighttpd Image Server  

    ## The Stats

    *   8 million articles spread over hundreds of language projects (english, dutch, ...)*   10th busiest site in the world (source: Alexa)*   Exponential growth: doubling every 4-6 months in terms of visitors / traffic / servers*   30 000 HTTP requests/s during peak-time*   3 Gbit/s of data traffic*   3 data centers: Tampa, Amsterdam, Seoul*   350 servers, ranging between 1x P4 to 2x Xeon Quad-Core, 0.5 - 16 GB of memory*   managed by ~ 6 people*   3 clusters on 3 different continents  

    ## The Architecture

    *   Geographic Load Balancing, based on source IP of client resolver, directs clients to the nearest server cluster. Statically mapping IP addresses to countries to clusters*   HTTP reverse proxy caching implemented using Squid, grouped by text for wiki content and media for images and large static files.*   55 Squid servers currently, plus 20 waiting for setup.*   1,000 HTTP requests/s per server, up to 2,500 under stress*   ~ 100 - 250 Mbit/s per server*   ~ 14 000 - 32 000 open connections per server*   Up to 40 GB of disk caches per Squid server*   Up to 4 disks per server (1U rack servers)*   8 GB of memory, half of that used by Squid*   Hit rates: 85% for Text, 98% for Media, since the use of CARP.*   PowerDNS provides geographical distribution.*   In their primary and regional data center they build text and media clusters built on LVS, CARP Squid, Cache Squid. In the primary datacenter they have the media storage.*   To make sure the latest revision of all pages are served invalidation requests are sent to all Squid caches.*   One centrally managed & synchronized software installation for hundreds of wikis.*   MediaWiki scales well with multiple CPUs, so we buy dual quad-core servers now (8 CPU cores per box)*   Hardware shared with External Storage and Memcached tasks*   Memcached is used to cache image metadata, parser data, differences, users and sessions, and revision text. Metadata, such as article revision history, article relations (links, categories etc.), user accounts and settings are stored in the core databases*   Actual revision text is stored as blobs in External storage*   Static (uploaded) files, such as images, are stored separately on the image server - metadata (size, type, etc.) is cached in the core database and object caches*   Separate database per wiki (not separate server!)*   One master, many replicated slaves*   Read operations are load balanced over the slaves, write operations go to the master*   The master is used for some read operations in case the slaves are not yet up to date (lagged)*   External Storage  
    - Article text is stored on separate data storage clusters, simple append-only blob storage. Saves space on expensive and busy core databases for largely unused data  
    - Allows use of spare resources on application servers (2x  
    250-500 GB per server)  
    - Currently replicated clusters of 3 MySQL hosts are used;  
    this might change in the future for better manageability  

    ## Lessons Learned

    *   Focus on architecture, not so much on operations or nontechnical stuff.  

    *   Sometimes caching costs more than recalculating or looking up at the  
    data source...profiling!  

    *   Avoid expensive algorithms, database queries, etc.  

    *   Cache every result that is expensive and has temporal locality of reference.  

    *   Focus on the hot spots in the code (profiling!).  

    *   Scale by separating:  
    - Read and write operations (master/slave)  
    - Expensive operations from cheap and more frequent operations (query groups)  
    - Big, popular wikis from smaller wikis  

    *   Improve caching: temporal and spatial locality of reference and reduces the data set size per server  

    *   Text is compressed and only revisions between articles are stored.  

    *   Simple seeming library calls like using stat to check for a file's existence can take too long when loaded.  

    *   Disk seek I/O limited, the more disk spindles, the better!  

    *   Scale-out using commodity hardware doesn't require using cheap hardware. Wikipedia's database servers these days are 16GB dual or quad core boxes with 6 15,000 RPM SCSI drives in a RAID 0 setup. That happens to be the sweet spot for the working set and load balancing setup they have. They would use smaller/cheaper systems if it made sense, but 16GB is right for the working set size and that drives the rest of the spec to match the demands of a system with that much RAM. Similarly the web servers are currently 8 core boxes because that happens to work well for load balancing and gives good PHP throughput with relatively easy load balancing.  

    *   It is a lot of work to scale out, more if you didn't design it in originally. Wikipedia's MediaWiki was originally written for a single master database server. Then slave support was added. Then partitioning by language/project was added. The designs from that time have stood the test well, though with much more refining to address new bottlenecks.  

    *   Anyone who wants to design their database architecture so that it'll allow them to inexpensively grow from one box rank nothing to the top ten or hundred sites on the net should start out by designing it to handle slightly out of date data from replication slaves, know how to load balance to slaves for all read queries and if at all possible to design it so that chunks of data (batches of users, accounts, whatever) can go on different servers. You can do this from day one using virtualisation, proving the architecture when you're small. It's a LOT easier than doing it while load is doubling every few months!</div>