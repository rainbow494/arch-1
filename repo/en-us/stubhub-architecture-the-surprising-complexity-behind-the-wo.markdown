## [StubHub Architecture: The Surprising Complexity Behind the World’s Largest Ticket Marketplace](/blog/2012/6/25/stubhub-architecture-the-surprising-complexity-behind-the-wo.html)

    

    

![](http://farm6.staticflickr.com/5340/7437966078_c8f1376634_m.jpg)

StubHub is an interesting architecture to take a look at because, as market makers for tickets, they are in a different business than we normally get to consider.

StubHub is surprisingly large, growing at 20% a year, serving 800K complex pages per hour, selling 5 million tickets per year, and handling 2 million API calls per hour. 

And the ticket space is surprisingly rich in complexity. StubHub's traffic is tricky. It's bursty, centering around unpredictable game outcomes, events, schedules, and seasons. There’s a lot of money involved. There are a lot of different actors involved. There are a lot of complex business processes involved. And StubHub has several complementary but very different parts of their business: they have an ad server component serving ads to sites like ESPN, a rich interactive UI, and a real-time ticket market component.

Most interesting to me is how StubHub is bringing into the digital realm the once quintessentially high-touch physical world of tickets, point-of-sale systems, FedEx delivery, buyers and sellers, and money. They are making it happen with deep electronic integration into organizations (like Major League Baseball) and a Lifecycle Bus that moves complex business processes out of the application space.

It's an interesting problem made more complex by having to move forward while dealing with legacy systems built when getting business building features out the door was the priority. Let's see how StubHub makes it all work...

## Source

*   Charlie Fineman's presentation at QCon: [StubHub: Designing for Scale and Innovation for the World’s Largest Ticket Marketplace](http://www.infoq.com/presentations/StubHub-Designing-for-Scale-and-Innovation-for-the-World-s-Largest-Ticket-Marketplace)

##     Business Model    

*       **        StubHub     is an eBay for tickets    **            . Providing a marketplace for buyers and sellers of tickets. They are not like     Ticketmaster    .        
*       **An escrow model is used to provide trust and safety for  buyers a sellers**            . A credit card is on file at     StubHub    , when the order is confirmed as received only then is the money transferred.        
*       **Tickets are not commodities**        . Buyers want specific tickets, not a bleacher seat, for example. There are limited quantities, there is no backorder for a ticket. Sellers are constantly updating prices and quantity in reaction to market forces. It’s a very active market.    

##     Statistics    

*       5 Million orders per year    
*       2 Million active listings/tickets.    
*       Tickets revolve around 45k events. Major League Baseball post season and the Super Bowl are the most active periods.     
*       Sales growing at 20% per year.    
*       Serves 600k - 800k complex pages per hour. Burst to a million per hour during post season.    
*           Traffic is very     bursty     in a narrow window of the 10-12 core waking hours in the US. When a playoff game finishes, for example, there’s a buyer frenzy for tickets for the next game.        
*           2 million     API     calls from affiliates per hour.        
*       24-36 engineers.    
*       It’s a high touch business. Support call to transaction was 1-1 in the past, but is a lot better now. Biggest part of staff is customer service and back office business operations support.    

##     Platform    

*       Java    
*       Cold Fusion (legacy)    
*               ActiveMQ            
*   [        SEDA        ](http://www.eecs.harvard.edu/~mdw/proj/seda/) (Staged Event-Driven Architecture)
*               Lucene    /    Solr            
*               Jboss            
*               Memcache            
*               Infiniband     - fast network to SAN        
*               XSL            
*       Advanced Queuing from Oracle    
*               TeamWorks     - An IBM workflow builder        
*               Splunk            
*           Apache     HttpClient            
*       Log4j (using message format)    
*       Tapestry    

##     Architecture    

*       Three sources of ticket purchases: Web, Point of Sale Systems, Bulk Upload. Bulk upload allows sheets of tickets to be uploaded into the system.    
*       A Manager layer provides a business object abstraction above the Ticket database. It mediates all conversations with the tickets table.    
*           The Ticket table gets hammered with the activity of buyers and sellers and the natural traffic     burstiness     around events.        
*       An active market can cause the mobile clients to become out of date with the current state of the system so buyers are reacting to old data.    
*           Two data pumps feed data from the Tickets database to internal and external systems: My Account, Find, and a Public Feed. My Account is an interface to a user’s account. Find is a search capability. The Public Feeds powers sites like     ESPN     and eBay.        
    *   Internal Feed - Contains sensitive information, like account information, that is used for dashboards, who sellers are, what sales are happening, sales velocities, heat maps, etc.  It also feeds parts of the home page that are based on sensitive market data, like what's hot, that they don't want to share with the public.
    *           External Feed (LCS) - Advertisers, like     ESPN    , are fed through this feed. Ads are     geo     mapped by     IP     address.         
*               LCS     (Listing Catalog Service):         
    *   My apologies here, the slides were a little off and it was difficult to  match the speaker with the presentation. All errors are mine. 
    *           Triggers are used to keep a modification table up to do date with changes as they occur in the database.         
    *           A Change Data Capture job constantly polls for changes and injects message into an     ActiveMQ     broker. This is the first level of     routing and contains     sma    ll     payloads: object ID, object type, and change date.        
    *           Change data is routed to the Master which is the fundamental mechanism for replicating between datacenters. A secondary     datacenter     subscribes to these topics and that's how data is replicated between     datacenters    .          
    *   Once in the Master data is injected into a SEDA queue for processing (more on that later).
    *       The Manager is not used because many legacy systems exist that don’t use the Manager and go directly to the database, so the database is the common point to distribute deltas.    
    *       Most of their ads are flash, but some use HTML rendering.    
    *       Shopping experiences like selecting tickets from an interactive graphic of the stadium are powered by LCS. Solr makes adding new features like this pretty easy.    
*           SEDA     (Staged Event-Driven Architecture) use in the Master    
    *   SEDA is a technique for smoothing out load. It has been effective for them from a resource management perspective. The idea is to break work down into small enough pieces that slow pieces of work won't steal threads from other users. Work is modeled in stages and each stage has its own thread pool, which acts as a throttle on work.
    *       The Master receives the sma    ll     updates and figures out how to build the thing that goes into     memcache     for eventual     routing     to     Solr    .    
    *   Messages are cached using a protocol buffer format in memcached.
    *   Messages are sent to a second broker which disperses messages out to the edge, which is Lucene/Solr. 
    *   A message consumer receives messages from a broker
        *   Loads the entity from the database
        *       Determine if there are any cascading impacts from the update. Because     Solr     and other     NoSQL     databases don't do joins, if a band name is changed, for example, that change must be propagated to a    ll     events.     
        *       Serialize entity. Stores it in     memcache    .     
        *       Sends a message to a second     ActiveMQ     broker which routes messages to the edges, which is Solr.     
        *       The broker is listened in on by a seperate process that does the routing. This was once in Jboss, but Jboss would get overwhelmed and they ran into starvation issues so they moved it out of Jboss. This listener became a useful valve in the system for operational management. If they needed to swap out a new Solr index to put in a new schema, they could turn the valve off, let messages back up in the message broker, and open up the valve again an messages would start flowing again. The valves had a huge impact on their operational stability when recovering from Solr failures, copying indexes around, and updating schemas.    
    *       A    ll     these are blocking operations so using the thread pools prevents the storming of database connections.    
*       Solr    
    *   Solr provides a nice document store and nature language text query feature.
    *   All search is built on Solr, including faceting. 
    *       FAST. Queries returned in 10    msec     or less. They use an     Infiniband     network to their SAN and found they didn't need to cache data in memory, they could it serve it off their SAN fast enough with the fast network.    
    *       POTENT. Flexible query language, fu    ll     text search, and     geo    -special searching.     
    *       Supports many output formats: XML, Atom,     RSS    , CSV,     Json    . Makes it easier to integrate with various clients.    
    *   Not so great with high frequency writes. Replication doesn't seem well integrated under high frequency writes. Doing an rsync when you are seeing 10s of thousands of changes doesn't work. You get really stale data. So they had to build out their own replication mechanism. 
    *   Flat data structures. Still pretty much row oriented. They would like support for documents with more complex structures. 
*           DCL     - Double Click intro Browse    
    *           URLs     are mapped onto     IDs    : genre ID,     geo     ID, render type ID.     
    *       Genre ID and     Geo     ID are used by     DCL    , which is just     XSL    , to create queries for     LCS    . The returned data can then be generically rendered. So a    ll     footba    ll     teams can have a similar page generated for them with the exact same structure using URL mapping,     XSL    , and     LCS    .     
    *       Huge productivity gain, it makes it much easier to add new features. Each block in a page is an individual asset that is managed via their     CMS    . They are chunks of     XSL     that get rendered against a context document which is retrieved from     LCS    .       
    *       A     RenderChunkByName     ca    ll     makes it easy to render events on other services like     Facebook    .     
    *       Doing all this on the backend for SEO purposes.  When search engines can index Ajax they may not need to do this.    
*       Edge caching of     gifs    , style sheets, etc, but  data is cached on their servers.    
*   Reducing the number of customer interactions per transaction:
    *   Customer interaction is the biggest drain on their operating income. When things go bad it takes a lot of working with buyers and sellers to straighten problems out. 
    *       Increasing customer self service. Customers want to know when they get their money and tickets. The     MyAccount     screens allow customers to check on the progress of their order without using customer service.     
    *       Expose     APIs     to sellers so they can integrate these features into their systems.     
    *           IVR     - integrated voice recognition systems support customers calling up to find the state of their account.     
    *   Cash flow is important to vendors so they work on getting payments out quicker. 
    *   Electronic integration with Major League Baseball so they can transfer a ticket directly to buyer from a seller before the seller has physical possession of the tickets. Instant delivery and huge improvement in customer satisfaction and removal of failure points.
*   Lifecycle Bus
    *       Used to prevent hardwiring complex workflow in applications. A check-out application doesn't want to have to worry about a    ll     the different business processes living downstream. You just want to worry about validated credit cards and ordering, not things like fulfillment and email.     
    *   Useful in dealing with legacy issues and managing changes to the site. 
    *       There are topics for a    ll     the major lifecycle events. Agents listen on Oracle's queue for topics. When an order is cut it goes into an unconfirmed state. A listener listens on the "unconfirmed" topic and emails the seller to come to the site and confirm the order. When the seller confirms the order there agents that wi    ll     capture the money from the buyer's escrow account, email the buyer saying the seller has confirmed and where to find the tickets. When the tickets are confirmed payment is released to the seller.     
    *   All this logic is decoupled from the end user facing part of the web site. These are all backoffice engines. 
    *           TeamWorks     models these processes, finds weakness, monitors the processes, check     SLAs    , and triggers actions. Helps better optimize     backoffice     business processes. As they grow 20% per year they don't want to grow the operations team 20% year over year.     
    *           FedEx     was the original mode of fulfillment. Electronic Fulfillment was added. The business process looks like: unconfirmed -> auto confirm; confirmed -> barcode reissue and Disburse PDF; fulfilled. A    ll     you have to do is write agents that live off the same order lifecycle to implement new fulfillment modes. This logic is not in the application. It's in the agent and is an individually deployable and testable unit.    
    *   Fraud avoidance. Uses the same lifecycle model for fulfillment, but adds two new states: purchased and approved. They didn't have to change anything to add fraud avoidance. They just had to change the state machine. The agent decided to move it to an approved state or not approved state. 
*   Point-of-Sale System Integration
    *       Uses a two phase commit: reserve a ticket on the external system, mark it as claimed in     StubHub    , commit purchase on external system.     
    *   Looking to generalize this so other systems can buy tickets as part of their transactions. Bundle a ticket with a trip or hotel purchase. 
*           Splunk     and Dye    
    *       One of the biggest     ROI     projects at     StubHub    . Saves so much time debugging problems.     
    *   Dye - an artifact is placed into the HTTP header of each request. 
    *   These are logged using Log4j. 
    *       Using     Splunk    , if there's a problem with an order, you can look at the log lines using Dye markers to pu    ll     back and see a    ll     the calls that were part of the request, including secondary calls to other services like     LCS    . It's easy to retrace activity.    
    *       Really like     Splunk    . It's like a document store for log lines. Put dye marker and order     IDs     into log lines, like a series of key-value pairs, and     Splunk     makes it really easy to look at logs. Their dashboards are written using     Splunk    , for     stats     like transactions per minute, failures per minute. You can slice and dice the data anyway you want.     
    *   Use Log4j with message format so it won't do dynamic string creations. 

## Lessons Learned

*       **Scalability is specialization**            . Every problem space has its unique characteristics and any system must be built to solve that particular problem.     StubHub     is constrained by the need for a safe buying experience, the unique nature of the ticket market,     bursty     traffic, and the vagaries of events. Their system must reflect those requirements.        
*       **Use an abstraction layer from the start**        . Otherwise you’ll be stuck supporting legacy clients far past your tolerance point.    
*       **Bake-off in production**            . Implement multiple solutions and have a bake-off in production to determine which version works better.     StubHub     tested two different data pump versions in production to see which was a better fit. You don’t want to support multiple infrastructures.        
*       **    Move work out of     Jboss        **            . Huge numbers of requests could cause     Jboss     to starve, so they moved work outside of     Jboss    .        
*   **Percolate dye through the causal chain**    . Instrument requests so that a request can be traced through an entire stack. It's a huge     ROI     to be able to debug problems across the stack.     
*   **Optimize business processes.**     Electronic integration between systems.  By acting as the coordinator between sellers, buyers, and     MLB    , they were able to increase customer satisfaction and remove a large number of possible failure points in the transaction. The bought out a popular point-of-resale program so they could integrate with it.    
*   **    Build on your own     APIs        **    . They are spending a lot of time trying to build their own application on their own     APIs so they can better govern the experience for their users and partners.        
*   **Define assets generically**    . Defining assets such that they could be rendered on any context made it easy to compose pages in different formats and render events on other     websites    .    
*   **    Maximize development from an     ROI     perspective    **    . Look for projects that improve developer     ROI    . Using     Solr     was a huge win for them. It's easy to use, fast, and very functional, many types of queries could be satisfied off the shelf.    
*   **        SEDA     is good for blocking reads    **    . A lot of their system is based on blocking reads, so     SEDA     is a good fit for that use case. Threads pools prevent overwhelming the resources they are trying to use.    
*   **Render on the client**    . For really cool interactive maps like stadium maps, handling a    ll     the     UI     interaction on the client wi    ll     save a lot of server load. Even for larger events with 10-20K listings, it was a better solution to download the whole listing.     
*   **Prefer thin over heavy frameworks**. Heavy frameworks are easy to use and abuse. Hiding complexity makes you lose control over your site really fast. Examples: Hibernate and Component-based frameworks. Validation and business logic can leak into the presentation layer. Make a conscious decision. Understand what you are biting off in terms of a legacy issues. 
*   **Bad experiences are the best training**    . There's nothing like failing to teach how to do things right. The guys that started     StubHub     were building a business by getting features out as fast as possible, but this left a legacy. The key to managing a legacy is managing the dependencies. Using agent based Lifecycle Bus style solution helps them understand the dependencies on legacy systems.     
*   **Decouple state machines from applications using workflow.**     Don't embed complex flows in application logic. Externalize logic so business processes can be couples together in more flexible ways. This makes the system infinitely more flexible and adaptable going forward.    
*   **    Avoid     ETL        **    . It introduces a lot of dependencies you would rather not deal with. It's a risk. A legacy data model can really suck away resource when you are trying to figure out if a change wi    ll     great they system you financially depend on.     
*   **Don't short-change CM and deployment**. Current biggest waste of time for developers now. It's very painful. Invest now in your CM and deployment system. 
*   **Invest in continuous improvement**    . It does not come for free. Run a post-    mortem     on projects. Make sure issues don't come up again. As your company grows this stuff does not scale. Make the right decisions now or it wi    ll     take 3x-5x more in the future to fix.    
*       **Build operational valves into the system**        . If, for example, you need to swap out a new schema, have a valve where you can turn off events and the restart them again.    

    