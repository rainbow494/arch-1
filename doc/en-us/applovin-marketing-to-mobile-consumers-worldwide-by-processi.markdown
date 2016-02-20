## [AppLovin: Marketing to Mobile Consumers Worldwide by Processing 30 Billion Requests a Day](/blog/2015/3/9/applovin-marketing-to-mobile-consumers-worldwide-by-processi.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, March 9, 2015 at 8:56AM</span></div>

<div class="body">

![](https://farm9.staticflickr.com/8570/16442530119_470a4487ee_m.jpg)

_This is a guest post from [AppLovin](http://www.applovin.com/)'s VP of engineering, [Basil Shikin](https://www.linkedin.com/in/basilshikin), on the infrastructure of its mobile marketing platform. Major brands like Uber, Disney, Yelp and Hotels.com use AppLovin's mobile marketing platform. It processes 30 billion requests a day and 60 terabytes of data a day._

<span>AppLovin's marketing platform provides marketing automation and analytics for brands who want to reach their consumers on mobile. The platform enables brands to use real-time data signals to make effective marketing decisions across one billion mobile consumers worldwide.</span>

# <span>Core Stats</span>

*   <span>30 Billion ad requests per day</span>

*   <span>300,000 ad requests per second, peaking at 500,000 ad requests per second</span>

*   <span>5ms average response latency</span>

*   <span>3 Million events per second</span>

*   <span>60TB of data processed daily</span>

*   <span>~1000 servers</span>

*   <span>9 data centers</span>

*   <span>~40 reporting dimensions</span>

*   <span>500,000 metrics data points per minute</span>

*   <span>1 Pb Spark cluster</span>

*   <span>15GB/s peak disk writes across all servers</span>

*   <span>9GB/s peak disk reads across all servers</span>

*   <span>Founded in 2012, AppLovin is headquartered in Palo Alto, with offices in San Francisco, New York, London and Berlin.</span>

# <span>Technology Stack</span>

## <span>Third Party Services</span>

*   [<span>Github</span>](https://github.com/) <span>for code</span>

*   [<span>Asana</span>](https://asana.com/) <span>for project management</span>

*   [<span>HipChat</span>](https://www.hipchat.com/) <span>for communication</span>

## <span>Data Storage</span>

*   [<span>Aerospike</span>](http://www.aerospike.com/) <span>for user profile storage</span>

*   [<span>Vertica</span>](http://www.vertica.com/) <span>for aggregated statistics and real-time reporting</span>

*   <span>Aggregating 350,000 rows per second and writing to Vertica at 34,000 rows per second</span>

*   <span>Peak 12,000 user profiles per second written to Aerospike</span>

*   <span>MySQL for ad data</span>

*   [<span>Spark</span>](https://spark.apache.org/) <span>for offline processing and deep data analysis</span>

*   [<span>Redis</span>](http://redis.io/) <span>for basic caching</span>

*   [<span>Thrift</span>](https://thrift.apache.org/) <span>for all data storage and transfers</span>

*   <span>Each data point replicated in 4 data centers</span>

*   <span>Each service is replicated at least in 2 data centers (at most in 8)</span>

*   <span>Amazon Web Services used for long term data storage and backups</span>

## <span>Core App And Services</span>

*   <span>Custom C/C++ Nginx module for high performance ad serving</span>

*   <span>Java for data processing and auxiliary services</span>

*   <span>PHP / Javascript for UI</span>

*   [<span>Jenkins</span>](http://jenkins-ci.org/) <span>for continuous integration and deployment</span>

*   <span>Zookeeper for distributed locking</span>

*   <span>HAProxy and IPVS for high availability</span>

*   <span>Coverity for Java/C++ static code analysis</span>

*   <span>Checkstyle and PMD for PHP static code analysis</span>

*   <span>Syslog for DC-centralized log server</span>

*   <span>Hibernate for transaction-based services</span>

## <span>Servers and Provisioning</span>

*   <span>Ubuntu</span>

*   <span>Cobbler for bare metal provisioning</span>

*   <span>Chef for configuring servers</span>

*   <span>Berkshelf for Chef dependencies</span>

*   <span>Docker with Test Kitchen for running infrastructure tests</span>

*   <span><span>A combination of software (ipvs, haproxy) and hardware load balancers. Plan to gradually move away from traditional hardware load balancers.</span></span>

# <span>Monitoring Stack</span>

## <span>Server Monitoring</span>

*   [<span>Icinga</span>](https://www.icinga.org/) <span>for all servers</span>

*   <span>~100 custom Nagios plugins for deep server monitoring</span>

*   <span>550 various probes per server</span>

*   [<span>Graphite</span>](http://graphite.wikidot.com/) <span>as data format</span>

*   [<span>Grafana</span>](http://grafana.org/) <span>for displaying all graphs</span>

*   [<span>PagerDuty</span>](http://www.pagerduty.com/) <span>for issue escalation</span>

*   [<span>Smokeping</span>](http://oss.oetiker.ch/smokeping/) <span>for network mesh monitoring</span>

## <span>Application Monitoring</span>

*   [<span>VividCortex</span>](https://vividcortex.com/) <span>for MySQL monitoring</span>

*   <span>JSON /health endpoint on each service</span>

*   <span>Cross-DC database consistency monitoring</span>

*   <span>9 4K 65” TVs for showing all graphs across the office</span>

*   <span>Statistical deviation monitoring</span>

*   <span>Fraudulent users monitoring</span>

*   <span>Third-party systems monitoring</span>

*   <span>Deployments are recorded in all graphs</span>

## <span>Intelligent Monitoring</span>

*   <span>Intelligent alerting system with a feedback loop: a system that can introspect anything can learn anything</span>

*   <span>Third-party stats about AppLovin are also monitored</span>

*   <span>Alerting is a cross-team exercise: developers, ops, business, data scientists are involved</span>

# <span>Architecture Overview</span>

## <span>General Considerations</span>

*   <span>Store everything in RAM</span>

*   <span>If it does not fit, save it to SSD</span>

*   <span>L2 cache level optimizations matter</span>

*   <span>Use right tool for the right job</span>

*   <span>Architecture allows swapping any component</span>

*   <span>Upgrade only if an alternative is 10x better</span>

*   <span>Write your own components if there is nothing suitable out there</span>

*   <span>Replicate important data at least 3x</span>

*   <span>Make sure every message can be re-played without data corruption</span>

*   <span>Automate everything</span>

*   <span>Zero-copy message passing</span>

## <span>Message Processing</span>

*   <span>Custom message processing system that guarantees message delivery</span>

*   <span>3x replication for each message</span>

*   <span>Sending a message = writing to disk</span>

*   <span>Any service may fail, but no data are lost</span>

*   <span>Message dispatching system connects all components together, provides isolation and extensibility of the system</span>

*   <span>Cross-DC failure tolerance</span>

## <span>Ad Serving</span>

*   <span>Nginx is really fast: can serve an ad in less than a millisecond</span>

*   <span>Keep all ad serving data in memory: read only</span>

*   <span>jemalloc gave a 30% speed improvement</span>

*   <span>Use Aerospike for user profiles: less than 1ms to fetch a profile</span>

*   <span>Pre-compute all ad serving data on one box and dispatch across all servers</span>

*   <span>Torrents are used to propagate serving data across all servers. Using Torrents resulted in 83% network load drop on the originating server compared to HTTP-based distribution.</span>

*   <span>mmap is used to share ad serving data across nginx processes</span>

*   <span>XXHash is the fastest hash function with a low collision rate. 75x faster than SHA-1 for computing checksums</span>

*   <span>5% of real traffic goes to staging environment</span>

*   <span>Ability to run 3 A/B tests at once (20%/20%/10% of traffic for three separate tests, 50% for control)</span>

*   <span>A/B test results are available in regular reporting</span>

# <span>Data Warehouse</span>

*   <span>All data are replicated</span>

*   <span>Running most reports takes under 2 seconds</span>

*   <span>Aggregation is key to allow fast reports on large amounts of data</span>

*   <span>Non-aggregated data for the last 48 hours is usually to resolve most issues</span>

*   <span>7 days of raw logs is usually enough for debug</span>

*   <span>Some reports must be pre-computed</span>

*   <span>Always think multiple data centers: every data point goes to a multiple locations</span>

*   <span>Backup in S3 for catastrophic failures</span>

*   <span>All raw data are stored in Spark cluster</span>

# <span>Team</span>

## <span>Structure</span>

*   <span>70 full-time employees</span>

*   <span>15 developers (platform, ad serving, frontend, mobile)</span>

*   <span>4 data scientists</span>

*   <span>5 dev. ops.</span>

*   <span>Engineers in Palo Alto, CA</span>

*   <span>Business in San Francisco, CA</span>

*   <span>Offices in New York, London and Berlin</span>

## <span>Interaction</span>

*   <span>HipChat to discuss most issues</span>

*   <span>Asana for project-based communication</span>

*   <span>All code is reviewed</span>

*   <span>Frequent group code reviews</span>

*   <span>Quarterly company outings</span>

*   <span>Regular town hall meetings with CEO</span>

*   <span>All engineers (junior to CTO) write code</span>

*   <span>Interviews are tough: offers are really rare</span>

## <span>Development Cycle</span>

*   <span>Developers, business side or data science team comes up with an idea</span>

*   <span>Idea is reviewed and scheduled to be executed on a Monday meeting</span>

*   <span>Feature is implemented in a branch; development environment is used for basic testing</span>

*   <span>A pull request is created</span>

*   <span>Code is reviewed and iterated upon</span>

*   <span>For big features group code reviews are common</span>

*   <span>The feature gets merged to master</span>

*   <span>The feature gets deployed to staging with the next build</span>

*   <span>The feature gets tested on 5% real traffic</span>

*   <span>Statistics are examined</span>

*   <span>If the feature is successful it graduates to production</span>

*   <span>Feature is closely monitored for couple days</span>

## <span>Avoiding Issues</span>

*   <span>The system is designed to handle failure of any component</span>

*   <span>No failure of a single component can harm ad serving or data consistency</span>

*   <span>Omniscient monitoring</span>

*   <span>Engineers watch and analyze key business reports</span>

*   <span>High quality of code is essential</span>

*   <span>Some features take multiple code reviews and iterations before graduating</span>

*   <span>Alarms are triggered when:</span>

    *   <span>Stats for staging are different from production</span>

    *   <span>FATAL errors on critical services</span>

    *   <span>Error rate exceeds threshold</span>

    *   <span>Any irregular activity is detected</span>

*   <span>data are never dropped</span>

*   <span>Most log lines can be easily parsed</span>

*   <span>Rolling back of any change is easy by design</span>

*   <span>After every failure: fix, make sure same thing does not happen in the future, and add monitoring</span>

# <span>Lessons Learned</span>

## <span>Product Development</span>

*   <span>Being able to swap any component easily is key to growth</span>

*   <span>Failures drive innovative solutions</span>

*   <span>Staging environment is essential: always be ready to loose 5%</span>

*   <span>A/B testing is essential</span>

*   <span>Monitor everything</span>

*   <span>Build intelligent alerting system</span>

*   <span>Engineers should be aware of business goals</span>

*   <span>Business people should be aware of limitations of engineering</span>

*   <span>Make builds and continuous integration fast. Jenkins run on a 2 bare metal servers with 32 CPU, 128G RAM and SSD drives</span>

## <span>Infrastructure</span>

*   <span>Monitoring all data points is critical</span>

*   <span>Automation is important</span>

*   <span>Every component should support HA by design</span>

*   <span>Kernel optimizations can have up to 25% performance improvement</span>

*   <span>Process and IRQ balancing lead to another 20% performance improvement</span>

*   <span>Power saving features impact performance</span>

*   <span>Use SSDs as much as possible</span>

*   <span>When optimizing, profile everything. Flame graphs are great!</span>

[On Hacker News](https://news.ycombinator.com/item?id=9171871)

</div>