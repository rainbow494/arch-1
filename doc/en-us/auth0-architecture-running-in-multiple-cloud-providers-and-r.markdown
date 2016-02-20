## [Auth0 Architecture - Running in Multiple Cloud Providers and Regions](/blog/2014/12/1/auth0-architecture-running-in-multiple-cloud-providers-and-r.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, December 1, 2014 at 8:56AM</span></div>

<div class="body">

![](https://farm8.staticflickr.com/7547/15920200395_04c420407a_m.jpg)

_This is a guest post by [Jose Romaniello](https://twitter.com/jfroma), Head of Engineering, at Auth0\._

[Auth0](https://auth0.com/) provides authentication, authorization and single sign on services for apps of any type: mobile, web, native; on any stack.

Authentication is critical for the vast majority of apps. We designed Auth0 from the beginning with multipe levels of redundancy. One of this levels is hosting. Auth0 can run anywhere: our cloud, your cloud, or even your own servers. And when we run Auth0 we run it on **multiple-cloud providers** and in **multiple regions** simultaneously.

<span>This article is a brief introduction of the infrastructure behind</span> [<span>app.auth0.com</span>](https://app.auth0.com/) <span>and the strategies we use to keep it up and running with high availability.</span>

## <span>Core Service Architecture</span>

<span>The core service is relatively simple:</span>

*   <span>**Front-end servers**</span><span>: these consist of several x-large VMs, running Ubuntu on Microsoft Azure.</span>

*   <span>**Store**</span><span>: mongodb, running on dedicated memory optimized X-large VMs.</span>

*   <span>**Intra-node service routing**</span><span>: nginx</span>

<span>All components of Auth0 (e.g. Dashboard, transaction server, docs) run on all nodes. All identical.</span>

## <span>Multi-cloud / High Availability</span><span>![](https://lh4.googleusercontent.com/H15B6XlT-i9OOBTmJESs1HFxxJy4EltxG3NrjuipG1jjUxjRPjXffvoJA8bkdT7YGNS-OZplhVQDFfK6LUWHs5Z9CTyCEtIRnmdrGg5YjJJnAa-S0S8Ak_692gKe-uM6)</span><span>Multi cloud architecture</span>

<span>![](https://lh4.googleusercontent.com/dSsQoV54IgStJl3_Z2Z_uvO_1DCE-Bx86V1EE-4uF4WRyGKQrttjBHxtoH-9DiwicBWvOKeBF9GmQEX9b0MeehoECnZS-cxme1UIeiuCzweQcP2Iomf8IqIbuVg5wjbz)</span>

<span>Last week, Azure suffered a global outage that lasted for hours. During that time our HA plan activated and we switched over to AWS</span>

*   <span>The services runs primarily on</span> <span>**Microsoft Azure (IaaS)**</span><span>. Secondary nodes on stand-by always ready on AWS.</span>

*   <span>We use</span> <span>**Route53**</span> <span>with a failover routing policy. TTL at 60 secs. The Route53 health check detects using a probe against primary DC, if it fails (3 times, 10 seconds interval) it changes the DNS entry to point to secondary DC. So max downtime in case of primary failure is ~2 minutes.</span>

*   <span>We use</span> <span>**puppet**</span> <span>to deploy on every "push to master". Using puppet allows us to be cloud independent on the configuration/deployment process. Puppet Master runs on our build server (TeamCity currently).</span>

*   <span>MongoDB is replicated often to secondary DC and secondary DC is configured as read-only.</span>

*   <span>While running on the secondary DC, only runtime logins are allowed and the dashboard is set to "read-only mode".</span>

*   <span>We replicate all the configuration needed for a login to succeed (application info, secrets, connections, users, etc). We don’t replicate transactional data (tokens, logs).</span>

*   <span>In case of failover, there might might some logging records that are lost. We are planning to improve that by having a real-time replica across Azure and AWS.</span>

*   <span>We use our own version of</span> <span>**chaos monkey**</span> <span>to test the resiliency of our infrastructure</span> [<span>https://github.com/auth0/chaos-mona</span>](https://github.com/auth0/chaos-mona)

[<span>![](https://lh3.googleusercontent.com/rSrBzApzqXXbAnqgGzN-eiyo1Qj6VwmBg4zH2IqKibxNfGzci__e70MOAKA8qVfuh7pqPNkizxxb3DyJnz_emowwiBHXpwDRP5_3EaYh1YMwPFrx9eOspl3CF3b-K_EH)</span>](https://github.com/auth0/chaos-mona)

## <span>Automated Testing</span>

*   <span>We have 1000+ unit and integration tests.</span>

*   <span>We use</span> <span>**saucelabs**</span> <span>to run cross-browser (desktop/mobile) integration tests for</span> [<span>Lock</span>](https://auth0.com/lock)<span>, our JavaScript login widget.</span>

*   <span>We use</span> <span>**phantomjs/casper**</span> <span>for integration tests. We test, for instance, that a full flow login with Google and other providers works fine.</span>

*   <span>All these run before every push to production.</span>

## <span>CDN</span>

<span>Our use case is simple, we need to serve our JS library and its configuration (which providers are enabled, etc.). Assets and configuration data is uploaded to S3\. It has to support TLS on our own custom domain (</span>[<span>https://cdn.auth0.com</span>](https://cdn.auth0.com/)<span>). We ended up building our own CDN.</span>

*   <span>We tried 3 reputable CDN providers, but run into a whole variety of issues: The first one we tried when we didn't have our own domain for cdn. At some point we decided we needed our own domain over SSL/TLS. This cdn was too expensive if you want SSL and customer domain at that point (600/mo). We also had issues configuring it to work with gzip and S3\. Since S3 cannot serve both version (zipped and not) of the same file and this CDN doesn't have content negotiation, some browsers (</span><span>cough</span> <span>IE) don't play well with this. So we moved to another CDN which was much cheaper.</span>

*   <span>The second CDN, we had a handful of issues and we couldn't understand the root cause of them. Their support was on chat and it took time to get answers. Sometimes it seemed to be S3 issues, sometimes they had issues on routing, etc.</span>

*   <span>We decided to spend more money and we moved to a third CDN. Given that this CDN is being used by high load services like GitHub we thought it was going to be fine. However, our requirements were different from GitHub. If the CDN doesn't work for GitHub, you won't see an image on the README.md. In our case, our customers depends on the CDN to serve the Login Widget, which means that if it doesn't work, then their customers can't login.</span>

*   <span>We ended up building our own CDN using nginx, varnish and S3\. It's hosted on every region on AWS and so far it has been working great (no downtime). We use Route53 latency based routing.</span>

## <span>Sandbox (Used to run untrusted code)</span>

<span>One of the features we provide is the ability to run custom code as part of the login transaction. Customers can write these rules and we have a public repository for commonly used</span> [<span>rules</span>](https://github.com/auth0/rules)<span>.</span>

*   The sandbox is built on**CoreOS**, **Docker** and **etcd**.

*   <span>There is a pool of Docker instances that gets assigned to a tenant on-demand.</span>

*   <span>Each tenant gets its own docker instance and there is a recycling policy based on idle time.</span>

*   <span>There is a controller doing the recycling policy and a proxy that routes the request to the right container.</span>

<span>![](https://lh3.googleusercontent.com/4OgDHL57tyifmsBeO1eTrfZAcCZHu4-af5dDpk8Dr1O0KdaXn_pI8VVxnZdrEuR9rJswJA5eE_SmFQOWaTp0-T3bWVDcV50dIMU5R-UW9ojJIc_OJzbY1a1qDmO7H34b)</span>

<span>![](https://lh4.googleusercontent.com/BSCW_ZmZpSJhGZSVnlSlatksaapSJ57p19yxIKvMO1SYSWTMT8zpfKZzDuFpKCVtahmJ6LejEwoEQFhX6bar5gLpUBmzKYvpuXomJsOUrGXjx0mOG8QgGGH8QJ_rDg3R)</span>

<span>More information about the sandbox is in this JSConf presentation Nov 2014:</span> [<span>https://www.youtube.com/watch?feature=player_detailpage&v=I4VkZ5H9PE8#t=7015</span>](https://www.youtube.com/watch?feature=player_detailpage&v=I4VkZ5H9PE8#t=7015) <span>and slides:</span> [<span>http://tjanczuk.github.io/about/sandbox.html</span>](http://tjanczuk.github.io/about/sandbox.html)

## <span>Monitoring</span>

<span>Initially we used</span> <span>**pingdom**</span> <span>(we still use it), but we decided to develop our own health check system that can run arbitrary health checks based on node.js scripts. These run from all AWS regions.</span>

*   <span>It uses the same sandbox we developed for our service. We call the sandbox via an http API and send the node.js script to run as an HTTP POST.</span>

*   <span>We monitor all the components and we also do synthetic transactions against the service (e.g. a login transaction).</span>

<span>![](https://lh6.googleusercontent.com/UgD9dLuAu_C7z0eGt4GmUnDqr-pdCjetofL1DwlBPv0tZ22v3bKO83qcafz8VKitV-2IDsYTTf-1IWncHojCcDuoeUiJElGSOK3yQWHZFT0tHEdGkouqi8OjgHDxkDpO)</span>

<span>![](https://lh4.googleusercontent.com/NeJRothvUc5Er2V4mDhS6RUo8XuUUBrftaJ_uxk3yGr17oqzkJK0BXOHeOXs1SOjuVPxVIAl3uwYru4mVKopjoZxATgS7q8Nfv4OpePRyahvrGRelsIhrCPDAC5mproo)</span>

<span>If a health check fails we get notified through</span> <span>**Slack**</span><span>. We have two</span> <span>Slack</span> <span>channels #p1 and #p2\. If the failure happens 1 time, it gets posted to #p2\. If it happens 2 times in a row it gets posted to #p1 and all members of devops get an SMS (via</span> <span>**Twilio**</span><span>).</span>

<span>![](https://lh6.googleusercontent.com/3av9ukg9bDqYGxE_ehZESo4o3ZJ0fHEy4FSbmPwVoAp5HhnuHfP5-EQmHoT6nIV-VfnIJBSHGNpFiGDHTyDC2__2if1jLeUkEAJ18f2jfQH-iZpFT_e_VeAf9Yp0PChe)</span>

<span>For detailed performance counters and response times we use</span> [<span>statsd</span>](https://github.com/etsy/statsd/) <span>and we send all the metrics to</span> [<span>Librato</span>](https://librato.com/)<span>. This is an example of a chart you can create.</span>

<span>![](https://lh4.googleusercontent.com/rS6Iu1fADBvQdHXCxSF6HtkD5Vx67urrcDzRniekF0zFQzOQeEsHaSPU2RjGyCRAVKGsL_Il5cQ42QEzQut8cEDK_fCKozb3DBRWvUSVMsZQpR0Ghn2hst2WFEZO92XS)</span>

We also setup alerts based on derivative metrics (i.e. how much something grows or shrinks in a time period). For instance, we have one based on logins:_if Derivate(logins) > X => Send an alert to Slack_.

<span>![](https://lh4.googleusercontent.com/89B9jm28wMQ6WUiudbURq_WwqOWPDKEPLsUpg9Od5TaMHLoD2AcyI7Ldyhn7LzLPhy_uN7Lo_m0L3bozC1xFH3SUy0Dty28rYH8HymQXsai2itWv4InFfa2ivLbLFgwM)</span>

<span>Finally, we have alerts coming from</span> <span>NewRelic</span> <span>for infrastructure components.</span>

<span>![](https://lh5.googleusercontent.com/8Huf4E7ONHOq-5BYAKxyU42Ne8k-DV3r6moeAeVkcUwb51XD7gF86goeWFUnbdo3OzT4AUYsdRyeBjZUiNMyv2Lt0lM5C682IaRR9hxJXIXggLpd36J-1UMOrWgApVJu)</span>

<span>For logging we use</span> [<span>ElasticSearch</span>](http://www.elasticsearch.org/)<span>,</span> [<span>Logstash</span>](http://logstash.net/) <span>and</span> [<span>Kibana</span>](http://www.elasticsearch.org/overview/kibana/)<span>. We are storing logs from nginx and mongodb at this point. We are also parsing mongo logs using logstash in order to identify slow queries (anything with a high number of collscans).</span>

<span>![](https://lh5.googleusercontent.com/7blltHFmL_T0XqyL3of7MN16HQLglh2wJJRzasCSeVllaMzN9KYr1t98wErGZMjoH5au0S1UqnfnJPExlE4Zo-VdhJHaVpJ7zkoCB3uQeYvTZDJsJ5YfxmBVhmBVK6iZ)</span>

## <span>Website</span>

*   <span>All related web properties: the auth0.com site, our blog, etc. run completely separate from the app and runtime, on their own Ubuntu + Docker VMs.</span>

## <span>Future</span>

<span>This is where we are going:</span>

*   <span>We are moving to CoreOS and Docker. We want to move to a model where we manage clusters as a whole instead of doing configuration management over individual nodes. Docker helps also by removing some moving parts by doing image-based deployment (and be able to rollback at that level as well).</span>

*   <span>MongoDB will be geo-replicated across DCs between AWS and Azure. We are testing latency.</span>

*   <span>For all the search related features we are moving to ElasticSearch to provide search based on any criteria. MongoDB didn't work out well in this scenario (given our multi-tenancy).</span>

</div>