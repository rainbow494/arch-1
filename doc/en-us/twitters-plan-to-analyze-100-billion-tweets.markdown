## [Twitter’s Plan to Analyze 100 Billion Tweets](/blog/2010/2/19/twitters-plan-to-analyze-100-billion-tweets.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Friday, February 19, 2010 at 7:41AM</span></div>

<div class="body">

![](http://farm5.static.flickr.com/4057/4370730318_4beb5e645c_m.jpg) If Twitter is the “nervous system of the web” as some people think, then what is the brain that makes sense of all those signals (tweets) from the nervous system? That brain is the Twitter Analytics System and Kevin Weil, as Analytics Lead at Twitter, is the homunculus within in charge of figuring out what those over 100 billion tweets (approximately the number of neurons in the human brain) mean.

Twitter has only 10% of the expected 100 billion tweets now, but a good brain always plans ahead. Kevin gave a talk, [Hadoop and Protocol Buffers at Twitter](http://www.slideshare.net/kevinweil/protocol-buffers-and-hadoop-at-twitter), at the [Hadoop Meetup](http://www.meetup.com/hadoop/), explaining how Twitter plans to use all that data to an answer key business questions.

What type of questions is Twitter interested in answering? Questions that help them better understand Twitter. Questions like:

1.  How many requests do we serve in a day?
2.  What is the average latency?
3.  How many searches happen in day?
4.  How many unique queries, how many unique users, what is their geographic distribution?
5.  What can we tell about as user from their tweets?
6.  Who retweets more?
7.  How does usage differ for mobile users?
8.  What went wrong at the same time?
9.  Which features get users hooked?
10.  What is a user’s reputation?
11.  How deep do retweets go?
12.  Which new features worked?

And many many more. The questions help them understand Twitter, their analytics system helps them get the answers faster.

## Hadoop and Pig are Used for Analysis

Any question you can think of requires analyzing big data for answers. 100 billion is a lot of tweets. That’s why Twitter uses [Hadoop and Pig](http://www.slideshare.net/kevinweil/hadoop-pig-and-twitter-nosql-east-2009) as their analysis platform. Hadoop provides: key-value storage on a distributed file system, horizontal scalability, fault tolerance, and map-reduce for computation. Pig is a query a mechanism that makes it possible to write complex queries on top of Hadoop.

Saying you are using Hadoop is really just the beginning of the story. The rest of the story is what is the best way to use Hadoop? For example, how do you store data in Hadoop?

This may seem an odd question, but the answer has big consequences. In a relational database you don’t store the data, the database stores you, er, it stores the data for you. APIs move that data around in a row format.

Not so with Hadoop. Hadoop’s key-value model means it’s up to you how data is stored. Your choice has a lot to do with performance, how much data can be stored, and how agile you can be in reacting to future changes.

Each tweet has 12 fields, 3 of which have sub structure, and the fields can and will change over time as new features are added. What is the best way to store this data?

## Data is Stored in Protocol Buffers to Keep it Efficient and Flexible

Twitter considered CSV, XML, JSON, and Protocol Buffers as possible storage formats. Protocol Buffer _is a way of encoding structured data in an efficient yet extensible format. Google uses Protocol Buffers for almost all of its internal RPC protocols and file formats_. BSON (binary JSON) was not evaluated, but would probably not work because it doesn’t have an IDL (interface definition language.) [Avro](http://hadoop.apache.org/avro/) is one potential option that they’ll look into in the future.

An evaluation matrix was created which declared Protocol Buffers the winner. Protocol Buffers won because it allows data to be split across different nodes; it is reusable for data other than just tweets (logs, file storage, RPC, etc); it parses efficiently; fields can be added, changed, and deleted without having to change deployed code; the encoding is small; it supports hierarchical relationships. All the other options failed one or more of these criteria.

## IDL Used for Codegen

Surprisingly efficiency, flexibility and other sacred geek metrics were not the only reason Twitter liked Protocol Buffers. What is often considered a weakness, Protocol Buffer’s use of an IDL to describe data structures, is actually considered a big win by Twitter. Having to define data structure IDL is often seen as a useless waste of time. But from the IDL they generate, as part of the build process, all Hadoop related code: Protocol Buffer InoutFOrmats, OutputFormats, Writables, Pig LoadFuncs, Pig StoreFuncs, and more.

All the code that once was written by hand for each new data structure is now simply auto generated from the IDL. This saves ton of effort and the code is much less buggy. IDL actually saves time.

At one point model driven auto generation was a common tactic on many projects. Then fashion moved to hand generating everything. Codegen it seems wasn't agile enough. Once you hand generate everything you start really worrying about the verbosity of your language, which moved everyone to more dynamic languages, and ironically DSLs were still often listed as an advantage of languages like Ruby. Another consequence of hand coding was the framework of the weekitis. Frameworks help blunt the damage caused by thinking everything must be written from scratch.

It’s good to see code generation coming into fashion again. There’s a lot of power in using a declarative specification and then writing highly efficient, system specific code generators. Data structures are the low hanging fruit, but it’s also possible to automate larger more complex processes.

Overall it was a very interesting and useful talk. I like seeing the careful evaluation of different options based on knowing what you want and why.  It's refreshing to see how these smart choices can synergize and make a better and more stable system.

## Related Articles

1.  [Hadoop and Protocol Buffers at Twitter](http://www.slideshare.net/kevinweil/protocol-buffers-and-hadoop-at-twitter)
2.  [A Peek Under Twitter's Hood](http://borasky-research.net/2010/02/17/a-peek-under-twitters-hood) - Twitter’s open source page goes live.
3.  [Hadoop](http://hadoop.apache.org/)
4.  [ProtocolBuffers](http://code.google.com/p/protobuf/)
5.  [Hadoop Bay Area User Group - Feb 17th at Yahoo! - RECAP](http://developer.yahoo.net/blogs/hadoop/2010/02/hadoop_bay_area_user_group_feb.html)
6.  [Twitter says](http://blog.twitter.com/2010/02/measuring-tweets.html) "Today, we are seeing 50 million tweets per day—that's an average of 600 tweets per second."

</div>