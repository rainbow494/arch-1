## [A Short on the Pinterest Stack for Handling 3+ Million Users](/blog/2012/2/16/a-short-on-the-pinterest-stack-for-handling-3-million-users.html)

    

    

![](http://farm8.staticflickr.com/7180/6886606039_bc5c70dbf9_m.jpg)

Pinterest co-founder Paul Sciarra [shared a bit about their stack](http://www.quora.com/Pinterest/What-technologies-were-used-to-make-Pinterest) on Quora:

*   Python + heavily-modified Django at the application layer
*   Tornado and (very selectively) node.js as web-servers.  
*   Memcached and membase / redis for object- and logical-caching, respectively.  
*   RabbitMQ as a message queue.  
*   Nginx, HAproxy and Varnish for static-delivery and load-balancing.
*   Persistent data storage using MySQL.  
*   MrJob on EMR for map-reduce.
*   Git.

Alex Popescu has [created a cool diagram](http://nosql.mypopescu.com/post/17658415847/polyglot-persistence-at-pinterest-redis-membase) of the setup and provided some thoughtful analysis as well.

    