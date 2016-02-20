## [Sean Hull's 20 Biggest Bottlenecks that Reduce and Slow Down Scalability](/blog/2013/8/28/sean-hulls-20-biggest-bottlenecks-that-reduce-and-slow-down.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, August 28, 2013 at 9:09AM</span></div>

<div class="body">

![](http://farm4.staticflickr.com/3671/9616263004_ce0845b560_m.jpg)

_This article is a lightly edited version of [20 Obstacles to Scalability](http://queue.acm.org/detail.cfm?id=2512489) by [Sean Hull](https://twitter.com/hullsean) (__with permission) from the always excellent and thought provoking [ACM Queue](http://queue.acm.org/)_.

### 1\. TWO-PHASE COMMIT

Normally when data is changed in a database, it is written both to memory and to disk. When a commit happens, a relational database makes a commitment to freeze the data somewhere on real storage media. Remember, memory doesn't survive a crash or reboot. Even if the data is cached in memory, the database still has to write it to disk. MySQL binary logs or Oracle redo logs fit the bill.

With a MySQL cluster or distributed file system such as DRBD (Distributed Replicated Block Device) or Amazon Multi-AZ (Multi-Availability Zone), a commit occurs not only locally, but also at the remote end. A two-phase commit means waiting for an acknowledgment from the far end. Because of network and other latency, those commits can be slowed down by milliseconds, as though all the cars on a highway were slowed down by heavy loads. For those considering using Multi-AZ or read replicas, the Amazon RDS (Relational Database Service) use-case comparison at [http://www.iheavy.com/2012/06/14/rds-or-mysql-ten-use-cases/](http://www.iheavy.com/2012/06/14/rds-or-mysql-ten-use-cases/) will be helpful.

Synchronous replication has these issues as well; hence, MySQL's solution is semi-synchronous, which makes some compromises in a real two-phase commit.

### 2\. INSUFFICIENT CACHING

Caching is very important at all layers, so where is the best place to cache: at the browser, the page, the object, or the database layer? Let's work through each of these.

_Browser caching_<span> might seem out of reach, until you realize that the browser takes directives from the Web server and the pages it renders. Therefore, if the objects contained therein have longer expire times, the browser will cache them and will not need to fetch them again. This is faster not only for the user, but also for the servers hosting the Web site, as all returning visitors will weigh less.</span>

<span><span>More information about browser caching is available at </span>[http://www.iheavy.com/2011/11/01/5-tips-cache-websites-boost-speed/](http://www.iheavy.com/2011/11/01/5-tips-cache-websites-boost-speed/)<span>. Be sure to set expire headers and cache control.</span></span>

<span>_Page caching_ requires using a technology such as Varnish (https://www.varnish-cache.org/). Think of this as a mini Web server with high speed and low overhead. It can't handle as complex pages as Apache can, but it can handle the very simple ones better. It therefore sits in front of Apache and reduces load, allowing Apache to handle the more complex pages. This is like a traffic cop letting the bikes go through an intersection before turning full attention to the more complex motorized vehicles.</span>

<span>_Object caching_ is done by something like memcache. Think of it as a Post-it note for your application. Each database access first checks the object cache for current data and answers to its questions. If it finds the data it needs, it gets results 10 to 100 times faster, allowing it to construct the page faster and return everything to the user in the blink of an eye. If it doesn't find the data it needs, or finds only part of it, then it will make database requests and put those results in memcache for later sessions to enjoy and benefit from.</span>

### <span>3\. SLOW DISK I/O, RAID 5, MULTITENANT STORAGE</span>

<span>Everything, everything, everything in a database is constrained by storage—not by the size or space of that storage but by how fast data can be written to those devices.</span>

<span>If you're using physical servers, watch out for RAID 5, a type of RAID (redundant array of independent disks) that uses one disk for both parity and protection. It comes with a huge write penalty, however, which you must always carry. What's more, if you lose a drive, these arrays are unusably slow during rebuild.</span>

<span>The solution is to start with RAID 10, which gives you striping over mirrored sets. This results in no parity calculation and no penalty during a rebuild.</span>

<span>Cloud environments may work with technology such as Amazon EBS (Elastic Block Store), a virtualized disk similar to a storage area network. Since it's network based, you must contend and compete with other tenants (aka customers) reading and writing to that storage. Further, those individual disk arrays can handle only so much reading and writing, so your neighbors will affect the response time of your Web site and application.</span>

<span>Recently Amazon rolled out a badly branded offering called Provisioned IOPS (I/O operations per second). That might sound like a great name to techies, but to everyone else it doesn't mean anything noteworthy. It's nonetheless important. It means you can lock in and guarantee the disk performance your database is thirsty for. If you're running a database on Amazon, then definitely take a look at this.</span>

### <span>4\. SERIAL PROCESSING</span>

<span>When customers are waiting to check out in a grocery store with 10 cash registers open, that's working in parallel. If every cashier is taking a lunch break and only one register is open, that's serialization. Suddenly a huge line forms and snakes around the store, frustrating not only the customers checking out, but also those still shopping. It happens at bridge tollbooths when not enough lanes are open, or in sports arenas when everyone is leaving at the same time.</span>

<span>Web applications should definitely avoid serialization. Do you see a backup waiting for API calls to return, or are all your Web nodes working off one search server? Anywhere your application forms a line, that's serialization and should be avoided at all costs.</span>

### <span>5\. MISSING FEATURE FLAGS</span>

<span>Developers normally build in features and functionality for business units and customers. Feature flags are operational necessities that allow those features to be turned on or off in either back-end config files or administration UI pages.</span>

<span>Why are they so important? If you have ever had to put out a fire at 4 a.m., then you understand the need for contingency plans. You must be able to disable ratings, comments, and other auxiliary features of an application, just so the whole thing doesn't fall over. What's more, as new features are rolled out, sometimes the kinks don't show up until a horde of Internet users hit the site. Feature flags allow you to disable a few features, without taking the whole site offline.</span>

### <span>6\. SINGLE COPY OF THE DATABASE</span>

<span>You should always have at least one read replica or MySQL slave online. This allows for faster recovery in the event that the master fails, even if you're not using the slave for browsing—but you should do that, too, since you're going to build a browse-only mode, right?</span>

<span>Having multiple copies of a database suggests horizontal scale. Once you have two, you'll see how three or four could benefit your infrastructure.</span>

### <span>7\. USING YOUR DATABASE FOR QUEUING</span>

<span>A MySQL database server is great at storage tables or data, and relationships between them. Unfortunately, it's not great at serving as a queue for an application. Despite this, a lot of developers fall into the habit of using a table for this purpose. For example, does your app have some sort of jobs table, or perhaps a status column, with values such as "in-process," "in-queue," and "finished"? If so, you're inadvertently using tables as queues.</span>

Such solutions run into scalability hang-ups because of locking challenges and the scan and poll process to find more work. They will typically slow down a database. Fortunately, some good open source solutions are available, such as RabbitMQ ([http://www.rabbitmq.com/](http://www.rabbitmq.com/)) or Amazon's SQS (Simple Queue Service;[http://aws.amazon.com/sqs/](http://aws.amazon.com/sqs/)).

### 8\. USING A DATABASE FOR FULL-TEXT SEARCHING

Page searching is another area where applications get caught. Although MySQL has had full-text indexes for some time, they have worked only with MyISAM tables, the legacy table type that is not crash-proof, not transactional, and just an all-around headache for developers. 

<div id="_mcePaste">One solution is to go with a dedicated search server such as Solr (http://lucene.apache.org/solr/). These servers have good libraries for whatever language you're using and high-speed access to search. These nodes also scale well and won't bog down your database.</div>

<div>Alternatively, Sphinx SE, a storage engine for MySQL, integrates the Sphinx server right into the database. If you're looking on the horizon, Fulltext is coming to InnoDB, MySQL's default storage engine, in version 5.6 of MySQL.</div>

### 9\. OBJECT RELATIONAL MODELS

The ORM (object relational model), the bane of every Web company that has ever used it, is like cooking with MSG. Once you start using it, it's hard to wean yourself off.

The plus side is that ORMs help with rapid prototyping and allow developers who aren't SQL masters to read and write to the database as objects or memory structures. They are faster, cleaner, and offer quicker delivery of functionality—until you roll out on servers and want to scale.

Then your DBA (database administrator) will come to the team with a very slow-running, very ugly query and say, "Where is this in the application? We need to fix it. It needs to be rewritten." Your dev team then says, "We don't know!" And an incredulous look is returned by the ops team.

The ability to track down bad SQL and root it out is essential. Bad SQL will happen, and your DBA team will need to index properly. If queries are coming from ORMs, they don't lend themselves to all of this. Then you're faced with a huge technical debt and the challenge of ripping and replacing.

### 10\. MISSING INSTRUMENTATION

Instrumentation provides speedometers and fuel needles for Web applications. You wouldn't drive a car without them, would you? They expose information about an application's internal workings. They record timings and provide feedback about where an application spends most of its time.

One very popular Web services solution is New Relic (http://newrelic.com/), which provides visual dashboards that appeal to everyone—project managers, developers, the operations team, and even business units can all peer at the graphs and see what's happening.

Some open source instrumentation projects are also available.

### 11\. LACK OF A CODE REPOSITORY AND VERSION CONTROL

Though it's rare these days, some Internet companies do still try to build software without version control. Those who use it, however, know the everyday advantage and organizational control it provides for a team.

If you're not using it, you are going to spiral into technical debt as your application becomes more complex. It won't be possible to add more developers and work on different parts of your architecture and scaffolding.

Once you starting using version control, be sure to get all the components in there, including configuration files and other essentials. Missing pieces that have to be located and tracked down at deployment time become an additional risk.

### 12\. SINGLE POINTS OF FAILURE

If your data is on a single master database, that's a single point of failure. If your server is sitting on a single disk, that's a single point of failure. This is just technical vernacular for an Achilles heel.

<span style="color: #333333; font-family: Verdana, Arial, sans-serif; line-height: 15px;">These single points of failure must be rooted out at all costs. The trouble is recognizing them. Even relying on a single cloud provider can be a single point of failure. Amazon's data center or zone failures are a case in point. If it had had multiple providers or used Amazon differently, AirBNB would not have experienced downtime when part of Amazon Web Services went down in October 2012</span>([http://www.iheavy.com/2012/10/23/airbnb-didnt-have-to-fail/](http://www.iheavy.com/2012/10/23/airbnb-didnt-have-to-fail/)).

### 13\. LACK OF BROWSE-ONLY MODE

If you've ever tried to post a comment on Yelp, Facebook, or Tumblr late at night, you've probably gotten a message to the effect of, "This feature isn't available. Try again later." "Later" might be five minutes or 60 minutes. Or maybe you're trying to book airline tickets and you have to retry a few times. To nontechnical users, the site still appears to be working normally, but it just has this strange blip.

What's happening here is that the application is allowing you to browse the site, but not make any changes. It means the master database or some storage component is offline.

Browse-only mode is implemented by keeping multiple read-only copies of the master database, using a method such as MySQL replication or Amazon read replicas. Since the application will run almost fully in browse mode, it can hit those databases without the need for the master database. This is a big, big win.

### 14\. WEAK COMMUNICATION

Communication may seem a strange place to take a discussion on scalability, but the technical layers of Web applications cannot be separated from the social and cultural ones that the team navigates.

Strong lines of communication are necessary, and team members must know whom to go to when they're in trouble. Good communication demands confident and knowledgeable leadership, with the openness to listen and improve.

### 15\. LACK OF DOCUMENTATION

Documentation happens at a lot of layers in a Web application. Developers need to document procedures, functions, and pages to provide hints and insight to future generations looking at that code. Operations teams need to add comments to config files to provide change history and insight when things break. Business processes and relationships can and should be documented in company wikis to help people find their own solutions to problems.

Documentation helps at all levels and is a habit everyone should embrace.

### 16\. LACK OF FIRE DRILLS

Fire drills always get pushed to the backburner. Teams may say, "We have our backups; we're covered." True, until they try to restore those backups and find that they're incomplete, missing some config file or crucial piece of data. If that happens when you're fighting a real fire, then something you don't want will be hitting that office fan.

Fire drills allow a team to run through the motions, before they really need to. Your company should task part of its ops team with restoring its entire application a few times a year. With AWS and cloud servers, this is easier than it once was. It's a good idea to spin up servers just to prove that all your components are being backed up. In the process you will learn how long it takes, where the difficult steps lie, and what to look out for.

### 17\. INSUFFICIENT MONITORING AND METRICS

Monitoring falls into the same category of best practices as version control: it should be so basic you can't imagine working without it; yet there are Web shops that go without, or with insufficient monitoring—some server or key component is left out

Collecting this data over time for system and server-level data, as well as application and business- level availability, are equally important. If you don't want to roll your own, consider a Web services solution to provide your business with real uptime.

### 18\. COWBOY OPERATIONS

You roll into town on a fast horse, walk into the saloon with guns blazing, and you think you're going to make friends? Nope, you're only going to scare everyone into complying but with no real loyalty. That's because you'll probably break things as often as you fix them. Confidence is great, but it's best to work with teams. The intelligence of the team is greater than any of the individuals.

Teams need to communicate what they're changing, do so in a managed way, plan for any outage, and so forth. Caution and risk aversion win the day. Always have a plan B. You should be able to undo the change you just made, and be aware which commands are destructive and which ones cannot be undone.

### 19\. GROWING TECHNICAL DEBT

As an app evolves over the years, the team may spend more and more time maintaining and supporting old code, squashing bugs, or ironing out kinks. Therefore, they have less time to devote to new features. This balance of time devoted to debt servicing versus real new features must be managed closely. If you find your technical debt increasing, it may be time to bite the bullet and rewrite. Rewriting will take time away from the immediate business benefit of new functionality and customer features, but it is best for the long term.

<span style="color: #333333; font-family: Verdana, Arial, sans-serif; line-height: 15px;">Technical debt isn't always easy to recognize or focus on. As you're building features or squashing bugs, you're more attuned to details at the </span>five-foot<span style="color: #333333; font-family: Verdana, Arial, sans-serif; line-height: 15px;"> level. It's easy to miss the forest for the trees. That's why generalists are better at scaling the Web (</span>[http://www.iheavy.com/2011/10/25/why-generalists-better-scaling-web/](http://www.iheavy.com/2011/10/25/why-generalists-better-scaling-web/)<span style="color: #333333; font-family: Verdana, Arial, sans-serif; line-height: 15px;">).</span>

### <span style="font-family: Verdana, Arial, sans-serif; color: #333333;"><span style="line-height: 15px;">20\. INSUFFICIENT LOGGING</span></span>

<span style="font-family: Verdana, Arial, sans-serif; color: #333333;"><span style="line-height: 15px;">Logging is closely related to metrics and monitoring. You may enable a lot more of it when you're troubleshooting and debugging, but on an ongoing basis you'll need it for key essential services. Server syslogs, Apache and MySQL logs, caching logs, etc., should all be working. You can always dial down logging if you're getting too much of it, or trim and rotate log files, discarding the old ones.</span></span>

</div>