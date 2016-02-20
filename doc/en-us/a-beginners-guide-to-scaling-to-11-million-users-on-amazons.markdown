## [A Beginner's Guide to Scaling to 11 Million+ Users on Amazon's AWS](/blog/2016/1/11/a-beginners-guide-to-scaling-to-11-million-users-on-amazons.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, January 11, 2016 at 8:56AM</span></div>

<div class="body">

![](https://c2.staticflickr.com/2/1546/23690555693_2aa3c8c0f1_o.png)

How do you scale a system from one user to more than 11 million users? [Joel Williams](https://www.linkedin.com/in/joel-williams-70257b7), Amazon Web Services Solutions Architect, gives an excellent talk on just that subject: [AWS re:Invent 2015 Scaling Up to Your First 10 Million Users](https://www.youtube.com/watch?v=vg5onp8TU6Q&list=PLhr1KZpdzukdRxs_pGJm-qSy5LayL6W_Y).

<span>If you are an advanced AWS user this talk is not for you, but it’s a great way to get started if you are new to AWS, new to the cloud, or if</span> you haven’t kept up with with constant stream of new features Amazon keeps pumping out.

As you might expect since this is a talk by Amazon that Amazon services are always front and center as the solution to any problem. Their platform play is impressive and instructive. It's obvious by how the pieces all fit together Amazon has done a great job of mapping out what users need and then making sure they have a product in that space. 

Some of the interesting takeaways:

*   Start with SQL and only move to NoSQL when necessary.
*   A consistent theme is take components and separate them out. This allows those components to scale and fail independently. It applies to breaking up tiers and creating microservices.
*   Only invest in tasks that differentiate you as a business, don't reinvent the wheel.
*   Scalability and redundancy are not two separate concepts, you can often do both at the same time.
*   There's no mention of costs. That would be a good addition to the talk as that is one of the major criticisms of AWS solutions.

## <span>The Basics</span>

*   <span>AWS is in 12 regions around the world.</span>

    *   <span>A Region is a physical location in the world where Amazon has multiple Availability Zones. There are</span> [<span>regions in</span>](https://aws.amazon.com/about-aws/global-infrastructure/)<span>: North America; South America; Europe; Middle East; Africa; Asia Pacific.</span>

    *   <span>An Availability Zone (AZ) is generally a single datacenter, though they can be constructed out of multiple datacenters.</span>

    *   <span>Each AZ is separate enough that they have separate power and Internet connectivity.</span>

    *   <span>The only connection between AZs is a low latency network. AZs can be 5 or 15 miles apart, for example. The network is fast enough that your application can act like all AZs are in the same datacenter.</span>

    *   <span>Each Region has at least two Availability Zones. There are 32 AZs total.</span>

    *   <span>Using AZs it’s possible to create a high availability architecture for your application.</span>

    *   <span>At least 9 more Availability Zones and 4 more Regions are coming in 2016.</span>

*   <span>AWS has 53 edge locations around the world.</span>

    *   <span>Edge locations are used by CloudFront, Amazon’s Content Distribution Network (CDN) and Route53, Amazon’s managed DNS server.</span>

    *   <span>Edge locations enable users to access content with a very low latency no matter where they are in the world.</span>

*   <span>Building Block Services</span>

    *   <span>AWS has created a number of services that use multiple AZs internally to be highly available and fault tolerant. Here </span>[<span>is a list</span>](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) <span>of what services are</span> [<span>available where</span>](http://docs.aws.amazon.com/general/latest/gr/rande.html)<span>.</span>

    *   <span>You can use these services in your application, for a fee, without having to worry about making them highly available yourself.</span>

    *   <span>Some services that exist within an AZ: CloudFront, Route 53, S3, DynamoDB, Elastic Load Balancing, EFS, Lambda, SQS, SNS, SES, SWF.</span>

    *   <span>A highly available architecture can be created using services even though they exist within a single AZ.</span>

## <span>1 User</span>

*   <span>In this scenario you are the only user and you want to get a website running.</span>

*   <span>Your architecture will look something like:</span>

    *   <span>Run on a single instance, maybe a type</span> [<span>t2.micro</span>](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/t2-instances.html)<span>. Instance types comprise varying combinations of CPU, memory, storage, and networking capacity and give you the flexibility to choose the appropriate mix of resources for your applications.</span>

    *   <span>The one instance would run the entire</span> [<span>web stack</span>]( http://whatis.techtarget.com/definition/Web-stack)<span>, for example: web app, database, management, etc.</span>

    *   <span>Use Amazon</span> [<span>Route 53</span>](https://aws.amazon.com/route53/) <span>for the DNS.</span>

    *   <span>Attach a single</span> [<span>Elastic IP</span>](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) <span>address to the instance.</span>

    *   <span>Works great, for a while.</span>

## <span>Vertical Scaling</span>

*   <span>You need a bigger box. Simplest approach to scaling is choose a larger instance type. Maybe a</span> [<span>c4.8xlarge</span>](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/c4-instances.html) <span>or</span> [<span>m3.2xlarge</span>](https://aws.amazon.com/ec2/instance-types/)<span>, for example.</span>

*   <span>This approach is called</span> [<span>vertical scaling</span>](https://en.wikipedia.org/wiki/Scalability)<span>.</span>

*   <span>Just stop your instance and choose a new instance type and you’re running with more power.</span>

*   <span>There is a wide mix of different hardware configurations to choose from. You can have a system with 244 gigs of RAM (2TB of RAM types are coming soon). Or one with 40 cores. There are High I/O instances, High CPU Instances, High storage instances.</span>

*   <span>Some Amazon services come with a</span> [<span>Provisioned IOPS</span>](http://serverfault.com/questions/580568/amazon-how-do-i-know-if-i-need-provisioned-iops) <span>option to guarantee performance. The idea is you can perhaps use a smaller instance type for your service and make use of Amazon services like DynamoDB that can deliver scalable services so you don’t have to.</span>

*   <span>Vertical scaling has a big problem: there’s no failover, no redundancy. If the instance has a problem your website will die. All your eggs are in one basket.</span>

*   <span>Eventually a single instances can only get so big. You need to do something else.</span>

## <span>Users > 10</span>

*   <span>Separate out a single</span> <span>host into multiple hosts</span>

    *   <span>One host for the web site.</span>

    *   <span>One host for the database. Run any database you want, but you are on the hook for the database administration.</span>

    *   <span>Using separate hosts allows the web site and the database to be scaled independently of each other. Perhaps your database will need a bigger machine than your web site, for example.</span>

*   <span>Or instead of running your own database you could use a database service.</span>

    *   <span>Are you a database admin? Do your really want to worry about backups? High availability? Patches? Operating systems?</span>

    *   <span>A big advantage of using a service is you can have a multi Availability Zone database setup with a single click. You won’t have to worry about replication or any of that sort of thing. Your database will be highly available and reliable.</span>

*   <span>As you might imagine Amazon has several  fully managed database services to sell you:</span>

    *   [<span>Amazon RDS</span>](https://aws.amazon.com/rds/) <span>(Relational Database Service). There are many options: Microsoft SQL Server, Oracle, MySQL, PostgreSQL, MariaDB, Amazon Aurora.</span>

    *   [<span>Amazon DynamoDB</span>](https://aws.amazon.com/dynamodb/)<span>. A NoSQL managed database.</span>

    *   [<span>Amazon Redshift</span>](https://aws.amazon.com/redshift/)<span>. A petabyte scale data warehouse system.</span>

*   <span>More</span> [<span>Amazon Aurora</span>](https://aws.amazon.com/rds/aurora/)<span>:</span>

    *   <span>Automatic storage scaling up to 64TB. You no longer have to provision the storage for your data.</span>

    *   <span>Up to 15 read read-replicas</span>

    *   <span>Continuous (incremental) backups to S3.</span>

    *   <span>6-way replication across 3 AZs. This helps you handle failure.</span>

    *   <span>MySQL compatible.</span>

*   <span>Start with a SQL database instead of a NoSQL database.</span>

    *   <span>The suggestion is to start with a SQL database.</span>

    *   <span>The technology is established.</span>

    *   <span>There’s lots of existing code, communities, support groups, books, and tools.</span>

    *   <span>You aren’t going to break a SQL database with your first 10 million users. Not even close. (unless your data is huge).</span>

    *   <span>Clear patterns to scalability.</span>

*   <span>When might you need start with a NoSQL database?</span>

    *   <span>If you need to store > 5 TB of data in year one or you have an incredibly data intensive workload.</span>

    *   <span>Your application has super low-latency requirements.</span>

    *   <span>You need really high throughput. You need to really tweak the IOs you are getting both on the reads and the writes.</span>

    *   <span>You don’t have any relational data.</span>

## <span>Users > 100</span>

*   <span>Use a</span> <span>separate host for the web tier</span><span>.</span>

*   <span>Store the</span> <span>database on Amazon RDS</span><span>. It takes care of everything.</span>

*   <span>That’s all you have to do.</span>

## <span>Users > 1000</span>

*   <span>As architected your application has availability issues. If the host for your web service fails then your web site goes down.</span>

*   <span>So you</span> <span>need another web instance in another Availability Zone</span><span>. That’s OK because the latency between the AZs is in the low single digit milliseconds, almost like they right next to each other.</span>

*   <span>You also need to a</span> <span>slave database to RDS</span> <span>that runs in another AZ. If there’s a problem with the master your application will automatically switch over to the slave. There are no application changes necessary on the failover because your application always uses the same endpoint.</span>

*   <span>An Elastic Load Balancer (ELB) is added to the configuration to load balance users between your two web host instances in the two AZs.</span>

*   <span>Elastic Load Balancer (ELB):</span>

    *   <span>ELB is a highly available managed load balancer. The ELB exists in all AZs. It’s a single DNS endpoint for your application. Just put it in Route 53 and it will load balance across your web host instances.</span>

    *   <span>The ELB has Health Checks that make sure traffic doesn’t flow to failed hosts.</span>

    *   <span>It scales without your doing anything. If it sees additional traffic it scales behind the scenes both horizontally and vertically. You don’t have to manage it. As your applications scales so is the ELB.</span>

## <span>Users > 10,000s - 100,000s</span>

*   <span>The previous configuration has 2 instances behind the ELB, in practice you can have 1000s of instances behind the ELB. This is</span> [<span>horizontal scaling</span>](https://en.wikipedia.org/wiki/Scalability)<span>.</span>

*   <span>You’ll need to add more read replicas to the database, to RDS. This will take load off the write master.</span>

*   <span>Consider performance and efficiency by</span> <span>lightening the load off your web tier</span> <span>servers by moving some of the traffic elsewhere. Move static content in your web app to Amazon S3 and Amazon CloudFront. CloudFront is the Amazon’s CDN that stores your data in the 53 edge locations across the world.</span>

*   <span>Amazon S3 is an object base store.</span>

    *   <span>It’s not like EBS, it’s not storage that’s attached to an EC2 instance, it’s an object store, not a block store.</span>

    *   <span>It’s a great place to store static content, like javascript, css, images, videos. This sort of content does not need to sit on an EC2 instance.</span>

    *   <span>Highly durable, 11 9’s of reliability.</span>

    *   <span>Infinitely scalable, throw as much data as it as you want. Customers store multiple petabytes of data in S3\.</span>

    *   <span>Objects of up to 5TB in size are supported.</span>

    *   <span>Encryption is supported. You can use Amazon’s encryption, your encryption, or an encryption service.</span>

*   <span>Amazon CloudFront  is cache for your content.</span>

    *   <span>It caches content at the edge locations to provide your users the lowest latency access possible.</span>

    *   <span>Without a CDN your users will experience higher latency access to your content. Your servers will also be under higher load as they are serving the content as well as handling the web requests.</span>

    *   <span>One customer needed to serve content at 60 Gbps. The web tier didn’t even know that was going on, CloudFront handled it all.</span>

*   <span>You can also lighten the load by shifting session state off your web tier.</span>

    *   <span>Store the session state in</span> [<span>ElastiCache</span>](https://aws.amazon.com/elasticache/) <span>or DynamoDB.</span>

    *   <span>This approach also sets your system up to support auto scaling in the future.</span>

*   <span>You can also lighten the load by caching data from your database into ElastiCache.</span>

    *   <span>Your database doesn’t need to handle all the gets for data. A cache can handle a lot of that work and leaves the database to handle more important traffic.</span>

*   <span>Amazon DynamoDB - A managed NoSQL database</span>

    *   <span>You provision the throughput you want. You dial up the read and write performance you want to pay for.</span>

    *   <span>Supports fast, predictable performance.</span>

    *   <span>Fully distributed and fault tolerant. It exists in multiple Availability Zones.</span>

    *   <span>It’s a key-value store. JSON is supported.</span>

    *   <span>Documents up to 400KB in size are supported.</span>

*   <span>Amazon Elasticache - a managed Memcached or Redis</span>

    *   <span>Managing a memcached cluster isn’t making you more money so let Amazon do that for you. That’s the pitch.</span>

    *   <span>The clusters are automatically scaled for you. It’s a self-healing infrastructure, if nodes fail new nodes are started automatically.</span>

*   <span>You can also lighten the load by shifting dynamic content to CloudFront.</span>

    *   <span>A lot of people know CloudFront can handle static content, like files, but it can also handle some dynamic content. This topic is not discussed further in the talk, but here’s</span> [<span>a link</span>](https://aws.amazon.com/cloudfront/dynamic-content/)<span>.</span>

## [<span>Auto Scaling</span>](https://aws.amazon.com/autoscaling/)

*   <span>If you provision enough capacity to always handle your peak traffic load, Black Friday, for example, you are wasting money. It would be better to match compute power with demand. That’s what Auto Scaling let’s you do, the automatic resizing of compute clusters.</span>

*   <span>You can define the minimum and maximum size of your pools. As a user you decide what’s the smallest number of instances in your cluster and the largest number of instances.</span>

*   [<span>CloudWatch</span>](https://aws.amazon.com/cloudwatch/) <span>is a management service that’s embedded into all applications.</span>

    *   <span>CloudWatch events drive scaling.</span>

    *   <span>Are you going to scale on CPU utilization? Are you going to scale on latency? On network traffic?</span>

    *   <span>You can also push your own custom metrics into CloudWatch. If you want to scale on something application specific you can push that metric into CloudWatch and then tell Auto Scaling you want to scale on that metric.</span>

## <span>Users > 500,000+</span>

*   <span>The addition from the previous configuration is</span> [<span>auto scaling groups</span>](http://docs.aws.amazon.com/gettingstarted/latest/wah/getting-started-create-as.html) <span>are added to the web tier</span><span>. The auto scaling group includes the two AZs, but can expand to 3 AZs, up to as many as are in the same region. Instances can pop up in multiple AZs not just for scalability, but for availability.</span>

*   <span>The example has 3 web tier instances in each AZ, but it could be thousands of instances. You could say you want a minimum of 10 instances and a maximum of a 1000.</span>

*   <span>ElastiCache is used to offload popular reads from the database.</span>

*   <span>DynamoDB is used to offload Session data.</span>

*   <span>You need to add monitoring, metrics and logging.</span>

    *   <span>Host level metrics. Look at a single CPU instance within an autoscaling group and figure out what’s going wrong.</span>

    *   <span>Aggregate level metrics. Look at metrics on the Elastic Load Balancer to get feel for performance of the entire set of instances.</span>

    *   <span>Log analysis. Look at what the application is telling you using</span> [<span>CloudWatch</span>](https://aws.amazon.com/about-aws/whats-new/2014/07/10/introducing-amazon-cloudwatch-logs/) <span>logs.</span> [<span>CloudTrail</span>](https://aws.amazon.com/cloudtrail/) <span>helps you analyze and manage logs.</span>

    *   <span>External Site Performance. Know what your customers are seeing as end users. Use a service like New Relic or Pingdom.</span>

*   <span>You need to know what your customers are saying. Is their latency bad? Are they getting an error when they go to your web page?</span>

*   <span>Squeeze as much performance as you can from your configuration. Auto Scaling can help with that. You don’t want systems that are at 20% CPU utilization.</span>

## <span>Automation</span>

*   <span>The infrastructure is getting big, it can scale to 1000s of instances. We have read replicas, we have horizontal scaling, but we need some automation to help manage it all, we don’t want to manage each individual instance.</span>

*   <span>There’s a hierarchy of automation tools.</span>

    *   <span>Do it yourself</span><span>: Amazon EC2, AWS CloudFormation.</span>

    *   <span>Higher-level services</span><span>: AWS Elastic Beanstalk, AWS OpsWorks</span>

*   [<span>AWS Elastic Beanstalk</span>](https://aws.amazon.com/elasticbeanstalk/)<span>: manages the infrastructure for your application automatically. It’s convenient but there’s not a lot of control.</span>

*   [<span>AWS OpsWorks</span>](https://aws.amazon.com/opsworks/)<span>: an environment where you build your application in layers, you use Chef recipes to manage the layers of your application.</span>

    *   <span>Also enables the ability to do Continuous Integration and deployment.</span>

*   [<span>AWS CloudFormation</span>](https://aws.amazon.com/cloudformation/)<span>: been around the longest.</span>

    *   <span>Offers the most flexibility because it offers a templatized view of your stack. It can be used to build your entire stack or just components of the stack.</span>

    *   <span>If you want to update your stack you update the Cloud Formation template it will update just that one piece of your application.</span>

    *   <span>Lots of control, but less convenient.</span>

*   [<span>AWS CodeDeploy</span>](https://aws.amazon.com/codedeploy/)<span>: Deploys your code to a fleet of EC2 instances.</span>

    *   <span>Can deploy to one or thousands of instances.</span>

    *   <span>Code Deploy can point to an auto scaling configuration so code is deployed to a group of instances.</span>

    *   <span>Can also be used in conjunction with Chef and Puppet.</span>

## <span>Decouple Infrastructure</span>

*   <span>Use</span> [<span>SOA</span>](https://en.wikipedia.org/wiki/Service-oriented_architecture)<span>/</span>[<span>microservices</span>](http://techblog.netflix.com/2015/02/a-microscope-on-microservices.html)<span>.  Take components from your tiers and separate them out.</span> <span>Create separate services</span> <span>like when you separated the web tier from the database tier.</span>

*   <span>The individual services can then be scaled independently. This gives you a lot of flexibility for scaling and high availability.</span>

*   <span>SOA is a key component of the architectures built by Amazon.</span>

*   <span>Loose coupling sets you free.</span>

    *   <span>You can scale and fail components independently.</span>

    *   <span>If a worker node fails in pulling work from SQS does it matter? No, just start another one. Things are going to fail, let’s build an architecture that handles failure.</span>

    *   <span>Design everything as a black box.</span>

    *   <span>Decouple interactions.</span>

    *   <span>Favor services with built-in redundancy and scalability rather than building your own.</span>

## <span>Don’t Reinvent the Wheel</span>

*   <span>Only invest in tasks that differentiate you as a business.</span>

*   <span>Amazon has a lot of services that are inherently fault tolerant because they span multiple AZs. For example: queuing, email, transcoding, search, databases, monitoring, metrics, logging, compute. You don’t have to build these yourself.</span>

*   [<span>SQS</span>](https://aws.amazon.com/sqs/)<span>: queueing service.</span>

    *   <span>The first Amazon service offered.</span>

    *   <span>It spans multiple AZs so it’s fault tolerant.</span>

    *   <span>It’s scalable, secure, and simple.</span>

    *   <span>Queuing can help your infrastructure by helping you pass messages between different components of your infrastructure.</span>

    *   <span>Take for example a Photo CMS. The systems that collects the photos and processes them should be two different systems. They should be able to scale independently. They should be loosely coupled. Ingest a photo, put it in queue, and workers can pull photos off the queue and do something with them.</span>

*   [<span>AWS Lambda</span>](https://aws.amazon.com/lambda/)<span>: lets you run code without provisioning or managing servers.</span>

    *   <span>Great tool for allowing you to decouple your application.</span>

    *   <span>In the Photo CMS example Lambda can respond to S3 events so when a S3 file is added the Lambda function to process is automatically triggered.</span>

    *   <span>We’ve done away with EC2\. It scales out for you and there’s no OS to manage.</span>

## <span>Users > 1,000,000+</span>

*   <span>Reaching a million users and above requires bits of all the previous points:</span>

    *   <span>Multi-AZ</span>

    *   <span>Elastic Load Balancing between tiers. Not just on the web tier, but also on the application tier, data tier, and any other tier you have.</span>

    *   <span>Auto Scaling</span>

    *   <span>Service Oriented Architecture</span>

    *   <span>Serve Content Smartly with S3 and CloudFront</span>

    *   <span>Put caching in front of the DB</span>

    *   <span>Move state off the web tier.</span>

*   <span>Use</span> [<span>Amazon SES</span>](https://aws.amazon.com/ses/) <span>to send email.</span>

*   <span>Use CloudWatch for monitoring.</span>

## <span>Users > 10,000,000+</span>

*   <span>As we get bigger we’ll hit issues in the data tier. You will potentially start to run into issues with your database around contention with the</span> [<span>write master</span>](https://en.wikipedia.org/wiki/Multi-master_replication)<span>, which basically means you can only send so much write traffic to one server.</span>

*   <span>How do you solve it?</span>

    *   <span>Federation</span>

    *   <span>Sharding</span>

    *   <span>Moving some functionality to other types of DBs (NoSQL, graph, etc)</span>

*   <span>Federation - splitting into multiple DBs based on function</span>

    *   <span>For example, create a Forums Database, a User Database, a Products Database. You might have had these in a single database before, now spread them out.</span>

    *   <span>The different databases can be scaled independently of each other.</span>

    *   <span>The downsides: you can’t do cross database queries; it delays getting to the next strategy, which is sharding.</span>

*   <span>Sharding -  splitting one dataset across multiple hosts</span>

    *   <span>More complex at the application layer, but there’s no practical limit on scalability.</span>

    *   <span>For example, in a Users Database ⅓ of the users might be sent to one shard, and the last third to another shard, and another shard to another third.</span>

*   <span>Moving some functionality to other types of DBs</span>

    *   <span>Start thinking about a NoSQL database.</span>

    *   <span>If you have data that doesn’t require complex joins, like say a leaderboard, rapid ingest of clickstream/log data, temporary data, hot tables, metadata/lookup tables, then consider moving it to a NoSQL database.</span>

    *   <span>This means they can be scaled independently of each other.</span>

## <span>Users > 11 Million</span>

*   <span>Scaling is an iterative process. As you get bigger there's always more you can do.</span>

*   <span>Fine tune your application.</span>

*   <span>More SOA of features/functionality.</span>

*   <span>Go from Multi-AZ to multi-region.</span>

*   <span>Start to build custom solutions to solve your particular problem that nobody has ever done before. If you need to serve a billion customers you may need custom solutions.</span>

*   <span>Deep analysis of your entire stack.</span>

## <span>In Review</span>

*   <span>Use a multi-AZ infrastructure for reliability.</span>

*   <span>Make use of self-scaling services like ELB, S3, SQS, SNS, DynamoDB, etc.</span>

*   <span>Build in redundancy at every level. Scalability and redundancy are not two separate concepts, you can often do both at the same time.</span>

*   <span>Start with a traditional relational SQL database.</span>

*   <span>Cache data both inside and outside your infrastructure.</span>

*   <span>Use automation tools in your infrastructure.</span>

*   <span>Make sure you have good metrics/monitoring/logging in place. Make sure you are finding out what your customers experience with your application.</span>

*   <span>Split tiers into individual services (SOA) so they can scale and fail independently of each other.</span>

*   <span>Use Auto Scaling once you’re ready for it.</span>

*   <span>Don’t reinvent the wheel, use a managed service instead of coding your own, unless it’s absolutely necessary.</span>

*   <span>Move to NoSQL if and when it makes sense.</span>

## <span>Further Reading</span>

*   [On HackerNews](https://news.ycombinator.com/item?id=10885727) / [On Reddit](https://www.reddit.com/r/sysadmin/comments/40hon7/a_beginners_guide_to_scaling_to_11_million_users/)

*   <span>[http://aws.amazon.com/documentation](http://aws.amazon.com/documentation/)</span>

*   [<span>http://aws.amazon.com/architecture</span>](http://aws.amazon.com/architecture/)

*   [<span>http://aws.amazon.com/start-ups</span>](http://aws.amazon.com/start-ups)

*   <span>[http://aws.amazon.com/free](http://aws.amazon.com/free)</span>

*   From 2007: [Amazon Architecture](http://highscalability.com/blog/2007/9/18/amazon-architecture.html)

</div>