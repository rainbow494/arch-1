## [AppLovin: Marketing to Mobile Consumers Worldwide by Processing 30 Billion Requests a Day](/blog/2015/3/9/applovin-marketing-to-mobile-consumers-worldwide-by-processi.html)

    

    

![](https://farm9.staticflickr.com/8570/16442530119_470a4487ee_m.jpg)

_This is a guest post from [AppLovin](http://www.applovin.com/)'s VP of engineering, [Basil Shikin](https://www.linkedin.com/in/basilshikin), on the infrastructure of its mobile marketing platform. Major brands like Uber, Disney, Yelp and Hotels.com use AppLovin's mobile marketing platform. It processes 30 billion requests a day and 60 terabytes of data a day._

    AppLovin's marketing platform provides marketing automation and analytics for brands who want to reach their consumers on mobile. The platform enables brands to use real-time data signals to make effective marketing decisions across one billion mobile consumers worldwide.    

#     Core Stats    

*       30 Billion ad requests per day    

*       300,000 ad requests per second, peaking at 500,000 ad requests per second    

*       5ms average response latency    

*       3 Million events per second    

*       60TB of data processed daily    

*       ~1000 servers    

*       9 data centers    

*       ~40 reporting dimensions    

*       500,000 metrics data points per minute    

*       1 Pb Spark cluster    

*       15GB/s peak disk writes across all servers    

*       9GB/s peak disk reads across all servers    

*       Founded in 2012, AppLovin is headquartered in Palo Alto, with offices in San Francisco, New York, London and Berlin.    

#     Technology Stack    

##     Third Party Services    

*   [    Github    ](https://github.com/)     for code    

*   [    Asana    ](https://asana.com/)     for project management    

*   [    HipChat    ](https://www.hipchat.com/)     for communication    

##     Data Storage    

*   [    Aerospike    ](http://www.aerospike.com/)     for user profile storage    

*   [    Vertica    ](http://www.vertica.com/)     for aggregated statistics and real-time reporting    

*       Aggregating 350,000 rows per second and writing to Vertica at 34,000 rows per second    

*       Peak 12,000 user profiles per second written to Aerospike    

*       MySQL for ad data    

*   [    Spark    ](https://spark.apache.org/)     for offline processing and deep data analysis    

*   [    Redis    ](http://redis.io/)     for basic caching    

*   [    Thrift    ](https://thrift.apache.org/)     for all data storage and transfers    

*       Each data point replicated in 4 data centers    

*       Each service is replicated at least in 2 data centers (at most in 8)    

*       Amazon Web Services used for long term data storage and backups    

##     Core App And Services    

*       Custom C/C++ Nginx module for high performance ad serving    

*       Java for data processing and auxiliary services    

*       PHP / Javascript for UI    

*   [    Jenkins    ](http://jenkins-ci.org/)     for continuous integration and deployment    

*       Zookeeper for distributed locking    

*       HAProxy and IPVS for high availability    

*       Coverity for Java/C++ static code analysis    

*       Checkstyle and PMD for PHP static code analysis    

*       Syslog for DC-centralized log server    

*       Hibernate for transaction-based services    

##     Servers and Provisioning    

*       Ubuntu    

*       Cobbler for bare metal provisioning    

*       Chef for configuring servers    

*       Berkshelf for Chef dependencies    

*       Docker with Test Kitchen for running infrastructure tests    

*           A combination of software (ipvs, haproxy) and hardware load balancers. Plan to gradually move away from traditional hardware load balancers.        

#     Monitoring Stack    

##     Server Monitoring    

*   [    Icinga    ](https://www.icinga.org/)     for all servers    

*       ~100 custom Nagios plugins for deep server monitoring    

*       550 various probes per server    

*   [    Graphite    ](http://graphite.wikidot.com/)     as data format    

*   [    Grafana    ](http://grafana.org/)     for displaying all graphs    

*   [    PagerDuty    ](http://www.pagerduty.com/)     for issue escalation    

*   [    Smokeping    ](http://oss.oetiker.ch/smokeping/)     for network mesh monitoring    

##     Application Monitoring    

*   [    VividCortex    ](https://vividcortex.com/)     for MySQL monitoring    

*       JSON /health endpoint on each service    

*       Cross-DC database consistency monitoring    

*       9 4K 65” TVs for showing all graphs across the office    

*       Statistical deviation monitoring    

*       Fraudulent users monitoring    

*       Third-party systems monitoring    

*       Deployments are recorded in all graphs    

##     Intelligent Monitoring    

*       Intelligent alerting system with a feedback loop: a system that can introspect anything can learn anything    

*       Third-party stats about AppLovin are also monitored    

*       Alerting is a cross-team exercise: developers, ops, business, data scientists are involved    

#     Architecture Overview    

##     General Considerations    

*       Store everything in RAM    

*       If it does not fit, save it to SSD    

*       L2 cache level optimizations matter    

*       Use right tool for the right job    

*       Architecture allows swapping any component    

*       Upgrade only if an alternative is 10x better    

*       Write your own components if there is nothing suitable out there    

*       Replicate important data at least 3x    

*       Make sure every message can be re-played without data corruption    

*       Automate everything    

*       Zero-copy message passing    

##     Message Processing    

*       Custom message processing system that guarantees message delivery    

*       3x replication for each message    

*       Sending a message = writing to disk    

*       Any service may fail, but no data are lost    

*       Message dispatching system connects all components together, provides isolation and extensibility of the system    

*       Cross-DC failure tolerance    

##     Ad Serving    

*       Nginx is really fast: can serve an ad in less than a millisecond    

*       Keep all ad serving data in memory: read only    

*       jemalloc gave a 30% speed improvement    

*       Use Aerospike for user profiles: less than 1ms to fetch a profile    

*       Pre-compute all ad serving data on one box and dispatch across all servers    

*       Torrents are used to propagate serving data across all servers. Using Torrents resulted in 83% network load drop on the originating server compared to HTTP-based distribution.    

*       mmap is used to share ad serving data across nginx processes    

*       XXHash is the fastest hash function with a low collision rate. 75x faster than SHA-1 for computing checksums    

*       5% of real traffic goes to staging environment    

*       Ability to run 3 A/B tests at once (20%/20%/10% of traffic for three separate tests, 50% for control)    

*       A/B test results are available in regular reporting    

#     Data Warehouse    

*       All data are replicated    

*       Running most reports takes under 2 seconds    

*       Aggregation is key to allow fast reports on large amounts of data    

*       Non-aggregated data for the last 48 hours is usually to resolve most issues    

*       7 days of raw logs is usually enough for debug    

*       Some reports must be pre-computed    

*       Always think multiple data centers: every data point goes to a multiple locations    

*       Backup in S3 for catastrophic failures    

*       All raw data are stored in Spark cluster    

#     Team    

##     Structure    

*       70 full-time employees    

*       15 developers (platform, ad serving, frontend, mobile)    

*       4 data scientists    

*       5 dev. ops.    

*       Engineers in Palo Alto, CA    

*       Business in San Francisco, CA    

*       Offices in New York, London and Berlin    

##     Interaction    

*       HipChat to discuss most issues    

*       Asana for project-based communication    

*       All code is reviewed    

*       Frequent group code reviews    

*       Quarterly company outings    

*       Regular town hall meetings with CEO    

*       All engineers (junior to CTO) write code    

*       Interviews are tough: offers are really rare    

##     Development Cycle    

*       Developers, business side or data science team comes up with an idea    

*       Idea is reviewed and scheduled to be executed on a Monday meeting    

*       Feature is implemented in a branch; development environment is used for basic testing    

*       A pull request is created    

*       Code is reviewed and iterated upon    

*       For big features group code reviews are common    

*       The feature gets merged to master    

*       The feature gets deployed to staging with the next build    

*       The feature gets tested on 5% real traffic    

*       Statistics are examined    

*       If the feature is successful it graduates to production    

*       Feature is closely monitored for couple days    

##     Avoiding Issues    

*       The system is designed to handle failure of any component    

*       No failure of a single component can harm ad serving or data consistency    

*       Omniscient monitoring    

*       Engineers watch and analyze key business reports    

*       High quality of code is essential    

*       Some features take multiple code reviews and iterations before graduating    

*       Alarms are triggered when:    

    *       Stats for staging are different from production    

    *       FATAL errors on critical services    

    *       Error rate exceeds threshold    

    *       Any irregular activity is detected    

*       data are never dropped    

*       Most log lines can be easily parsed    

*       Rolling back of any change is easy by design    

*       After every failure: fix, make sure same thing does not happen in the future, and add monitoring    

#     Lessons Learned    

##     Product Development    

*       Being able to swap any component easily is key to growth    

*       Failures drive innovative solutions    

*       Staging environment is essential: always be ready to loose 5%    

*       A/B testing is essential    

*       Monitor everything    

*       Build intelligent alerting system    

*       Engineers should be aware of business goals    

*       Business people should be aware of limitations of engineering    

*       Make builds and continuous integration fast. Jenkins run on a 2 bare metal servers with 32 CPU, 128G RAM and SSD drives    

##     Infrastructure    

*       Monitoring all data points is critical    

*       Automation is important    

*       Every component should support HA by design    

*       Kernel optimizations can have up to 25% performance improvement    

*       Process and IRQ balancing lead to another 20% performance improvement    

*       Power saving features impact performance    

*       Use SSDs as much as possible    

*       When optimizing, profile everything. Flame graphs are great!    

[On Hacker News](https://news.ycombinator.com/item?id=9171871)

    