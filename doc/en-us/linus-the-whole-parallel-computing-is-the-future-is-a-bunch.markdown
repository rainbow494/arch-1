## [Linus: The whole "parallel computing is the future" is a bunch of crock.](/blog/2014/12/31/linus-the-whole-parallel-computing-is-the-future-is-a-bunch.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, December 31, 2014 at 9:09AM</span></div>

<div class="body">

[![](https://farm8.staticflickr.com/7506/15528960904_b8fcbe02dc_n.jpg)](http://falln-stock.deviantart.com/art/Boy-on-Train-Tracks-13580138)

<span>Linus Torvalds in his usual politically correct way made a typically understated statement about “pushing the whole parallelism snake-oil” that generated almost no response whatsoever.</span>

<span>Well, not quite. His comment on</span> [<span>Avoiding ping pong</span>](http://www.realworldtech.com/forum/?threadid=146066&curpostid=146227) <span>has generated hundreds of responses, both on the original post and</span> [<span>on Reddit</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/)<span>.</span>

<span>The</span> [<span>contention</span>](http://www.realworldtech.com/forum/?threadid=146066&curpostid=146227)<span>:</span>

> <span>The whole "let's parallelize" thing is a huge waste of everybody's time. There's this huge body of "knowledge" that parallel is somehow more efficient, and that whole huge body is pure and utter garbage. Big caches are efficient. Parallel stupid small cores without caches are horrible unless you have a very specific load that is hugely regular (ie graphics).</span>
> 
> <span>Nobody is ever going to go backwards from where we are today. Those complex OoO [</span>[<span>Out-of-order execution</span>](http://en.wikipedia.org/wiki/Out-of-order_execution)<span>] cores aren't going away. Scaling isn't going to continue forever, and people want mobility, so the crazies talking about scaling to hundreds of cores are just that - crazy. Why give them an ounce of credibility?</span>
> 
> <span>Where the hell do you envision that those magical parallel algorithms would be used?</span>
> 
> <span>The only place where parallelism matters is in graphics or on the server side, where we already largely have it. Pushing it anywhere else is just pointless.</span>
> 
> <span>So give up on parallelism already. It's not going to happen. End users are fine with roughly on the order of four cores, and you can't fit any more anyway without using too much energy to be practical in that space. And nobody sane would make the cores smaller and weaker in order to fit more of them - the only reason to make them smaller and weaker is because you want to go even further down in power use, so you'd still not have lots of those weak cores.</span>
> 
> <span>Give it up. The whole "parallel computing is the future" is a bunch of crock.</span>

An interesting question to ponder on the cusp of a new year. What will programs look like in the future? Very different than they look today? Or pretty much the same?

<span>From the variety of replies to Linus it's obvious we are in no danger of arriving at consensus. There was the usual discussion of the differences between distributed, parallel, concurrent, and multithreading, with each succeeding explanation more confusing than the next. The general gist being that how you describe a problem in code is not how it has to run.  Which is why I was not surprised to see a mini-language war erupt.</span> 

<span>The idea is parallelization is a problem only because of the old fashioned languages that are used. Use a better language and parallelization of the design can be separated from the runtime and it will all just magically work. There are echoes here of how datacenter architectures are now utilizing schedulers like Mesos to treat entire datacenters as a programmable fabric.</span> 

<span>One of the more interesting issues raised in the comments was a confusion over what exactly is a server? Can a desktop machine that needs to run fast parallel builds be considered a server? An unsatisfying definition of a not-server may simply be a device that can comfortably run applications that aren't highly parallelized.</span> 

<span>I pulled out some of the more representative comments from the threads for your enjoyment. The consensus? There is none, but it's quite an interesting discussion...</span>

[<span>spookylukey</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn97272)<span>:</span>

> <span>In other words: "4 cores should be enough for anybody"</span>

[<span>Ruudjah</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9aaje)<span>:</span>

> <span>Parrallelism ≠ Multithreading ≠ Distributed computing</span>
> 
> <span>just sayin</span>

<span></span>[<span>Linus Torvalds</span>](http://www.realworldtech.com/forum/?threadid=146066&curpostid=146238)<span>:</span>

> <span>I can imagine people actually using 60 cores in the server space, yes. I don't think we'll necessarily see it happen on a huge scale, though. It's probably more effective to make bigger caches and integrate more of the IO on the server side too.</span>
> 
> <span>On the client side, there are certainly still workstation loads etc that can use 16 cores, and I guess graphics professionals will be able to do their photoshop and video editing faster. But that's a pretty small market in the big picture. There's a reason why desktops are actually shrinking.</span>
> 
> So the bulk of the market is probably more in that "four cores and lots of integration, and make it cheap and low-power" market.
> 
> But hey, predicting is hard. Especially the future. We'll see. 

[<span>Patrick Chase</span>](http://www.realworldtech.com/forum/?threadid=146066&curpostid=146260)<span>:</span>

> <span>While you're right that the specific issue Gabriele raised can be mitigated by a different language choice, that isn't generally true. There have been no fundamental technological breakthroughs that make parallelism easier/cheaper to achieve than it was, say, a decade ago. No magical compilers, no breakthrough methodologies or languages, and therefore no substantive mitigation of Amdahl's law.</span>
> 
> <span>While there is certainly pressure from the semiconductor process side that limits the pace of improvement of serial performance, I don't see anything that would actually shift the equilibrium/optimum in favor of smaller cores.</span>
> 
> <span>I therefore think 'anon' had it about right: We'll continue to muddle along by improving per-core performance where we can and increasing parallelism where we must. It's not a terribly sexy vision, but sometimes reality works that way.</span> 

[<span>Maynard Handley</span>](http://www.realworldtech.com/forum/?threadid=146066&curpostid=146253)<span>:</span>

> <span>We have not even really started on this re-education of programmers so that they do things in a better way (where "better" means using abstractions that are a better match for parallel programming). Our languages, APIs, and tools are still in an abysmal state, like we're using Fortran and so trying to force recursion or pointers into the language is like pulling teeth. Our tools don't have the sort of refactoring that today makes us not care about needing to add a new parameter to a chain of function calls. etc etc.</span>

[<span>Patrick Chase</span>](http://www.realworldtech.com/forum/?threadid=146066&curpostid=146265)<span>:</span>

> <span>It all depends on volume and rate of change. When volumes are low and/or the algorithm set is unstable you use commodity HW, of which the flavor of the decade is the GPU. When the volumes get higher and the algorithms more stable it pays to do custom HW (the fixed costs of a moderately complex ASIC on a modern processes are O($10M) or higher when all is said and done, so you can do the math and figure out when it makes sense).</span>
> 
> <span>If the volumes are high but the algorithms are only partially stable, then it can make sense to do an ASIC that incorporates both fixed-function HW and programmability in the form of DSPs/GPUs/whatever. That's why Qualcomm sprinkles those 'Hexagon' doohickeys in all of their Snapdragons.</span>

[<span>jerf</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9oufx)<span>:</span>

> <span>It is possible that Linus is both right and wrong at the same time. In fact I'd observe that people have been expecting for a while that fundamentally new languages were going to be necessary to really take advantage of the parallel hardware, so it's not like this is a new thought. We don't just need languages that make it "possible, if you put a lot of work into it and fundamentally fight the language's default structure", we need languages that make it the "easy default case to parallelize where possible". We are, at best, just starting to walk through that door. We do not have these languages in hand. (And I say this despite some fairly extensive experience in the languages you'd consider today's best options. We're just starting.)</span>

[<span>666666666</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn99y3i)<span>:</span>

> <span>Considering he's talking about MASSIVE parallelism (i.e. hundreds of cores) in a general purpose CPU I'd tend to agree with this.</span>
> 
> <span>Massively parallel GPUs, and the like, he's good with.</span> 

[<span>Gabriele Svelto</span>](http://www.realworldtech.com/forum/?threadid=146066&curpostid=146236)<span>:</span>

> <span>In today's mobile world "fast enough" might not be the end of it all as far as optimization goes. Since most computing devices now run on batteries making something that's fast enough for the user perception faster is actually an important goal: it allows you to save power. In this respect certain parallel methods provide a bad trade-off: all else being equal they can improve the overall speed of execution at the cost of increased computation (and often communication) over an equivalent serial approach. Now in practice not everything outside of computation is equal so you've got fixed costs to amortize too and overall faster execution might be more efficient nevertheless; but this just to point out that there's more variables to take into account nowadays when deciding if a workload needs to be sped up or not.</span>

[<span>Crazy__Eddie</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9ke08)<span>:</span>

> <span>Parallelism is also still important on mobile, where a lot of stuff is going. The reason being is that getting the complete job done in a shorter time so you can go into power-save mode is nicer on the battery than partially using the CPU for longer. So you saturate it with parallel tasks to get everything done sooner.</span>

[<span>666666666</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9q378)<span>:</span>

> <span>But massive parallelism is not important on mobile. I don't think anyone is arguing against anything but massive parallelism (at least he mentioned "hundreds of processors" in his rant, IIRC. Believe me, I went and read it in the same mindset is you espouse in that I expected another mindless rant from Linus and was more than ready to slap it down. But when I got to that part I had to agree.</span>
> 
> <span>My argument isn't that most apps aren't parallel, my argument is that most apps don't NEED to be. There are certainly apps that do need to be.</span>
> 
> <span>I believe Linus is basically arguing against software development managers who gratuitously slap parallelism into their app, just because it becomes a feature that they can sell up the management chain.</span> 

[<span>rwessel</span>](http://www.realworldtech.com/forum/?threadid=146066&curpostid=146159)<span>:</span>

> <span>Despite all the hoopla, compilers mostly suck at optimization. About the only thing that does a worse job of optimization is the average programmer…</span>
> 
> <span>Higher level expression of programs is a good idea. Good enough that a ten fold reduction in performance is absolutely worth it in 99% of cases. And much of the computing world is built on far worse performance than that. Just consider the vast use of scripting languages in web servers.</span> 

[<span>darksurfer</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn98xvg)<span>:</span>

> <span>Servers of all sorts, games, artificial intelligence, image / video processing, analytics, simulation and modelling</span>
> 
> <span>I'm struggling to think of any application other than simple GUI's where parallel processing couldn't be helpful (if it was easy)</span>

[<span>mccoyn</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9dc1e)<span>:</span>

> <span>We have waited. We have seen. The parallel everything idea really took hold when clock speeds stopped increasing 10 years ago. Since then we have seen multiple cores with shared cache. It is tempting since the cache takes a lot of space and synchronizing dedicated caches without slowing them down is difficult. The problem is that each core then doesn't run the same speed as a single core would. It has to fight over the limited shared cache and suffers more cache misses and slows down.</span>
> 
> <span>Give me a modern single core processor with a huge cache. It will run circles around a similarly sized 4-core processor and won't require special programming techniques.</span>

[<span>dobkeratops</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn980zv)<span>:</span>

> <span>There has been a trend for increasingly versatile GPGPU. I think there was a case for a sliding scale between a single-thread optimised machine and the massively parallel GPU - sony tried to fill something in the middle with CELL (admittedly flawed in many ways), but increasingly general GPGPU capable GPUs and SIMD+multicore CPUs took that space from either end and did a better job. making use of 'gather' instructions for example is still an exercise in parallelism.</span>

[<span>ruinercollector</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9ioby)<span>:</span>

> <span>Depending on size of files and what you were doing with them, you likely would have gotten even better gains from using async IO instead of making a worker thread per core.</span>

[<span>introspeck</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9dafb)<span>:</span>

> <span>Linus speaks his mind in an unkind way, but I agree with much of what he says.</span>
> 
> <span>In my opinion, it's not that parallelism is necessarily bad; it's that programmers adopt it because they believe it will make their task run faster, without being aware of the overhead which comes with it - task switching, cache thrashing, lock contention, etc. And parallelism is harder to model mentally, so whole new categories of bugs arise. If your needs are well-suited to parallel implementations, it's worth doing.</span>

[<span>gbromios</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9kzqj)<span>:</span>

> <span>haha, It absolutely is covered. A large number of small, clearly independent tasks is the "trivial" (I use that word lightly) case where parallelization is obvious... I hope I'm not being too liberal with Linus's words when I interpret this sort of thing as what he meant by "server side". If he has 4 cores, he can get all four blasting on 1/4 of the workload without worrying about changing the workload itself, since the units of work are (presumably) mostly independent of each other.</span>
> 
> <span>The post does not say "parallel is bad", it says that you should respect the law of diminishing returns. My gut feeling is that we are at or near "peak parallel" for the time being, until some large innovation comes along that turns computing on its head. I couldn't begin to speculate what that would be, but I think it would have to be fundamental. Put another way:</span> <span>[http://xkcd.com/619/](http://xkcd.com/619/)</span>

[<span>Matt3k</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9ce4u)<span>:</span>

> <span>They're related. Threading is often a means of achieving parallelism.</span>
> 
> <span>Parallelism could be multiple discrete processes all working on the same task. Even if those processes are running on entirely different systems. But in general, parallelism is any situation where the work is broken up into multiple workflows that execute at the same time.</span>
> 
> <span>Threading is a technique where a single process has multiple paths of execution, and there's some shared memory and separate stacks and all that jazz. Threading normally implies concurrency, but it isn't a rule. For example, running a multi-threaded program on a CPU with a single core probably means you're not parallel.</span>
> 
> <span>In short:</span>
> 
> <span>Parallel = Multiple things happening at once for a common goal</span>
> 
> <span>Threaded = Often a specific implementation of parallelism, but not necessarily.</span>

[<span>dlyund</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9m5eg)<span>:</span>

> <span>Ummm. What? So if I want to do some "simulation or other scientific work" I should need to go out and buy/rent a server? Problem solved? No. This stuff might be better done on the server/cluster/cloud/whatever right now but the extent to which that's true only points to a weakness in the current "consumer" computers offerings.</span>
> 
> <span>If we redefine things so "consumer" computers are only for browsing the web, watching videos, and or playing some games then sure... but then appliances such as phone/tablet is probably good enough for that at this point so who really needs a "consumer" computer? It's also not the kind of computer use I'm interested in and I think there's a real market for computers that are good for... computing...</span>

[<span>bakuretsu</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9og18)<span>:</span>

> <span>I am no expert, but I think the point Linus is trying to make here is that parallelizing work at the CPU core level has already reached a point of diminishing returns when dealing with desktop software, which is an unpredictable heterogenous workload.</span>
> 
> <span>In specific applications, where the code is written for it, such as graphics, scientific research, compiling, etc., there are gains to be made, but probably not enough to push the industry as a whole toward more and more cores at the cost of heat, electricity, processor lifespan, etc.</span>
> 
> <span>You can already buy machines with multiple multi-core CPUs if you need it for your application, but those are edge cases and the hardware is still relatively expensive.</span>

[<span>quiI</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn96zac)<span>:</span>

> <span>The problem is parallelisation != threading, yet all the time people say things like "you shouldn't deal with parallelisation because you'll have to deal with locks, etc".</span>
> 
> <span>There are plenty of really good abstractions over parallelisation out there that makes it much easier to work with, such as actor systems, Future monads, whatever go's thing is called, etc.</span>
> 
> <span>To me it seems people talk about bad experiences with something when they haven't fully explored it. It's like when people say static typing is too restrictive when they only tried Java</span>

[<span>postmaster3000</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9adv8)<span>:</span>

> <span>Multitasking is a completely different thing than writing parallel code. With multitasking, the CPU and OS are executing completely different programs on separate threads. In parallel code, the same program is executing the same task on multiple threads.</span>

[<span>Terr_</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn98w8g)<span>:</span>

> <span>Imagine a shipwright in 1800, confidently predicting that neither larger nor faster ships are possible... due to the limitations of wood and sails, ignorant of what his descendants will do with metal, steam, composites, combustion...</span>
> 
> <span>Similarly, we aren't still making our CPUs out of metal gears... Don't confuse a shortage of opportunities for incremental improvement with a shortage of any opportunities.</span>

[<span>SarahC</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9akuy)<span>:</span>

> <span>Doesn't he realise we're stuck around 3.5GHz, and the only reason we went to multi core for the home user was to get more power out of a stalled clock speed processor?</span>
> 
> <span>There's faster cores, but the trash rate of them makes them unaffordable.</span>

[<span>quintric</span>](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn97ut0)<span>:</span>

> <span>Usually there are trade-offs involved with hitting massive levels of parallelism that involve sacrificing per-thread performance in favor of squeezing a higher number of simpler cores onto the same piece of silicon. Most consumers (myself included) would probably prefer the former over the latter for everyday computing needs.</span>

[<span>prennis</span>](http://www.reddit.com/r/LinuxActionShow/comments/2p8dfg/linus_the_whole_lets_parallelize_thing_is_a_huge/cmuafmd)<span>:</span>

> <span>Linus, you forgot about science. My colleagues and I routinely use 12 to hundreds of cores, because it is the fastest way for us to do our work.</span>

[<span>phearus-reddit</span>](http://www.reddit.com/r/LinuxActionShow/comments/2p8dfg/linus_the_whole_lets_parallelize_thing_is_a_huge/cmuixm2)<span>:</span>

> <span>That's true given how we have gone about putting software together for the last 30-50 years. However, when we reach the physical atomical limit of how small we can make transistors - also the limit for power and die-size (e.g. the end of Moore's Law), we will need to drastically change how we engineer software and software engineering processes if we are to see any further significant evolution in computing - even low-end consumer computing.</span>
> 
> <span>Something worth noting - take one core running at say 800Mhz (say) - its gonna get hot and consume a bit of power. Take 8 of those same cores each running at 100Mhz and they are not going to get as hot as their single cousin - though both (theoretically) can execute the same volume of execution cycles - but the 8 cores together will incur less thermal losses - thus will draw less power. This alone is worth learning how to better parallelize software. And yes, there are thread overheads to consider, too - but doesn't it make sense to put effort into learning how to minimise these overheads?</span>

[<span>TheEndIsNear17</span>](http://www.reddit.com/r/LinuxActionShow/comments/2p8dfg/linus_the_whole_lets_parallelize_thing_is_a_huge/cmvek7v)<span>:</span>

> <span>You also have to remember Amdahl's law. If you read further he also comments about using 4-6 cores for the average user makes a lot of sense. He also comments further on, that he is involved in a project that is working on optimizing performance for multiple cores, and how much work it takes to just squeeze a few more %'s of performance, and for a lot of use cases the effort doesn't justify the small gains.</span>
> 
> <span>Linus isn't saying, oh we should forget parallel completely, he's combating the idea that parallel will save everything, and we should just port over everything to parallel.</span>

[AntiProtonBoy](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cna4dkj):

> <div id="_mcePaste">It's also worth noting that parallel programming is a fairly narrow and specific subset of multi-core computing. In this case, similar type of problems are broken up and processed in parallel. Not many algorithms can exploit this. Whereas concurrent programming is more immediately useful for a broader selection of problems. In this case, multi-core computing takes form as a collection of non-blocking and asynchronous design patterns. I think concurrent programming will become increasingly dominant in the future. </div>

<div>[Lucky75](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9ptba): </div>

<div>

> <div>I generally rather dislike Linus, but to be fair, he does make a good point here. The main application for lots of smaller cores is when you talk about very distributed systems, up to the point where you're trying to simulate a neural net. And at that point you're not going to be using 200,000 PCs. He's just saying that in the user space, you'd rather have 4-8 fast cores over 30 slow cores, so there's not all that much of an inherent advantage to parallelize past that. Things are only so parallelizable before you start hitting bottlenecks anyway.</div>

<div>[greg_lw](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn98vlm): </div>

<div>

> <div>I'm torn. On one hand, I love how callously dismissive you are of the trinkets of the software industry, with all its smug, self-important delusions... on the other, if stupidly simple UIs hadn't been built, this would right now be a usenet group with a couple hundred readers.</div>
> 
> <div>I will vote your post up, down, touch the ground, swipe, pinch zoom, double tap.</div>

<div>[procyon112](http://www.reddit.com/r/programming/comments/2qsqus/linus_the_whole_lets_parallelize_thing_is_a_huge/cn9zy25): </div>

> <div>I mean, if we keep going down this route we'll end up writing reactive applications by using an interpreted scripting language to modify a markup syntax for static documents and then bootstrap a rendering style sheet over that mess to get it to show up right, while using a static file transfer protocol to issue commands to the file server to run its own interpreted script on a virtual machine running on an operating system that runs on another virtual machine that abstracts away the hardware to run an entire OS stack on top of another host OS running the same goddamn kernel. We wouldn't want to go down THAT road would we?</div>

<div>[Martin Thompson](https://groups.google.com/d/msg/mechanical-sympathy/Ds5HmaEcivw/DgKITXEXTecJ):</div>

> Bigger caches are of limited value once the working set is larger than the cache size[1]. In the low-latency space we often do crazy things to keep the entire application in cache but this is not the mainstream. Moving to large pages and having L2 support for these large pages can be more significant than actual cache size as we can now see for some large memory applications running on Haswell.
> 
> Rather than going parallel it is often more productive moving to cache friendly or cache oblivious (actually very cache friendly) data structures. It is very easy to make the argument that if a small proportion of the effort that went into Fork-Join and parallel streams was spent on providing better general purpose data structures, i.e. Maps and Trees, that are cache friendly then mainstream applications would benefit more than they do from FJ and parallel streams that are supposedly targeted at "solving the multi-core problem". It is not that FJ and parallel streams are bad. They are really an impressive engineering effort. However it is all about opportunity cost. When we choose to optimise we should choose what gives the best return for the investment.
> 
> A lot can be achieved with more course grain parallelism/concurrency. The Servlet model is a good example of this, or even how the likes of PHP can scale on the server side. Beyond this pipeling is often a more intuitive model that is well understood and practised extensively by our hardware friends.
> 
> When talking about concurrent access to data structures it is very important to separate query from mutation. If data structures are immutable, or support concurrent non-blocking reads, then these can scale very well in parallel and can be reasoned about. Concurrent update to any remotely interesting data structure, let alone full model, is very very complex and difficult to manage. Period. Leaving aside the complexity, any concurrent update from multiple writers to a shared model/state is fundamentally limited as proven by Universal Scalability Law (USL)[2]. As an industry we are kidding ourselves if we think it is a good idea to have concurrent update from multiple writers to any model when we expect it to scale in our increasing multi-core world. The good thing is that the majority of application code that needs to be developed is queries against models that do not mutate that often.
> 
> A nasty consequence of our industry desire to have concurrent access to shared state is that we also do it synchronously and that spreads to a distributed context. We need to embrace the world as being asynchronous as a way to avoid the latency constraints in our approaches to design and algorithms. By being asynchronous we can be non-blocking and set our applications free to perform better and be more resilient due to enforced isolation. Bandwidth will continue to improve at a great rate but latency improvements are leveling off.
> 
> My new years wishlist to platform providers would be the infrastructure to enable the development of more cache friendly, immutable, and append only data structures, better support for pipeline concurrency, non-blocking APIs (e.g. JDBC), language extensions to make asynchronous programming easier to reason about (e.g. better support for state machines and continuations), and language extensions for writing declarative queries (e.g. LINQ[3] for C# which could be even better). Oh yes, and don't be so shy about allowing lower level access from the likes of Java, we are well beyond writing applets that run in browser sandboxes these days!

</div>

## <span>Related Articles</span>

*   [On Hacker News](https://news.ycombinator.com/item?id=8820319) / [On Slashdot](http://developers.slashdot.org/story/15/01/02/024222/how-well-program-1000-cores---and-get-linus-ranting-again?utm_source=rss1.0mainlinkanon&utm_medium=feed)

*   <span>[The Machine: HP's New Memristor Based Datacenter Scale Computer - Still Changing Everything](http://highscalability.com/blog/2014/12/15/the-machine-hps-new-memristor-based-datacenter-scale-compute.html)</span>

*   [<span>Brawny cores still beat wimpy cores, most of the time</span>](http://static.googleusercontent.com/media/research.google.com/en/us/pubs/archive/36448.pdf) <span>- Slower but energy efficient “wimpy” cores only win for general workloads if their single-core speed is reasonably close to that of mid-range “brawny” cores.</span>

*   [<span>Amdahl's law in reverse: the wimpy core advantage</span>](http://yosefk.com/blog/amdahls-law-in-reverse-the-wimpy-core-advantage.html)

*   [<span>Stochastic Computing for Embedded Platforms</span>](https://www.intel-university-collaboration.net/wp-content/uploads/2012/06/ISTC-seminar-05-31-12.pdf)

*   [<span>Welcome to the Jungle</span>](http://herbsutter.com/welcome-to-the-jungle/) <span>- The free lunch is over. Now welcome to the hardware jungle.</span> 

*   [Ask For Forgiveness Programming - Or How We'll Program 1000 Cores](http://highscalability.com/blog/2012/3/6/ask-for-forgiveness-programming-or-how-well-program-1000-cor.html)

</div>

</div>