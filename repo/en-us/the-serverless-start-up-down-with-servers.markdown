## [The Serverless Start-up - Down with Servers!](/blog/2015/12/7/the-serverless-start-up-down-with-servers.html)

    

    

[![teletext.io](https://teletext.io/storage/logo_no_text.png)](https://teletext.io/ "Teletext.io - content as a service")

_This is a guest post by [Marcel Panse](http://www.linkedin.com/in/marcelpanse "Marcel Panse") and [Sander Nagtegaal](http://www.linkedin.com/in/centrical "Sander Nagtegaal") from [Teletext.io](https://teletext.io/ "Teletext.io")._

In our early [Peecho days](http://www.peecho.com/ "Peecho"), we [wrote an article](http://highscalability.com/blog/2011/8/1/peecho-architecture-scalability-on-a-shoestring.html "Peeacho architecture - scalability on a shoestring") explaining how to build a really scalable architecture for next to nothing, using [Amazon Web Services](http://aws.amazon.com/ "Amazon Web Services"). Auto-scaling, merciless decoupling and even automated bidding on unused server capacity were the tricks we used back then to operate on a shoestring. Now, it is time to take it one step further.

We would like to introduce [Teletext.io](https://teletext.io/ "Teletext.io - content as a service"), also known as _the serverless start-up_ - again, entirely built around AWS, but leveraging only the [Amazon API Gateway](https://aws.amazon.com/api-gateway/ "Amazon API Gateway"), [Lambda](https://aws.amazon.com/lambda/ "AWS Lambda") functions, [DynamoDb](https://aws.amazon.com/dynamodb/ "AWS DynamoDb"), [S3](https://aws.amazon.com/s3/ "AWS S3") and [Cloudfront](https://aws.amazon.com/cloudfront/ "AWS Cloudfront").

## The Virtues of Constraint

We like rules. At our previous start-up [Peecho](http://www.peecho.com/ "Peecho printing"), product owners had to do _fifty push-ups_ as payment for each user story that they wanted to add to an ongoing sprint. Now, at our current company [myTomorrows](https://mytomorrows.com/ "myTomorrows - expanding access to drugs in development"), our _developer dance-offs_ are legendary: during the daily stand-ups, you are only allowed to speak _while dancing_ - leading to the most efficient meetings ever.

This way of thinking goes all the way into our product development. It may seem counter-intuitive at first, but constraints fuel creativity. For example, all our logo design is done with technical diagramming tool [Omnigraffle](https://www.omnigroup.com/omnigraffle "Omnigraffle"), so there is no way we could use hideous _lens flares_ and such. Anyway - recently, we launched yet another initiative called [Teletext.io](https://teletext.io/ "Teletext.io"). So, we needed a new restriction.

_At Teletext.io, we are not allowed to use servers. Not even one._

It was a good choice. We will explain why.

## Why Servers are Bad

In the past years, the Amazon cloud already made us very happy with the ability to use auto-scaled clustering of EC2 server instances with a load balancer on top. This means that if one of your servers goes down, a new one can be initiated automatically. The same happens when unexpectedly high peaks of traffic occur. Extra servers will then be initiated.

Although this is pretty cool, there are disadvantages.

*   First of all, you **need to keep a minimum number of server instances alive** to be able to serve any visitor, even if you don't have any traffic - or revenue. This costs money.
*   Secondly, because cloud instances run installed software on top of operating systems, you need not only maintain your own code, but also **make sure the server software stays up to date** and operational.
*   Third, you **can't scale up or down in a granular way**, but only one whole server at a time.

In an architecture based on an API with micro-services, this means a lot of overhead for small tasks. Luckily, there are now options to solve this. Let's first take a look at the problem at hand.

## Scratch Your own Itch

Our brand new solution was born out of personal frustration about _content management_ within custom software. In most start-ups, HTML texts inside buttons, dashboards, help sections or even entire web pages have to be managed and deployed by _programmers_ instead of the writers of the texts. This is really annoying for both developers and editors.

For years, we tried to find a distributed content management service that would solve this issue properly. We failed. So, a couple of months ago, we were fed up and decided that we would just have to build something ourselves.

## The Plan

We planned to create a really simple, Javascript-based service that could inject centrally hosted content, using the common data attributes of HTML for labeling of elements. It should be able to handle localization and dynamic insertion of data. Most importantly, it should give content writers a way to just change texts whenever they want, using an inline WYSIWYG editor in their own app.

Apart from some UX caveats, there are three technical challenges here.

1.  Because live websites rely on it for content, this new commodity service should _never_ go down. Ever.
2.  It should be really, really fast, so your visitors don't even notice the content is loading.
3.  The content should be indexed by Google.

The third issue is not related to architecture per se. The trusty search engine moves in mysterious ways, so all we could do was to [test the assumption](http://www.centrical.com/test/google-json-ld-and-javascript-crawling-and-indexing-test.html "Google crawling an indexing test for javacript insertion and JSON-LD structured data"). Spoiler: yes, it works. The solution to the first two problems is in your own hands, though. These can be solved at low cost - with a clever architecture.

## Building Blocks

Let's dive into the building blocks of our system, which are based on several of the latest features of Amazon Web Services.

### Amazon API Gateway

[Amazon API Gateway](https://aws.amazon.com/api-gateway/ "Amazon API Gateway") is a managed AWS service that allows developers to create APIs at any scale, with just a few clicks in the AWS Management Console. The service can trigger other AWS services, including Lambda functions.

### AWS Lambda

Instead of running cloud instances, we use [AWS Lambda](https://aws.amazon.com/lambda/ "AWS Lambda"). The name derives from the Greek letter lambda (    λ    ) used to denote binding a variable in a function. AWS Lambda lets you run code without maintaining any server instances. You may think of an atomic, stateless function with a single task, that may run for a limited amount of time (one minute, currently). The functions can be written in Javascript (Node.js), Python or Java.

If you upload your Lambda code, Amazon will take care of everything required to run and scale your code with high availability. Lambda executes in parallel. So, if a million requests are made, a million Lambda functions will execute without loss of speed or capacity. According to Amazon, "there are no fundamental limits to scaling a function".

The best thing is, that from a developer's perspective, Lambda functions are not even there when they don't get executed. They only appear when needed. And what isn't up, can't come down.

### DynamoDB

The Lambda functions put their data in a data store. Because our rule says that we are not allowed to use any servers, we can't use the Relational Database Service (RDS). Instead, we use DynamoDB, Amazon's giant managed data store.

[Amazon DynamoDB](https://aws.amazon.com/dynamodb/ "AWS DynamoDb") is a NoSQL database service for all applications at any scale. It is fully managed and supports both document and key-value store models. Scalability of the service has been proven with other users like Netflix, AirBnB and IMDb.

## Architecture

Our system is decoupled into three parts:

1.  Content management
2.  Content delivery
3.  Our website

This separation of concerns is by design. A problem in either the content management API or our own website should never lead to an outage of the customer's websites. Therefore, the delivery of content should be entirely autonomous.

### Content Management

[![Teletext.io - editing dynamic content with Amazon Gateway API, DynamoDb and Lambda](https://teletext.io/storage/teletext.io-content-editing-api-architecture-aws-lambda.png)](https://teletext.io/help/introduction "Teletext.io - editing dynamic content with Amazon Gateway API, DynamoDb and Lambda") The content management part is what editors use to edit HTML and texts. Editors can log in using their Google account, via an [AWS Cognito identity pool](https://aws.amazon.com/cognito/ "AWS Cognito") connected to [AWS IAM](https://aws.amazon.com/documentation/iam/ "AWS Identity and Access Management") using Javascript only. For loading draft content, storing edits and publishing the result, the Amazon API Gateway gets called, triggering Lambda functions. The Lambda functions all relate to a single API call and store their data in DynamoDb.  

As you can see, there are no servers that could crash or get stuck.

### Content Delivery

As mentioned, we decided to decouple the delivery of content entirely from the editing features, so your app will work even if disaster strikes. When an editor decides to publish new content to the live version of his app, another Lambda immediately copies the draft content as flat JSON files to [S3](https://aws.amazon.com/s3/ "AWS S3"), Amazon's data store for files. The JSON file contains meta data and i18n localized HTML strings that describe the content.

[![Teletext.io - publishing static content with S3 and Cloudfront](https://teletext.io/storage/teletext.io-content-delivery-architecture-aws-s3-cloudfront.png)](https://teletext.io/help/introduction "Teletext.io - publishing static content with S3 and Cloudfront") From there, the Teletext.io script in your app can access these files through the [Cloudfront](https://aws.amazon.com/cloudfront/ "AWS Cloudfront") CDN, ensuring high availability and performance. We added a clever prefetching algorithm to ensure the most popular files get retrieved and cached in your browser before you need them, so the next clicks are provisioned without actual load of content.  

Since there is no server-side logic involved in the delivery of published content, it is really fast and practically bullet proof.

### Our Website

[![Teletext.io static single page app in S3 with proper routing for deeplinking](https://teletext.io/storage/teletext.io-website-architecture-s3-cloudfront-reactjs.png)](https://teletext.io/ "Teletext.io static single page app in S3 with proper routing for deeplinking") What about [our website](https://teletext.io/ "Teletext.io - content management as a service"), though? We chose a simple, but effective concept - again, without servers. The website was built using the [React framework](https://facebook.github.io/react/ "Facebook React JS") as a single-page app and [deployed to S3 as a single, static file](http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html "Static single-page website in S3"). Then, Cloudfront was configured as the content delivery mechanism on top, ensuring super-fast content delivery from many endpoints around the world.  

Again, this approach is based on flat file delivery and therefore very robust.

The static app uses HTML5 pushState and React Router for URL handling. Usually, there is one problem with that. If you access a specific URL other than the root, a web server must dynamically render the same routes that the front end is rendering dynamically. This is currently impossible in S3\. We found a trick, though, that we would like to share here.

1.  Configure the app as [a static website in S3](http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html "Static single-page app in S3"), with the root pointing to the main file.
2.  Do not add any S3 redirect rules. Donâ€™t even add the custom error page.
3.  Create a Cloudfront distribution pointing to the S3 bucket.
4.  Create a custom error response in Cloudfront, that points to the main file as well. Make sure to enforce a fixed 200 response.

The result is that all URL paths (except for the root) lead to a 404 response in S3, which then triggers the cached custom error response of Cloudfront. This final response is just your single-page app again. Now, in the browser, you can handle all routing based on the current path.

There is only one disadvantage. You can't return an actual 404 HTTP response code in any case. However, in return, you will get an ultra-cheap, ultra-scalable single-page app.

### Practical Perks

Working with Lambda has impact on your development process. Support is improving, though. For example, previously, Lambda functions couldn't be versioned. This had the effect that testing and deploying was very risky. However, Amazon recently [rolled out their versioning system](http://docs.aws.amazon.com/lambda/latest/dg/versioning-aliases.html "Versioning AWS Lambda") and now we can use _mutable aliasing_. This means that it is now possible to have different versions of the same function that can be updated independently, similar to a test environment versus a production environment.

## The Result

Our freemium service is now [open for customers](https://teletext.io/ "Teletext.io - content management as a service"). We eat our own dog food, so we use it, too. In the following GIF you can see the functionality at work in our own website.

[![Teletext.io demo: inline content management inside custom software](https://teletext.io/storage/teletext.io-demo.gif)](https://teletext.io/ "Teletext.io - content management as a service")

However, the truly scalable nature of the system shows in our monthly AWS bill.

### Cost

The cost is entirely defined by actual usage. For simplicity's sake, we will ignore temporary free tiers, as well as the slew of smaller services that we use.

*   With **Lambda**, you **pay only for the compute time you consume** - there is no charge when your code is not running. There is a perpetual free tier, too.
*   **Amazon API Gateway** has **no minimum fees or startup costs**. You pay only for the API calls you receive and the amount of data transferred out.
*   **DynamoDb** costs are based on **pay-for-what-you-use** as well, although the pricing is a bit more complicated. To put it simply, it's based on storage, reads and writes.
*   Then, there are S3 and Cloudfront. Basically, you pay for storage and bandwidth out.

We just started - so to calculate the costs, we made some assumptions. Let's consider a fairly big website to be our average customer. We guess that such a client uses 1000 API calls per month (that's editing only), requires therefore 1GB of data in, and needs about 10 GB of traffic-related data out. Persistent storage we estimate to be 500MB. We don't expect the Lambda execution time to get over 2 seconds.

For several different numbers of this type of customer, our monthly cost would look like this (rounded, in American dollars).

<table style="width: 100%; text-align: left; border-collapse: collapse; border-spacing: 0; margin-bottom: 20px;">

<thead style="font-weight: bold; background-color: #efefef;">

<tr>

<th>Customers</th>

<th>API Gateway</th>

<th>Lambda</th>

<th>DynamoDb</th>

<th>S3</th>

<th>Cloudfront</th>

<th>Total</th>

</tr>

</thead>

<tbody>

<tr>

<td>0</td>

<td>0</td>

<td>0</td>

<td>0</td>

<td>0.25</td>

<td>1.00</td>

<td style="font-weight: bold;">1.25</td>

</tr>

<tr>

<td>100</td>

<td>9.50</td>

<td>0</td>

<td>3.50</td>

<td>1.50</td>

<td>85.00</td>

<td style="font-weight: bold;">99.50</td>

</tr>

<tr>

<td>1000</td>

<td>93.50</td>

<td>4.00</td>

<td>3.50</td>

<td>15.00</td>

<td>850.00</td>

<td style="font-weight: bold;">966.00</td>

</tr>

<tr>

<td>10000</td>

<td>935.00</td>

<td>35.00</td>

<td>3.50</td>

<td>150.00</td>

<td>8050.00</td>

<td style="font-weight: bold;">9173.50</td>

</tr>

<tr>

<td>100000</td>

<td>9350.00</td>

<td>410.00</td>

<td>3.50</td>

<td>1500.00</td>

<td>76700.00</td>

<td style="font-weight: bold;">87963.50</td>

</tr>

</tbody>

</table>

As you can see, the **cost is largely determined by the CDN**, which is becoming cheaper at larger traffic numbers. The API and associated Lambdas attribute for a much smaller portion. So, if you would build a service that is less dependent on Cloudfront, you can do much better.

## Down with Servers

Using our love for creative constraint, we managed to launch a start-up without servers, auto-scaling, load balancers and operating systems to maintain. This is not only a really scalable solution with an almost infinite peak capacity, but also a very cheap solution - especially in the early days before traction hits.

So... Down with servers!

[On HackerNews](https://news.ycombinator.com/item?id=10690842) / [On Reddit](https://www.reddit.com/r/programming/comments/3vwrf4/on_high_scalability_building_a_company_using_only/)

    