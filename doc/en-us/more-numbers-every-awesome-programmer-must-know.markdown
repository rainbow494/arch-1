## [More Numbers Every Awesome Programmer Must Know](/blog/2013/1/15/more-numbers-every-awesome-programmer-must-know.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, January 15, 2013 at 9:30AM</span></div>

<div class="body">

![](http://farm9.staticflickr.com/8476/8380608425_574817758a_o.jpg)

[<span>Colin Scott</span>](http://www.eecs.berkeley.edu/~rcs/index.html)<span>, a Berkeley researcher, updated Jeff Dean’s famous</span> [<span>Numbers Everyone Should Know</span>](http://highscalability.com/numbers-everyone-should-know) <span>with his</span> [<span>Latency Numbers Every Programmer Should Know</span>](http://www.eecs.berkeley.edu/~rcs/research/interactive_latency.html) <span>interactive graphic. The interactive aspect is cool because it has a slider that let’s you see numbers back from as early as 1990 to the far far future of 2020. </span>

Colin explained his [motivation for updating the numbers](http://colin-scott.github.com/blog/2012/12/24/latency-trends/):

> The other day, a friend mentioned a latency number to me, and I realized that it was an order of magnitude smaller than what I had memorized from Jeff’s talk. The problem, of course, is that hardware performance increases exponentially! After some digging, I actually found that the numbers Jeff quotes are over a decade old

<span>Since numbers without interpretation are simply data, take a look at</span> <span>[Google Pro Tip: Use Back-Of-The-Envelope-Calculations To Choose The Best Design](http://highscalability.com/blog/2011/1/26/google-pro-tip-use-back-of-the-envelope-calculations-to-choo.html). The idea is back-of-the-envelope calculations are estimates you create using a combination of thought experiments and common performance numbers to a get a good feel for which designs will meet your requirements.</span>  

<span>And given most of these measures are in nanoseconds, to better understand the nanosecond you can do no better than</span> <span>[Grace Hopper To Programmers: Mind Your Nanoseconds!](http://highscalability.com/blog/2012/3/1/grace-hopper-to-programmers-mind-your-nanoseconds.html) 11.8 inches is the length of wire that light travels in a nanosecond, a billionth of a second.</span>  

<span>Colin's post inspired some great threads</span> [<span>On Reddit</span>](http://www.reddit.com/r/programming/comments/15fgxj/latency_numbers_every_programmer_should_know_by/) <span>and</span> [<span>On Hacker News</span>](http://news.ycombinator.com/item?id=4966363)<span>. Here are some I found particularly juicy:</span>  

<span>To the idea that these numbers are inaccurate</span> [<span>Beckneard</span>](http://www.reddit.com/r/programming/comments/15fgxj/latency_numbers_every_programmer_should_know_by/c7m3e7y) <span>counters:</span>

> <span>It's for putting things into perspective, no matter how much these vary you can be pretty sure that L1 cache latency is about 2 orders of magnitude faster than the memory latency which again is a few orders of magnitude faster than the SSD latency which is again much faster than an ordinary hard drive, and that IS really fucking important to know if you want to be a good programmer.</span>

[<span>dmazzoni</span>](http://www.reddit.com/r/programming/comments/15fgxj/latency_numbers_every_programmer_should_know_by/c7mp7gh)<span>:</span>

> <span>Yeah, but the latency numbers help a lot in interpreting profiling results. If you don't know that there's a such thing as an L2 cache, you won't understand why your program suddenly gets much slower when your pixel buffer exceeds 512 x 512 pixels.</span>

<span>[jeffffff](http://news.ycombinator.com/item?id=4967907):</span>

> <span>What does this mean in practice? that to do anything practical with these latency numbers, you also need to know the parallelism of your processor and the parallelizability of your access patterns. the pure latency number only matters if you are limited to one access at a time.</span>

[<span>RagingOrangutan</span>](http://www.reddit.com/r/programming/comments/15fgxj/latency_numbers_every_programmer_should_know_by/c7m7gsb)<span>:</span>

> <span>I find this to be pretty misleading for different years because it is extrapolating (or interpolating) those data. In reality technology does not improve smoothly like that; we tend to have modest growth punctuated by big advancements. </span>Particularly misleading is that it has numbers for SSDs in 1991.

[<span>djimbob</span>](http://www.reddit.com/r/programming/comments/15fgxj/latency_numbers_every_programmer_should_know_by/c7mdlbh)<span>:</span>

> <span>Most of the time, the choices to a programmer are clear ... minimize branch mis-predictions (e.g., make branching predictable; or make it branch free if possible); prefer L1 cache to L2, (to L3) to main memory to SSD to HDD to pulled from faraway network; prefer sequential reads to random reads; especially from an HDD. And know that for most of these there's order's of magnitude improvements.</span>
> 
> <span>Knowing the specifics is only helpful if you need to make a decision between two different types of method; (do I recalculate this with stuff in cache/memory) or look up a prior calculation from disk or pull from another computer in the data center?</span>
> 
> <span>The other stuff is more useful as trivia for why something happens after you profile; e.g., why does this array</span>

[<span>patrickwiseman</span>](http://news.ycombinator.com/item?id=4967525)<span>:</span>

> <span>With timings that are several orders of magnitude in difference I'd just ignore the constants between factors as they change too frequently. Also there is a difference between latency and bandwidth and the chart is simply inconsistent.</span>
> 
> <span>CPU Cycle ~ 1 time unit Anything you do at all. The cost of doing business.</span>
> 
> <span>CPU Cache Hit ~ 10 time units Something that was located close to something else that was just accessed by either time or location.</span>
> 
> <span>Memory Access ~ 100 time units Something that most likely has been accessed recently, but not immediately previously in the code.</span>
> 
> <span>Disk Access ~ 1,000,000 time units It's been paged out to disk because it's accessed too infrequently or is too big to fit in memory.</span>
> 
> <span>Network Access ~ 100,000,000 time units It's not even here. Damn. Go grab it from that long series of tubes. Roughly about the same amount of time it takes to blink your eye.</span>

<span>[HoLyVieR](http://news.ycombinator.com/item?id=4966964):</span>

> <span>The idea is more about knowing the scale of time it takes to help compare different solution. With those number you can see that it's faster to get data in memory from a distant server than to read the same data from disk. In common application that means using disk storage is less efficient that storing it in a database on an other server, since it usually keeps almost everything in memory. It also tells a lot about why CDN are needed if you are serving client worldwide. A ping from North America to Europe will always takes 100+ ms, so having geographically distributed content is a good idea.</span>

[<span>Tuna-Fish2</span>](http://www.reddit.com/r/programming/comments/15fgxj/latency_numbers_every_programmer_should_know_by/c7mac9d)<span>:</span>

> <span>Not really. Lately, memory speeds have improved by allowing more parallel requests, not by reducing single-request latency. This is important because it means that code that does pointer-chasing in a large memory pool becomes 2x worse against independent parallel accesses on every new type of memory. Trees are becoming a really bad way to manage data...</span>

[<span>TallboyOne</span>](http://news.ycombinator.com/item?id=4966824)<span>:</span>

> <span>It amazes me that something happening locally (in the ~10ms range, say a database query) is even in the same scale as something traveling halfway around the world. The internet wouldn't really work without this fact, but I'm very thankful for it. Simply incredible.</span>

[<span>cojoco</span>](http://www.reddit.com/r/programming/comments/15fgxj/latency_numbers_every_programmer_should_know_by/c7m6ufh)<span>:</span>

> <span>And as latency goes up, it makes more sense to optimise for cache use, and you can make some huge speed-ups by reordering memory accesses appropriately.</span>

[<span>gilgoomesh</span>](http://news.ycombinator.com/item?id=4967782)<span>:</span>

> <span>I think one important latency is left out here...</span>
> 
> <span>Latency to screen display: approximately 70ms (between 3 and 4 display frames on a typical LCD).</span>
> 
> <span>Obviously, you only have 1 display latency in your pipeline but it's still typically the biggest single latency.</span>

<span>To a question how to reduce the round trip time between California and the Netherlands </span><span>[AngryParsley](http://news.ycombinator.com/item?id=4966567) replies</span><span>:</span>

> <span>There's not much room for improvement. It's a little over 5000 miles from CA to the Netherlands, so light takes about 30ms to travel that far on a vacuum. That means a 60ms round trip minimum. Light in fiber goes at 0.6-0.7c, so the fastest round-trip you could have with a straight fiber line is 80ish milliseconds</span>

[<span>HHBones</span>](http://www.reddit.com/r/programming/comments/15fgxj/latency_numbers_every_programmer_should_know_by/c7m6q3q)<span>:</span>

> <span>I was surprised they weren't less. Because the number is so small (only 17 ns), we have to assume some things (note that I'm guessing what they're assuming):</span>  
> <span>the mutex is available (we clearly aren't blocking)</span>  
> <span>the mutex is in at least L2 cache (if it were in main memory, the latency would be far greater)</span>  
> <span>the mutex is implemented as a simple shared integer. 1 is locked, 0 is free.</span>  
> <span>According to Agner, a TEST involving cache takes 1 cycle (1/2.53 ns on my system), and a CMOVZ takes a variable amount of time (by their numbers, this should take roughly 12 cycles (1 to test the zflag, 11 to write to L2 cache). So, the entire test-and-lock process takes 13ticks/2.53 billion ticks per second = 5.14 ns; about a third of what they say.</span>  
> <span>Of course, if you take away the assumptions, the latency skyrockets. </span>

## <span>Related Articles</span>

*   [Erik Meijer: Latency, Native Relativity and Energy-Efficient Programming](http://channel9.msdn.com/Blogs/Charles/Erik-Meijer-Latency-Native-Relativity-and-Energy-Efficient-Programming)
*   [Optimization manuals](http://www.agner.org/optimize/#manuals)
*   [How Big Is A Petabyte, Exabyte, Zettabyte, Or A Yottabyte?](http://highscalability.com/blog/2012/9/11/how-big-is-a-petabyte-exabyte-zettabyte-or-a-yottabyte.html)

</div>