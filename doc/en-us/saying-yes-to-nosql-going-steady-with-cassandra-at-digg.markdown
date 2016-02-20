## [Saying Yes to NoSQL; Going Steady with Cassandra at Digg](/blog/2010/3/10/saying-yes-to-nosql-going-steady-with-cassandra-at-digg.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, March 10, 2010 at 4:42PM</span></div>

<div class="body">

The last six months have been exciting for Digg's engineering team. We're working on a soup-to-nuts rewrite. Not only are we rewriting all our application code, but we're also rolling out a new client and server architecture. And if that doesn't sound like a big enough challenge, we're replacing most of our infrastructure components and moving away from LAMP.

Perhaps our most significant infrastructure change is abandoning MySQL in favor of a NoSQL alternative. To someone like me who's been building systems almost exclusively on relational databases for almost 20 years, this feels like a bold move.

### What's Wrong with MySQL?

Our primary motivation for moving away from MySQL is the increasing difficulty of building a high performance, write intensive, application on a data set that is growing quickly, with no end in sight. This growth has forced us into horizontal and vertical partitioning strategies that have eliminated most of the value of a relational database, while still incurring all the overhead.

Relational database technology can be a blunt instrument and we're motivated to find a tool that matches our specific needs closely. Our domain area, news, doesn't exact strict consistency requirements, so (according to [Brewer's theorem](http://en.wikipedia.org/wiki/CAP_theorem)) relaxing this allows gains in availability and partition tolerance (i.e. operations completing, even in degraded system states). We're confident that our engineers can implement application level consistency controls much more efficiently than MySQL does generically.

As our system grows, it's important for us to span multiple data centers for redundancy and network performance and to add capacity or replace failed nodes with no downtime. We plan to continue using commodity hardware, and to continue assuming that it will fail regularly. All of this is increasingly difficult with MySQL.

<div id="_mcePaste">[Read the Full Digg Blog](http://about.digg.com/node/564)</div>

</div>