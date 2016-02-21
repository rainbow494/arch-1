## [How do you explain cloud computing to your grandma?](/blog/2008/5/25/how-do-you-explain-cloud-computing-to-your-grandma.html)

    

    _Update 2: Nice introductory New York Time's article [Cloud Computing: So You Don’t Have to Stand Still](http://www.nytimes.com/2008/05/25/technology/25proto.html). Good example of how Animoto used RightScale and Amazon to meet a Facebook driven demand of 25,000 test drives an hour._  
_Update: Peter Laird in [Understanding the Cloud Computing/SaaS/PaaS markets: a Map of the Players in the Industry](http://dev2dev.bea.com/blog/plaird/archive/2008/05/understanding_t.html) paints a very cool visual map of all the cloud service players. It's a larger industry than you might think._  

Once upon a time I worked at an Asynchronous Transfer Mode (ATM) switch startup. Over a delicious Christmas punch my grandma asked me what I did for a living that I could afford such extravagantly inexpensive gifts. Always so subtle. I explained I worked on an ATM switch. Mistake. She sniffed, said that's nice, and asked me why the Automated Teller Machine ate her bank card that morning. No matter how hard I tried I couldn't convince her I didn't work on bank ATMs. To all future job interrogations I waxed off, protesting I do boring software stuff that nobody cares about.  

Not put off in the least, grandma asked me last night to explain this cloud computing thing she keeps hearing about at her church club. Afraid of being another victim of the distortion field surrounding cloud computing, I instead referred her to Kent Langley's excellent overview of the subject in [Cloud Computing: Get Your Head in the Clouds](http://www.productionscale.com/home/2008/4/24/cloud-computing-get-your-head-in-the-clouds.html). It does a good job demystifying the very confusing concept of cloud computing. It has nice diagrams, definitions, examples and is a great place to start.  

She agreed that she had learned a lot, but one thing still troubled her: what's the difference between cloud computing and utility computing? They seem to be the same to her. Always so perceptive. She felt sure if she could drive this point home she would score big points with her church group. Oh the pressure.  

I steadied myself and explained [3Tera’s](http://3tera.com/) take is that cloud computing is for service users and utility computing is for service builders. Cloud computing is essentially about the surrender of control. Users of a service like Salesforce.com don’t care how the site is implemented. They don’t care about how it scales, deals with failure, or any of the other 1000s of little details you have to care about when running a complicated operation. Users just want their service to work when they need it. Utility computing customers on the other hand require fine control over their resources because they are the builders of services like Salesforce.com. Cloud computing is built on utility computing. You couldn’t build a Salesforce.com on Google whereas you could build it on top of 3Tera or Amazon.  

[StorageMojo](http://storagemojo.com/2008/01/27/cloud-computing-is-foggy-thinking/) thinks all this cloud/utility nonsense is just foggy thinking. Real computing will stay local because the cost of network access is too high. Memory and CPU are plentiful and cheap while bandwidth is neither. Distributed computing 1990s style will still rule the day.  

Mike Nygard thinks there’s [A Cloud for Everyone](http://www.michaelnygard.com/blog/2008/02/a_cloud_for_everyone_1.html) in the future. Latency matters and “Keeping your endpoints on your own network at least lets you control your own latency.” Security matters and pushing your precious data into the hands of strangers isn’t secure. Yet we see SalesForce, Google Docs, Basecamp, SugarCRM, and hosted email all flourishing so is privacy really a concern for newer generations trying to get stuff done?  

[HP’s Patrick Eitenbichler](http://www.processor.com/editorial/article.asp?article=articles/P3015/31p15/31p15.asp) thinks “utility computing refers to a business model, while cloud computing describes the underlying IT architecture” with the real decision point being “utility/cloud computing vs. purchasing your own IT assets.”  

[Geva Perry writing for GigaOM](http://gigaom.com/2008/02/28/how-cloud-utility-computing-are-different/) essentially agrees with Mr. Eitenbichler saying: _Utility computing relates to the business model in which application infrastructure resources — hardware and/or software — are delivered. While cloud computing relates to the way we design, build, deploy and run applications that operate in a virtualized environment, sharing resources and boasting the ability to dynamically grow, shrink and self-heal._  

[Krish](http://gigaom.com/2008/02/28/how-cloud-utility-computing-are-different/#comment-863577) tries to condense that down to: _cloud computing is software as a service (where companies run their own software) and utility computing is hardware as a service (where you can run your own software)._  

[Margaret Rouse]( http://itknowledgeexchange.techtarget.com/overheard/overheard-what-the-heck-is-computing-in-a-cloud/) makes a good case for cloud computing being just a better marketing concept for utility/grid/cluster/distributed/parallel computing.  

[Bits or Pieces](http://blog.gardeviance.org/2008/02/market-forces-part-iii-saas.html) smartly ignores saying the word cloud but my impression is they think providing Software as a Service on a utility computing basis is the game changing innovation.  

James Urquhart [defines the cloud](http://blog.jamesurquhart.com/2008/02/engage-cautiously.html) to include: _SaaS, PaaS (e.g. force.com) and HaaS (e.g. Amazon, Mosso, etc.). SaaS is in clearly in play today, HaaS is being experimented with, but PaaS may be the most interesting facet of the cloud in the long term._  

[Keystones and Rivets](http://www.keystonesandrivets.com/kar/2008/02/cloud-computing.html) finds that “The Cloud” is grid computing wrapped up in a service offered by data centers.  

Confident I must have answered her original question, I asked “Now, doesn’t that clear things up grandma?”  

Grandma sniffed, said that's all very nice, but she still wanted to know why the ATM ate her bank card! I groaned and said “Goodnight grandma. I’ll call again next week.” “Excellent,“ she Cheshire smiled, “next week my church group is going to tackle if social networks are really monitizeable.”  
    