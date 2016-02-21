## [iDoneThis - Scaling an Email-based App from Scratch](/blog/2012/6/20/idonethis-scaling-an-email-based-app-from-scratch.html)

    

    

![](http://farm9.staticflickr.com/8147/7402945462_2a8216aef6_m.jpg)

_This is a guest post by Rodrigo Guzman, CTO of [iDoneThis](http://idonethis.com), which makes status reporting happen at your company with the lightest possible touch._

**iDoneThis** is a simple management application that emails your team at the end of every day to ask, "What'd you get done today?" Just reply with a few lines of what you got done. The following morning everyone on your team gets a digest with what the team accomplished the previous day to keep everyone in the loop and kickstart another awesome day.

Before we launched, we built iDoneThis over a weekend in the most rudimentary way possible. I kid you not, we sent the first few batches of daily emails using the BCC field of a Gmail inbox. The upshot is that we’ve had users on the site from Day 3 of its existence on.

We’ve gone from launch in January 2011 when we sent hundreds of emails out per day by hand to sending out over 1 million emails and handling over 200,000 incoming emails per month. In total, customers have recorded over 1.7 million dones.

## Stats 

*   10k inbound emails/day
*   40k outgoing emails/day, most of them happen over the peak hours when it is 6p in the US and are expected to arrive as scheduled.
*   A few web-requests per second, in bursts. the web-server sits mostly idle.
*   1GB of user content, 5GB database
*   Documents are indexed for searching in real time
*   All of it is handled using a single xlarge EC2 instance.

## Stack 

*   python for almost everything
*   django for the web interface
*   apache + mod_wsgi
*   postgresql
*   incoming email is handled with the sendgrid parse API and a custom email processor written in python in-house
*   coffeescript, backbone, jquery, and sass
*   lucene + solr for searching running behind a jetty instance

## Email - From Gmail to SendGrid

We started out with BCC in Gmail because if we sent the emails from our application server, they would all get marked as spam.  Mailchimp didn’t allow for the amount of variation we wanted in our emails because it was built for traditional email marketing.  

We switched to a custom script (“sendmail”) on a cronjob with [SendGrid](http://sendgrid.com) when, to our surprise, hundreds of people signed up for the service.  Sendmail is what we still use today, with iterative improvements to handle the error conditions that have popped up.  

It used to crash once a week and now that happens almost never. To achieve this, sendmail went from being a simple for loop to using the database to keep track of the state of each email and having logic to deal with the different state transitions gracefully. It served us well to have it be a simple for loop at first, though -- it was much easier to design the machinery to make it reliable after watching all the common ways in which it failed.

To process incoming email, we started with a 200-line script we called “getmail” to access our Gmail inbox over IMAP.  The process was unreliable and would leave the database in a bad state after throwing an exception and thus had to be run by hand with someone babysitting it.  Not only that, but someone had to remove unwanted lines (signatures, reply text, etc) from the entries.  We built an incoming parser using the standard library that made our getmail process much more reliable. It was a hot mess of 800 lines of Python code devoted to dealing with correctly with encodings, parts, and heuristics for what makes a line unwanted.

We moved from getmail to SendGrid's incoming parse API, which basically turns incoming email into HTTP POST requests, so that we only had to worry about writing a web app. We switched because getmail could not process the incoming email fast enough due to rate-limiting from Google. To make matters worse, getmail was not designed to have multiple copies of it running simultaneously which started causing a host of problems when the incoming email rate became too large.

## Scale the Process

Scalability is a funny thing. Early on, the main bottleneck that we faced was not technical, per se, but developer time -- it was just me against a world of emails! The same is largely true today. This means that performance and scalability are built upon principles of simplicity in code design, thoughtful abstractions and agility. 

The first ingredient is to be ruthless about limiting the scope of features down to the bare minimum or outsourcing them altogether. Only after the feature is out the door and we see it being used we optimize it to make sure it is performant (e.g., we added [Celery](http://celeryproject.org/) to send notification emails asynchronously after that became sufficiently important part of the UX).

Second, make the code as simple (to write and read) as possible. This often involves making performance sacrifices. For instance, the ORM doesn’t always produce the most efficient SQL for complicated queries, but we set that aside until after the feature is in production. Admittedly this has led to a couple embarrassing moments, but more often it has kept us away from premature optimizations and our code base relatively clean and easy to work with.

As the number of incoming and outgoing emails increased we considered switching to a multi-server architecture. However, this started adding a lot of complexity to our continuous deployment library. So, instead of optimizing, we just bought a bigger computer and built a load and performance monitoring system. Currently iDoneThis runs on a single xlarge EC2 instance. We could get away with a few small instances by doing some work, but we prefer to optimize for simplicity and developer time instead.

The most important part of our process, though, may be that we have very good continuous deployment. Every developer can deploy any branch of the git repo to production with one short command and the process takes a few seconds. This means that what’s in production and what we’re developing on never diverge too much. It also means that we can iterate quickly after watching what happens live.

## Re-Architecting for the Modern Web

The web interface for iDoneThis started as a very simple Django project. Just a few models, views, and templates. As the product evolved we continued to embrace the old paradigm of web-application development: everything happened with page loads and form submissions. This served us well for some time, but as our users put data in our system they wanted better interfaces to access it. 

We slowly started piling up the jQuery spaghetti along with the backend functions needed to support that. The result was a hot mess. One that worked, but that was difficult and depressing to debug and iterate on.

Again, choosing to introduce only as much complexity as absolutely necessary, we re-wrote the frontend using CoffeeScript and Backbone.js which yielded much more manageable, organized code. When we took this step we largely left that backend alone, only adding support as needed for the new features of the frontend. This turned out to be problematic.

Since iDoneThis is largely email based and later introduced an iPhone app, we ended up with three channels for user data i/o: email, web, and iphone app -- each comes with its own set of nuances. To us, this makes it such that several of our user interactions and flows don’t quite work the same way you’d expect on a normal web app, the way that Django abstractions make easy. For example, when a user is invited to a team he starts receiving emails from us and can just reply to them like anyone else, but he has never visited the site to set up an account.

As we built out the backend to support all this, the code started becoming more and more unruly. We embraced TDD as much as possible to help cope with that, but more importantly, we decided to switch to an API-based architecture -- we’re still amidst making this switch. 

The way this looks is that the Django backend largely consists of two components: the one in charge of serving a json API that can be consumed by Backbone.js and the iPhone app and a set of simple views and templates in charge of serving the frontend code and html placeholders.

So far, the code that is written in this style is far simpler than it’s old-fashioned equivalent and more easily tested. However, it has been a pretty big time investment to figure out the ins and outs of making the switch since django wasn’t quite designed for this paradigm. The fact that the new code is much cleaner makes me think it will all pay off in the end in terms of iteration speed.

## Lessons Learned

*   **Outsource the hard work, outsource as much as possible**. Even for an email-based product it makes sense to use sendgrid for both outgoing and incoming email -- that's what we do. Of course, at some point it makes sense to do these things in house, but for a startup that point might as well be t = infinity.
*   **Simplicity over performance**: we could probably get away with a smaller EC2 instance and use nginx, but the default apache configuration with mod_wsgi works just fine and it is simpler to automate. We do this sort of a thing a lot: get the simplest and easiest thing out the door, worry about it's performance and polish afterwards.
*   For a short while we were moving in the direction of splitting components to run in their own servers. This started causing more trouble than it was worth and thus we ended up with a **single EC2 instance**.

    