## [Product: Gearman - Open Source Message Queuing System](/blog/2009/1/13/product-gearman-open-source-message-queuing-system.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, January 13, 2009 at 1:16AM</span></div>

<div class="body">

**Update:** [New Gearman Server & Library in C, MySQL UDFs](http://www.oddments.org/?p=30).  

[Gearman](http://gearman.org/) is an open source [message queuing](http://en.wikipedia.org/wiki/Message_queue) system that makes it easy to do distributed job processing using multiple languages. With Gearman you: _farm out work to other machines, dispatching function calls to machines that are better suited to do work, to do work in parallel, to load balance lots of function calls, to call functions between languages, spread CPU usage around your network_.  

Gearman is used by companies like LiveJournal, Yahoo!, and Digg. Digg, for example, runs 300,000 jobs a day through Gearman without any issues. Most large sites use something similar. Why would anyone ever even need a message queuing system?

Message queuing is a handy way to move work off your web servers (like image manipulation), to generate thousands of documents in the background, to run the multiple requests in parallel needed to build a web page, or to perform tasks that can comfortably be run in the background and not part of the main request loop for servicing a web request.  

There's a gearmand server and clients written in Perl, Ruby, Python or C. Use at least two gearmand server daemons for higher availability. The tasks each client can perform are registered with gearman distributes requests for those functions to the client that can implement them.  

Gearman uses a very robust, if somewhat higher latency, signal-and-pull architecture.

*   According to dormando the flow goes like:  
    * worker connects to all gearmand servers.  
    * worker registers what functions it supports.  
    * worker asks for jobs.  
    * if no jobs, sends command 'pre_sleep' to all gearmand's and sleeps.  

    *   Client does:  
    * Connect to gearmand.  
    * submit's a job for a particular func.  

    *   Gearmand does:  
    * Acks the job, finds all *sleeping workers* related to the function.  
    * Sends them all a 'noop' command to wake them up.  

    *   Worker does:  
    * Urk, I'm awake now.  
    * Worker asks for jobs.  
    * If jobs, do work.  
    * If no jobs, sends command 'pre_sleep' to all gearmand's, etc.  

    Gearman uses an efficient binary protocol and no XML. There's an a line-based text protocol for admin so you can use telnet and hook into Nagios plugins.  

    The system makes no guarantees. If there's a failure the client is told about the failure and the client is responsible for retries. And the queue isn’t persistent. If gearman is restarted the queue is gone.  

    ## Related Articles

    *   [Gearman Wiki](http://gearmanproject.org/doku.php)*   [German Google Groups](http://groups.google.com/group)*   [Queue everything and delight everyone](http://decafbad.com/blog/2008/07/04/queue-everything-and-delight-everyone) by Leslie Michael Orchard.*   [USENIX 2007](http://danga.com/words/2007_06_usenix/). Starts at slide 83.*   [PEAR and Gearman](http://clockwerx.blogspot.com/2008/04/pear-and-gearman.html) by Daniel O'Connor.*   [Amazon Architecture](http://highscalability.com/amazon-architecture)</div>