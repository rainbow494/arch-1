## [Stuff The Internet Says On Scalability For February 19th, 2016](/blog/2016/2/19/stuff-the-internet-says-on-scalability-for-february-19th-201.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Friday, February 19, 2016 at 8:56AM</span></div>

<div class="body">

<div align="center">![](https://c2.staticflickr.com/2/1632/25054765165_d80d13e3dd_o.jpg)  
JPL is firing up their [Exoplanet Travel Bureau](http://www.jpl.nasa.gov/news/news.php?feature=5052). Reserve your space now.</div>

*   [200K](https://daringfireball.net/thetalkshow/2016/02/12/ep-146): msgs send per second through iMessage; [750 million](https://daringfireball.net/thetalkshow/2016/02/12/ep-146): xactions per week in App and iTunes store; [11 million](https://daringfireball.net/thetalkshow/2016/02/12/ep-146): Apple Music subscribers; [.7c](https://vimeo.com/155635184): speed of light in silicon; [1.125Tpbs](http://gizmodo.com/researchers-achieve-fastest-ever-data-transmission-at-b-1758452054): fastest ever data transmission; [360TB](http://www.extremetech.com/extreme/223144-researchers-develop-superman-memory-crystal-that-could-store-360tb-of-data): Superman memory crystal stores data forever;

*   Quotable Quotes:
    *   [Joseph Bradley](https://www.cryptocoinsnews.com/scaling-decentralized-blockchains/): "Here is the takeaway. Blockchains must be massively more scalable than the current tech that supports Bitcoin. We start scaling slowly or quickly. And if we choose the latter, it will “require fundamental protocol redesign."
    *   [@sigfpe](https://twitter.com/sigfpe/status/689929982991175680): Nobody knows how to "program" DNA. They just copy-and-paste bits from other organisms. A bit like how most code is built from stackoverflow.
    *   [@evankirstel](https://twitter.com/evankirstel/status/698335827374505984): Slack now has 2.3 million daily active users, 675,000 paid seats, and 280 apps in its directory
    *   [Jonas Luster](https://www.quora.com/Shouldnt-writers-on-Quora-Facebook-TripAdvisor-get-a-share-of-the-sites-profits-or-funding-since-they-are-essential-for-the-sites-popularity/answer/Jonas-Mikka-Luster): Money spoiled blogging. Why? Because people moved from doing great things for money and then talking about them on their free blogs, to people doing nothing but talking on their monetized blogs. 
    *   [Greg Ferro](http://etherealmind.com/the-data-centre-network-of-networks/): In my view, the most common architectural flaw made by network engineers is that the data centre has a single network. I believe that the correct perspective is that any “network” is a “network of networks”.
    *   [@antirez](https://twitter.com/antirez/status/700252254104846336): @Nick_Craver I’ve always this nice feeling that you manage to run a top-traffic site with 1/100 of the hardware. It’s like a reality check…
    *   [@levie](https://twitter.com/levie/status/698353313452929025): With Apple, Amazon, and Netflix now producing their own shows, Box will be accepting script submissions for enterprise software dramas.
    *   [Fred George](http://www.infoq.com/news/2016/02/not-just-microservices): We had 50 IT professionals, 25+ titles and zero people understanding the project they were working on...
    *   [@jasongorman](https://twitter.com/jasongorman/status/698417616390647809): "They asked for a bridge, but I know what they really needed was (another) reusable civil engineering framework"
    *   [@aplokhotnyuk](https://twitter.com/aplokhotnyuk/status/700347753776463873): @cbrisket @giltene @netty_project @eBay Neutrino is highly available... We have measured upwards of 300+ requests per second on a 2-core VM.
    *   [jsmthrowaway](https://news.ycombinator.com/item?id=11003740): You don't even need unikernels, and as much as I loathe myself for saying it, I find myself agreeing with a few of Cantrill's points regarding unikernels in prod. Not all of them, and I think it's worth exploring, but there's a spectrum here: on one end, unikernel app containers, and on the other full jails. The Google approach with minimal containers that still act Unixy and Posixy but carry very little distribution overhead is somewhere around 0.1 on the spectrum.
    *   [@Beaker](https://twitter.com/beaker/status/699696035350900737): This is why I call these "Internet-scale monoculture vulnerabilities." FFS. 

*   We never considered the possibility Skynet may just be stupid. [The NSA's SKYNET program may be killing thousands of innocent people](https://www.listbox.com/member/archive/247/2016/02/sort/time_rev/page/1/entry/5:71/20160216134233:086C810C-D4DD-11E5-A3F7-0684EF10038B/): At root, this is a story about the problems that occur in the absence [of] adversarial peer review. NSA and GCHQ cut corners in their machine-learning approach, and no one called them on it, and they deployed it, and it kills people. But is also a microcosm of the spy services' culture of secrecy and the way that the lack of peer review turns into missteps.

*   A [buffer overflow exploit in glibc](http://arstechnica.com/security/2016/02/extremely-severe-bug-leaves-dizzying-number-of-apps-and-devices-vulnerable/) remained undetected for 8 years. How Bizaar. Also, [Linux kernel bug delivers corrupt TCP/IP data to Mesos, Kubernetes, Docker containers](https://medium.com/vijay-pandurangan/linux-kernel-bug-delivers-corrupt-tcp-ip-data-to-mesos-kubernetes-docker-containers-4986f88f7a19#.9kesqrtf5).

*   [How Uber Engineering Evaluated JSON Encoding and Compression Algorithms to Put the Squeeze on Trip Data](https://eng.uber.com/trip-data-squeeze). They tested a whole bunch of different compression approaches. A whole bunch. Their goal was to find a solution that both yielded a small size and a short time to encode and decode. The conclusion: "MessagePack with zlib. We felt this was the best choice for our Python-based, sharded datastore with no strict schema enforcement (Schemaless). We only discovered this combination because we took a disciplined approach to test a wide range of protocols and algorithm combinations on real data and production hardware. First lesson learned: when in doubt, invest in benchmarking." Result: A 1 TB disk will now last almost a year (347 days), compared to a month (30 days) without compression. We now have enough space to last over 30 years compared to just under 1 year, thanks to putting the squeeze on the data.

*   That feeling when you try to show your Grandma how to use the TV remote. [When you're house sitting for millennials and ask how the lights work](https://twitter.com/c8ters/status/699701086656077825). This is funny and a little sad, not much has changed in 30 years. 

*   If you are IBM and your insatiable demon child needs feeding, what do you do? You buy companies for their data. [Why IBM Just Bought Billions of Medical Images for Watson to Look At](https://www.technologyreview.com/s/540141/why-ibm-just-bought-billions-of-medical-images-for-watson-to-look-at/): Merge’s data set contains some 30 billion images, which is crucial to IBM because its plans for Watson rely on a technology, called deep learning, that trains a computer by feeding it large amounts of data.

*   Only two weeks notice? To be a developer is to continually have your heart broken. [Kimono Labs Acquisition Leaves Devs Hanging](http://www.programmableweb.com/news/kimono-labs-acquisition-leaves-devs-hanging/2016/02/17).

**Don't miss all that the Internet has to say on Scalability, click below and become eventually consistent with all scalability knowledge** (which means this post has many more items to read so please keep on reading)...

[Click to read more ...](/blog/2016/2/19/stuff-the-internet-says-on-scalability-for-february-19th-201.html)

</div>