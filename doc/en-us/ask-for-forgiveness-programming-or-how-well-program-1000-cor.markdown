## [Ask For Forgiveness Programming - Or How We'll Program 1000 Cores](/blog/2012/3/6/ask-for-forgiveness-programming-or-how-well-program-1000-cor.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, March 6, 2012 at 9:15AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7186/6959224869_67f5d44763_m.jpg)

The argument for a massively multicore future is now familiar: while clock speeds have leveled off, device density is increasing, so the future is cheap chips with hundreds and thousands of cores. That’s the inexorable logic behind our multicore future.

The unsolved question that lurks deep in the dark part of a programmer’s mind is: how on earth are we to program these things? For problems that aren’t [<span>embarrassingly parallel</span>](http://www.futurechips.org/thoughts-for-researchers/parallel-programming-gene-amdahl-said.html)<span>, we really have no idea. IBM Research’s</span> [<span>David Ungar</span>](http://en.wikipedia.org/wiki/David_Ungar) <span>has an idea. And it’s radical in the extreme...</span>  

[<span>Grace Hopper</span>](http://highscalability.com/blog/2012/3/1/grace-hopper-to-programmers-mind-your-nanoseconds.html) <span>once advised “It's easier to ask for forgiveness than it is to get permission.” I wonder if she had any idea that her strategy for dealing with human bureaucracy would the same strategy David Ungar thinks will help us tame  the technological bureaucracy of 1000+ core systems?</span>  

<span>You may recognize David as the co-creator of the</span> [<span>Self programming</span>](http://en.wikipedia.org/wiki/Self_(programming_language)) <span>language, inspiration for the HotSpot technology in the JVM and the prototype model used by Javascript. He’s also the innovator behind using</span> [<span>cartoon animation techniques</span>](about:blank) <span>to build user interfaces. Now he’s applying that same creative zeal to solving the multicore problem.</span>  

<span>During a talk on his research,</span> [<span>Everything You Know (about Parallel Programming) Is Wrong! A Wild Screed about the Future</span>](http://www.cmu.edu/silicon-valley/news-events/seminars/2011/ungar-talk.html)<span>, he called his approach “anti-lock or “race and repair” because the core idea is that the only way we’re going to be able to program the new multicore chips of the future is to sidestep</span> [<span>Amdhal’s Law</span>](http://en.wikipedia.org/wiki/Amdahl's_law) <span>and program without serialization, without locks, embracing non-determinism. Without locks calculations will obviously be wrong, but correct answers can be approached over time using techniques like</span> [<span>freshener</span>](http://soft.vub.ac.be/~smarr/renaissance/ungar-kimelman-adams-2011-inconsistency-robustness-for-scalability-in-interactive-concurrent-update-in-memory-molap-cubes.pdf)<span>s:</span>

> <span>A thread that, instead of responding to user requests, repeatedly selects a cached value according to some strategy, and recomputes that value from its inputs, in case the value had been inconsistent. Experimentation with a prototype showed that on a 16-core system with a 50/50 split between workers and fresheners, fewer than 2% of the queries would return an answer that had been stale for at least eight mean query times. These results suggest that tolerance of inconsistency can be an effective strategy in circumventing Amdahl’s law.</span>

<span></span>  
<span>During his talk David mentioned that he’s trying  to find a better name than “anti-lock or “race and repair” for this line of thinking. Throwing my hat into the name game, I want to call it</span> <span>_Ask For Forgiveness Programming_</span> <span>(AFFP), based on the idea that using locks is “asking for permission” programming, so not using locks along with fresheners is really “asking for forgiveness.” I think it works, but it’s just a thought.</span>

## <span>No Shared Lock Goes Unpunished</span>

<span>Amdahl’s Law is used to understand why simply having more cores won’t save us for a</span> [<span>large class of problems</span>](http://www.scl.ameslab.gov/Publications/Gus/AmdahlsLaw/Amdahls.html)<span>. The idea is that any program is made up of a serial fraction and a parallel fraction. More cores only helps you with the parallel portion. If an operation takes 10 seconds, for example, and one second of it is serial, then having infinitely many cores will only help you make the parallelizable part faster, the serial code will always take  one second. Amdahl says you can never go faster than that 10%. As long as your code has a serial portion it’s impossible to go faster.</span>

Jakob Engblom recounts a similar line of thought in [his blog](http://jakob.engbloms.se/archives/1612):

> They also had the opportunity to test their solution [for parallel Erlang] on a Tilera 64-core machines. This mercilessly exposed any scalability limitations in their system, and proved the conventional wisdom that going beyond 10+ cores is quite different from scaling from 1 to 8… The two key lessons they learned was that no shared lock goes unpunished, and data has to be distributed as well as code.  
>   
> <span>It seems that for all big system parallelization efforts turn into a hunt for locks and the splitting up of code and data into units that can run in parallel without having to synchronize. The “upper limit” of this process is clearly a system with no synchronization points at all.</span>

Sherlock Holmes says that when you have eliminated the impossible, whatever remains, however improbable, must be the truth, so the truth is: removing serialization is the only way to use all these cores. Since synchronization is the core serial component to applications, we must get rid of synchronization.

## <span>A Transaction View Isn’t as Strong as it Once Was</span>

Getting rid of locks/synchronization may not be the radical notion it once was. Developments over the last few years have conditioned us to deal with more ambiguity, at least for distributed systems.  

<span>Through many discussions about CAP, we’ve come to accept a non-transactional view of databases as a way to preserve availability in the face of partitions. Even if a read doesn’t return the last write, we know it will eventually and that some merge logic will make them consistent once again.</span>  

<span>The idea of</span> [<span>compensating transactions</span>](http://en.wikipedia.org/wiki/Compensating_transaction) <span>as a way around the performance problems due to distributed coordination has also become more familiar.</span>  

<span>Another related idea comes from the realm of</span> [<span>optimistic concurrency control</span>](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) <span>that lets multiple transactions proceed in parallel without locking and then checks at commit time if a conflict requires a rollback. The application then gets to try again.</span>  

<span>And for some time now memcache has supported a</span> [<span>compare-and-set</span>](http://neopythonic.blogspot.com/2011/08/compare-and-set-in-memcache.html) <span>operation that allows multiple clients to avoid writing over each other by comparing time stamps.</span>  

<span>As relaxed as these methods are, they all still require that old enemy: synchronization.</span>

## <span>Your Answers are Already Wrong</span>

<span>The core difficulty with abandoning synchronization is coming to terms with the notion that results may not be correct at all times. It’s a certainty we love certainty, so even considering we could get wrong answers for any window of time is heresy.</span>  

<span>David says we need to change our way of thinking. The idea is to get results that are “good enough, soon enough.” Get wrong answers quickly, but that are still right enough to be useful. Perfect is the enemy of the good and perfect answers simply take too long at scale.</span>  

<span>David emphasises repeatedly that there’s a fundamental trade-off between correctness and performance. Without locks operations happen out of order, which gives the wrong answer. Race conditions happen. To get correct answers we effectively add in delays, making one thing wait for another, which kills performance, and we don’t want to kill performance, we want to use all those cores.</span>  

<span>But consider an aspect of working on distributed systems that people don’t like to think about: your answers are already always wrong.</span>  

<span>Unless you are querying a read-only corpus or use a global lock, in distributed systems any answer to any query is potentially wrong, always. This is the hardest idea to get into your brain when working in distributed systems with many cores that experience simultaneous updates. The world never stops. It’s always in flux. You can never assume for a single moment there’s a stable frame of reference. The answer from a query you just made could be wrong in the next instant. A query to see if a host is up, for example, can’t ever be assumed right. That host maybe up or down in the next instant and your code won’t know about it until it finds a conflict.</span>  

<span>So are we really that far away from accepting that all answers to queries could be wrong?</span>

## <span>Lessons from Nature</span>

<span>One strategy for dealing with many cores is to move towards biological models instead of mathematical models, where complicated behaviours emerge without global determinism. Bird</span> [<span>flocks</span>](http://en.wikipedia.org/wiki/Flocking_(behavior))<span>, for example, emerge from three simple rules: avoid crowding, steer towards average heading of neighbors,  steer towards average position of neighbors. No pi-calculus required, it works without synchronization or coordination. Each bird is essentially its own thread, it simply looks around and makes local decisions. This is a more</span> [<span>cellular automaton</span>](http://en.wikipedia.org/wiki/Cellular_automaton) <span>view of the world.</span>

## <span>Race and Repair - Mitigating Wrong Results</span>

<span>The idea is that errors created by data races won’t be prevented, they will be repaired. A calculation made without locks will be wrong under concurrent updates. So why not use some of our many cores to calculate the right answers in the background, in parallel, and update the values? This approach:</span>

*   <span>Uses no synchronization.</span>
*   <span>Tolerates some wrong answers.</span>
*   <span>Probabilistically fixes the answers over time.</span>

<span>Some obvious questions: how many background threads do you need? What order should values be recalculated? And how wrong will your answers be?</span>  

<span>To figure this out an experiment was run and described in</span> [<span>Inconsistency Robustness for Scalability in Interactive Concurrent‑Update In-Memory MOLAP Cubes</span>](http://soft.vub.ac.be/~smarr/renaissance/ungar-kimelman-adams-2011-inconsistency-robustness-for-scalability-in-interactive-concurrent-update-in-memory-molap-cubes.pdf)<span>, which test updates on a complicated spreadsheet.</span>  

<span>With locking the results were correct, but scalability was limited. Without locking, results were usually wrong. Both results are as might be expected.</span> And when they added freshener threads they found:

> Combining the frequency with the duration data suggests that in a 16-core system with a 50/50 split between workers and fresheners, fewer than 2% of the queries would return an answer that had been stale for at least eight mean query times.

I found this result quite surprising. I would have expected the answers to be wrong more of the time. Could this actually work?

### <span>Breadcrumbs</span>

<span>The simple idea of Race and Repair is open to a lot of clever innovations. Breadcrumbs are one such innovation that attempts to be smarter about which values need recalculating. Meta-data is attached on a value indicating that a entity is recalculating the value or has changed a dependent value such that this value is now out of date. Any entity that might want to use this data can wait until a “valid” value is calculated and/or not to insert a calculated value if it is out of data. This narrows the window of time in which errors are introduced. It’s a mitigation.</span>

<span>There are endless variations of this. I can imagine remembering calculations that used a value and then publishing updated values so those calculations can be rerun. The result is a roiling  event driven sea that is constantly bubbling with updates that are trying to bring values towards correctness, but probably never quite getting there.</span>

## <span>Probabilistic Data Structures</span>

<span>Another area David has researched are hashtables that can be inserted into without synchronization. This would allow entries to be added to a hashtable from any number of cores without slowing down the entire system with a lock. Naive insertion into a hashtable in parallel will mess up pointers, which can either result in the loss of values or the insertion of values, but it’s possible to work around these issues and create a lock free hashtable.</span>  

<span>His</span> [<span>presentation</span>](http://www.cmu.edu/silicon-valley/news-events/seminars/2011/ungar-talk.html) <span>goes into a lot of detail on how this might work. He rejects light-weight locking schmes like</span> [<span>CAS</span>](http://en.wikipedia.org/wiki/Compare-and-swap) <span>because these are still a choke point and there’s a penalty for atomic instructions under contention. They won’t scale.</span>  

<span>He thinks there’s a big research opportunity in probabilistic data structures that work without synchronization and that work with mitigation.</span>

## <span>Is this the Future?</span>

<span>This is just a light weight introduction. For more details please read all the papers and watch all the videos. But I think it’s important to talk about and think about how we might make use of all these cores for traditional programming tasks, though the result may be anything but traditional.</span>  

<span>A bad experience on one project makes me somewhat skeptical that human nature will ever be comfortable in accepting wrong answers as the norm. My experience report was on an event driven system with a large number of nodes that could generate events so fast that events had to be dropped. Imagine a city undergoing an earthquake and a huge sensor net spewing out change events to tell the world what’s going on in real-time.</span>  

<span>The original requirement was events could never be dropped, ever, which made a certain amount of sense when the number of sensors was small. As the number of sensors expands it’s simply not possible. My proposal was to drop events and have background processes query the actual sensors in the background so that the database would be synced over time. Very much like the proposals here.</span>  

<span>It was a no go. A vehement no go. Titanic arguments ensued.  Management and everyone on down simply could not accept the idea that their models would be out of sync with the sensors (which of course they were anyway). My eventual solution took a year to implement and radically changed everything, but that was simpler than trying to convince people to deal with uncertainty.</span>  

<span>So there might be some convincing to do.</span>

## <span>Related Articles</span>

*   [<span>Everything You Know (About Parallel Programming) Is Wrong!: A Wild Screed About the Future</span>](http://www.cmu.edu/silicon-valley/news-events/seminars/2011/ungar-talk.html) <span>(</span>[<span>slides</span>](http://www.dynamic-languages-symposium.org/dls-11/program/media/Ungar_2011_EverythingYouKnowAboutParallelProgrammingIsWrongAWildScreedAboutTheFuture_Dls.pdf)<span>,</span> [<span>video</span>](https://cmusv.adobeconnect.com/_a829716469/p1vdztyrp8e/?launcher=false&fcsContent=true&pbMode=normal)<span>)</span>
*   [<span>Renaissance Project</span>](http://soft.vub.ac.be/~smarr/renaissance)
*   <span>[On Hacker News](https://news.ycombinator.com/item?id=3471970)</span>
*   [On Reddit](http://www.reddit.com/r/programming/comments/s6mkb/ask_for_forgiveness_programming_or_how_well/)
*   [<span>David Ungar: It is Good to be Wrong</span>](http://jakob.engbloms.se/archives/1612)<span></span>
*   [<span>We Really Don't Know How To Compute!</span>](http://www.infoq.com/presentations/We-Really-Dont-Know-How-To-Compute)
*   [<span>Harnessing emergence for manycore programming: early experience integrating ensembles, adverbs, and object-based inheritance</span>](http://soft.vub.ac.be/~smarr/renaissance/ungar-adams-2010-harnessing-emergence-for-manycore%20programming-early-experience-integrating-ensembles-adverbs-and-object-based-inheritance.pdf)
*   <span>[Inconsistency Robustness for Scalability in Interactive Concurrent‑Update In-Memory MOLAP Cubes](http://soft.vub.ac.be/~smarr/renaissance/ungar-kimelman-adams-2011-inconsistency-robustness-for-scalability-in-interactive-concurrent-update-in-memory-molap-cubes.pdf)</span>

</div>