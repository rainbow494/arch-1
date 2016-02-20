## [Vinted Architecture: Keeping a busy portal stable by deploying several hundred times per day](/blog/2015/2/9/vinted-architecture-keeping-a-busy-portal-stable-by-deployin.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, February 9, 2015 at 8:56AM</span></div>

<div class="body">

![](https://farm8.staticflickr.com/7375/16286520378_5e8136d8d4_o.jpg)

_This is guest post by [Nerijus Bendžiūnas](https://www.linkedin.com/profile/view?id=48589106) and [Tomas Varaneckas](https://twitter.com/spajus) of [Vinted](https://www.vinted.com/)._

[Vinted](https://www.vinted.com/) <span>is a peer-to-peer marketplace to sell, buy and swap clothes. It allows members to communicate directly and has the features of a social networking service.</span>

Started in 2008 as a small community for Lithuanian girls, it developed into a worldwide project that serves over 7 million users in 8 different countries, and is growing non-stop, handling over 200 M requests per day.

## Stats

*   7 million users and growing
*   2.5 million monthly active users
*   200 million requests / day (pageviews + API calls)
*   930 million photos
*   80 TB raw space for image storage
*   60 TB HDFS raw space for analytics data
*   70% of traffic comes from mobile apps (API requests)
*   3+ hours / week time spent per member
*   25 million listed items
*   Over 220 servers:
    *   47 for internal tools (vpn, chef, monitoring, development, graphing, build, backups, etc.)
    *   38 for the Hadoop ecosystem
    *   34 for image processing and storage
    *   30 for Unicorn and microservices
    *   28 for MySQL databases, including replicas
    *   19 for the Sphinx search
    *   10 for resque background jobs
    *   10 for load balancing with Nginx
    *   6 for Kafka
    *   4 for Redis
    *   4 for email delivery

## Technology Stack

### Third party services

*   [GitHub](https://github.com/) for code, issues and discussions
*   [NewRelic](http://newrelic.com/) for monitoring app response times
*   [CloudFlare](https://www.cloudflare.com/) for DDoS protection and DNS
*   [Amazon Web Services](http://aws.amazon.com/) (SES, Glacier) for notifications and long term backups
*   [Slack](https://slack.com/) for work conversations and "chat ops"
*   [Trello](https://trello.com/) for tasks
*   [Pingdom](http://pingdom.com/) for uptime monitoring

### Core app and services

*   [Ruby](https://www.ruby-lang.org/) for services, scripts and the main app
*   [Rails](http://rubyonrails.org/) for the main app and internal apps
*   [Unicorn](http://unicorn.bogomips.org/) to serve the main app
*   [Percona MySQL Server](http://www.percona.com/software/percona-server/ps-5.6) as main database
*   [Sphinx Search](http://sphinxsearch.com/) for full-text search and for reducing load to MySQL (testing [ElasticSearch](http://www.elasticsearch.org/))
*   [Capistrano](http://capistranorb.com/) for deployments
*   [Jenkins](http://jenkins-ci.org/) for running tests, deployments and various other tasks
*   [Memcached](http://memcached.org/) for caching
*   [Resque](http://resquework.org/) for background jobs
*   [Redis](http://redis.io/) for Resque and Feed
*   [RabbitMQ](http://www.rabbitmq.com/) for passing messages to services
*   [HAproxy](http://www.haproxy.org/) for high availability and load balancing
*   [GlusterFS](http://www.gluster.org/) for distributed storage
*   [Nginx](http://nginx.org/) to serve web requests
*   [Apache Zookeeper](http://zookeeper.apache.org/) for distributed locking
*   [Flashcache](https://github.com/facebook/flashcache/) for increased I/O throughput

### Data warehouse stack

*   [Clojure](http://clojure.org/) for data ingestion services
*   [Apache Kafka](http://kafka.apache.org/) for storing in-flight data
*   [Camus](https://github.com/linkedin/camus) for offloading data from Kafka to HDFS
*   [Apache Hive](https://hive.apache.org/) as SQL-on-Hadoop solution
*   [Cloudera CDH](http://www.cloudera.com/content/cloudera/en/products-and-services/cdh.html) Hadoop distribution
*   [Cloudera Impala](http://www.cloudera.com/content/cloudera/en/products-and-services/cdh/impala.html) as low latency SQL-on-Hadoop solution
*   [Apache Spark](https://spark.apache.org/) in experimentation phase
*   [Apache Oozie](http://oozie.apache.org/) as workflow scheduler (investigating alternatives)
*   [Scalding](https://github.com/twitter/scalding) for data transformations
*   [Avro](http://avro.apache.org/) for data serialization
*   [Apache Parquet](http://parquet.incubator.apache.org/) for data serialization

### Servers and Provisioning

*   Mostly bare metal
*   Multiple data centers
*   [CentOS](http://www.centos.org/)
*   [Chef](https://www.chef.io/) for provisioning nearly everything
*   [Berkshelf](http://berkshelf.com/) for managing Chef dependencies
*   [Test Kitchen](http://kitchen.ci/) for running infrastructure tests on VMs
*   [Ansible](http://www.ansible.com/) for rolling upgrades

### Monitoring

*   [Nagios](http://www.nagios.org/) for usual ops monitoring
*   [Cacti](http://www.cacti.net/) for graphs and capacity planning
*   [Graylog2](https://www.graylog2.org/) for app logs
*   [Logstash](http://logstash.net/) for server logs
*   [Kibana](http://www.elasticsearch.org/overview/kibana/) for querying logstash
*   [Statsd](https://github.com/etsy/statsd) for collecting real-time metrics from the app
*   [Graphite](http://graphite.wikidot.com/) for storing real-time metrics from the app
*   [Grafana](http://grafana.org/) for creating beautiful dashboards with app metrics
*   [Hubot](https://hubot.github.com/) for chat-based monitoring

## Architecture overview

*   Bare metal servers are used as a cheaper and more powerful alternative to cloud providers.
*   Servers hosted in 3 different data centers across Europe and the US.
*   Nginx frontends route HTTP requests to unicorn workers and do SSL termination.
*   [Pacemaker](http://clusterlabs.org/) is used to ensure high availability of Nginx servers.
*   In different countries, every Vinted portal has its own separate deployment and set of resources.
*   To increase machine utilization, most services that belong to multiple portals may run alongside each other on a single bare metal server. Chef recipes take care of unique port allocation and other separation concerns.
*   MySQL sharding is avoided by having a separate database for each portal.
*   In our largest portal, functional sharding is already happening, and a point where the single largest table will not fit into the server is months away.
*   Caching in Rails app uses custom mechanism with L2 cache that prevents spikes when the main cache is expired. Several caching strategies are used:
    *   remote (in memcached)
    *   local (in unicorn worker memory)
    *   semi-local (in unicorn worker memory, falling back to memcached when local cache expires).
*   Several microservices are built around the core rails app, all with a clear purpose, like sending iOS push notifications, storing and serving brand names, storing and serving hashtags.
*   Microservices can be either per-portal, per-datacenter or global.
*   Multiple instances of each microservice are running for high availability.
*   HTTP-based microservices are balanced by Nginx.
*   Custom feeds for each member are implemented using Redis.
*   Catalog pages are loaded from the Sphinx index rather than MySQL when filters are used (custom size, color, etc.).
*   Images are processed by a separate Rails app.
*   Processed images are stored in GlusterFS.
*   Images are cached after 3rd hit, since the first few hits usually come from the uploader and an image may be rotated.
*   Memcached instances are sharded using [twemproxy](https://github.com/twitter/twemproxy).

## Team

*   over 150 full-time employees
*   30 developers (backend, frontend, mobile)
*   5 site reliability engineers
*   Main HQ in Vilnius, Lithuania
*   Offices in the USA, Germany, France, the Czech Republic and Poland

## How The Company Works

*   Nearly all information is open to all employees
*   Using GitHub for almost everything (including to discuss company issues)
*   Real-time discussions in Slack
*   Everyone is free to participate in things that interest them
*   Autonomous teams
*   No seniority levels
*   Cross-functional development teams
*   No enforced process, teams decides how to manage themselves
*   Teams work on high-level problems and decide how to tackle them
*   Each team decides how, when and where they work
*   Teams hire for themselves when a need arises

## Development Cycle

*   Developers branch from master.
*   Changes become pull requests in GitHub.
*   Jenkins runs [Pronto](https://github.com/mmozuras/pronto) to do static code and style checks, runs all tests on pull request branches and updates the GitHub pull request with a status.
*   Other developers review the change, add comments.
*   Pull requests may be updated multiple times with various fixes.
*   Jenkins runs all tests after each update.
*   Git history is cleaned up before merging with master to keep master history concise.
*   Accepted pull requests are merged into master.
*   Jenkins runs master build with all tests and triggers deployment jobs to roll out the new version.
*   Several minutes later, after the code has reached master branch, it is applied in production.
*   Pull requests are usually small.
*   Deployments happen ~300 times per day.

## Avoiding Failures

Deploying hundreds of times per day does not mean everything always has to be broken, but keeping the site stable requires some discipline.

*   Deployments do not happen if tests are broken. There is no way to override this, master has to be green in order to deploy.
*   Automatic deployments are locked every night and during weekends.
*   Anyone can lock deployments manually from a Slack chat.
*   When deployments are locked, it is possible to "force" a deployment manually, from the Slack chat.
*   The team is very picky when it comes to reviewing code. The quality bar is set high. Tests are mandatory.
*   Before Unicorn reloads code, a warmup is done in production. It includes several requests to various critical parts of the portal.
*   During warmup, if any one request does not return 200 OK, the old code remains and deployment fails.
*   Occasionally a problem is not caught by tests / warmup, and broken code is released to production.
*   Errors are streamed to graylog server.
*   Alarms are trigerred if the error rate exceeds a threshold.
*   Error rate alarms and failed build notifications are reported instantly to a Slack chat.
*   All errors contain extra metadata: Unicorn worker host and pid, HTTP request details, git revision of the code that caused the problem, error backtrace.
*   Some types of "Fatal" errors are also reported directly to a Slack chat.
*   Each deployment log contains GitHub diff URL with all the changes.
*   If something breaks in production, it is easy to pinpoint the problem quickly due to a small changeset and instant error feedback.
*   Deployment details are reported to NewRelic, which makes it easy to troubleshoot introduction of performance bottlenecks.

## Reducing Time To Production

There is a big focus on both fast and stable releases, the team is always working to keep build and deployment times as short as possible.

*   Full test suite contains >7000 tests and runs in ~3 minutes.
*   New tests are added constantly.
*   Jenkins runs on a bare metal machine with 32 CPU, 128G RAM and SSD drives.
*   Jenkins splits all tests into multiple chunks and runs them in parallel, aggregating final results.
*   Tests would run over 1 hour on an average machine without parallelisation.
*   Rails assets are pre-built in Jenkins for every commit in master branch.
*   During deployment, prebuilt assets are downloaded from jenkins and uploaded to all target servers.
*   BitTorrent is used when uploading a release to target servers.

## Running live database migrations

We can change the structure of our production MySQL databases without downtime, in most cases even at rush hour.

*   Database migrations can happen during any deployment
*   Percona toolkit's pt-online-schema-change is used to run alters on a copy of the table, while keeping it in sync with the original, and to switch the copy with the original when the change is done

# Operations

## Server Provisioning

*   Chef is used to provision nearly every aspect of our infrastructure.
*   SRE team does pull requests for infrastructure changes just like Development teams do for code changes.
*   Jenkins is verifying all Chef pull requests, running cross dependency checks, foodcritic, etc.
*   Chef code changes are reviewed in GitHub by the team.
*   Developers can make infrastructure change pull requests to Chef repository.
*   When pull requests are merged, Jenkins uploads changed Chef cookbooks and applies them in production.
*   Plans to spawn DigitalOcean droplets to run Chef kitchen tests with Jenkins.
*   Jenkins itself is configured with Chef, not using the web UI.

## Monitoring

*   Cacti graphs server side metrics
*   Nagios checks for everything that can be up or down
*   Nagios health checks for some of our apps and services
*   Statsd / Graphite for tracking app metrics, like member logins, signups, database object creation
*   Grafana used for nice dashboards
*   Grafana dashboards can be described and created dynamically using Chef

## Chat Ops

*   Hubot sits in Slack.
*   Slack rooms are subscribed to various event notifications via [hubot-pubsub](https://github.com/spajus/hubot-pubsub).
*   #deploy room shows information about deployments, with links to GitHub diffs, Jenkins console outputs, etc. Right here deployments can be triggered, locked and unlocked with simple chat commands.
*   #deploy-fail room shows only deployment failures.
*   #failboat room shows announcements about error rates and individual errors in production.
*   There multiple #failboat-* rooms that give information about cron failures, stuck resque jobs, nagios warnings, newrelic alerts.
*   Twice a day Graylog errors are crunched and cross-checked with GitHub, reports are generated and presented to the development team.
*   If someone mentions you in GitHub, Hubot gives you a link to the issue in a private message on Slack.
*   When an issue or pull request is created in GitHub, teams that are mentioned receive pings in their appropriate Slack rooms.
*   Graphite graphs can be queried and displayed in Slack chat via Hubot.
*   You can ask Hubot questions about infrastructure, i.e. as about the machines where a certain service is deployed. Hubot queries the Chef server to get the data.
*   Developers use Hubot notifications for certain events that happen in our app. For example, customer the support chatroom is automatically notified about possible fraud cases.
*   Hubot is integrated with the office [netatmo](https://www.netatmo.com/) module and can tell everyone what CO2, noise, temperature and humidity levels are.

# Data warehouse stack

*   We ingest data from two sources:
    *   Website event tracking data: impressions, clicks etc.
    *   Database (MySQL) changes.
*   Most event tracking data is published to an HTTP endpoint from JavaScript/Android/iOS front-end apps. Some events are published to a local UDP socket by our core Ruby on Rails application. We chose UDP in order to avoid blocking the request.
*   There's a service that listens on the UDP socket for new events, buffers them and periodically emits to a raw events' topic in Kafka.
*   A stream processing service consumes the raw events topic, validates the events, encodes them in Avro format and emits to event-specific topics.
*   We keep our Avro schemas of events in a separate centralized schema registry service.
*   Invalid events are placed into a separate Kafka topic for possible correction.
*   We use LinkedIn's Camus job to offload events from event-specific topics to HDFS incrementally. Time to Hadoop for an event is usually up to an hour.
*   Using Hive and Impala for ad-hoc queries and data analysis.
*   Working on Scalding-based solution for data processing.
*   Reports run from our in-house grown OLAP-like reporting system written in Ruby.

# Lessons Learned

## Product development

*   Invest in code quality.
*   Cover absolutely everything with tests.
*   Release small changes.
*   Use feature switches to deploy unfinished features to production.
*   Do not keep long running branches of code when developing something.
*   Invest in a fast release cycle.
*   Build the product mobile-first.
*   Design API very well from the very beginning, it is difficult to change it later.
*   Including sender hostname and app name in RabbitMQ messages can be a life saver.
*   Do not guess or make assumptions, do [AB](https://github.com/vinted/ab) testing and check the data.
*   If you plan on using Redis for something big, think about sharding from very beginning.
*   Never use forked and slightly modified ruby gems if you are planning on keeping your Rails up to date.
*   Make sharing your content through social networks as easy as possible.
*   Search engine optimization is a full-time job.

## Infrastructure and Operations

*   Deploy often to increase system stability. It may sound counter-intuitive at first.
*   Automate everything you can.
*   Monitor everything that can be down.
*   Network switch buffers matter.
*   Capacity planning is hard.
*   Think about HA from the start.
*   RabbitMQ cluster is not worth the effort.
*   Measure and graph everything you can: HTTP response times, Resque queue sizes, Model persistence rates, Redis response times. You cannot improve what you cannot measure.

## Data warehouse stack

*   There's a myriad of tools in the Hadoop ecosystem. It's hard to pick the right one.
*   If you are writing a user defined function (UDF) for Hive it's time to consider a non-SQL solution for data transformations.
*   The "Hadoop application" notion is vague. Oftentimes we need to glue our application components together manually.
*   Everyone writes their own workflow manager. And for a good reason.
*   Distributed system monitoring is tough. Anomaly detection and graphing helps.
*   The core Hadoop infrastructure (HDFS, YARN) is rock solid but the newer tools usually have unpleasant nuances.
*   Kafka is amazing.

</div>