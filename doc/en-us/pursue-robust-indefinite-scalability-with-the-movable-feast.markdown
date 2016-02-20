## [Pursue robust indefinite scalability with the Movable Feast Machine](/blog/2011/9/28/pursue-robust-indefinite-scalability-with-the-movable-feast.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, September 28, 2011 at 9:10AM</span></div>

<div class="body">

![](http://farm7.static.flickr.com/6158/6192442184_bca3fe3333_m.jpg)

And now for something completely different, brought to you by David Ackley and Daniel Cannon in their playfully thought provoking paper: [Pursue robust indefinite scalability](http://www.usenix.org/event/hotos11/tech/final_files/Ackley.pdf), wherein they try to take a fresh look at neural networks, starting from scratch.

What is this strange thing called [indefinite scalability](http://robust.cs.unm.edu/wiki/Indefinite_Scalability)? They sound like words that don't really go together:

> Indefinite scalability is the property that the design can support open-ended computational growth without substantial re-engineering, in as strict as sense as can be managed. By comparison, many computer, algorithm, and network designs -- even those that address scalability -- are only finitely scalable because their scalability occurs within some finite space. For example, an in-core sorting algorithm for a 32 bit machine can only scale to billions of numbers before address space is exhausted and then that algorithm must be re-engineered.
> 
> Our idea is to expose indefinitely scalable computational power to programmers using reinvented and restricted—but still recognizable—concepts of sequential code, integer arithmetic, pointers, and user-deﬁned classes and objects. Within the space of indefinitely scalable designs, consequently, we prioritize programmability and software engineering concerns well ahead of either theoretical parsimony or maximally efficient or ﬂexible tile hardware design.

It's easy to read indefinite as "infinite" here. So what would such machine look like? Well, they've built the [Movable Feast Machince](http://robust.cs.unm.edu/wiki/MFM/A_Movable_Stack_Machine) as an implementation of their ideas:

> We propose the indeﬁnitely scalable Movable Feast Machine, which defers many architectural decisions to an execution model that associates processing, memory, and communications functions with movable bit patterns rather than ﬁxed locations. We illustrate basic and novel computational elements such as self-healing wire, simple cell membranes for modularity, and robust stochastic sorting by movable self-replicating programs.
> 
> The Movable Feast Machine (MFM) is a computer architecture and computational model under development by the Robust Physical Computation Group based at the University of New Mexico.The MFM is distinguished by its emphases on indefinite scalability and multilevel robustness. Compared to a traditional computer architecture that permanently assigns fixed functions -- CPU, cache, memory, bus -- to different bulk regions of a machine, the MFM is an open-ended spatial tiling of relatively small and locally-connected hardware elements, and it associates processing, memory, and communications functions with movable bit patterns rather than fixed locations.
> 
> The Movable Feast Machine consists of a 2D grid in which each tile contains a processor with a fixed amount of volatile and non-volatile memory. Each processor can communicate with its nearest neighbor processors via point-to-point links. The computation model for the proposed machine consists of a set of “event windows” that involve a group of tiles communicating with each other to perform the computation. Note that many non-overlapping event windows can exist concurrently. One of the critiques for this proposed architecture is that the hardware costs will be too high for cost-effective computation.

Sounds a bit like [memristors](http://highscalability.com/blog/2010/5/5/how-will-memristors-change-everything.html) meet the [ambient cloud](http://highscalability.com/blog/2009/12/16/building-super-scalable-systems-blade-runner-meets-autonomic.html), which is why I find this idea so intriguing. It combines biological computing models, ideas from chemistry, and robust computing design. 

Their [presententation](http://www.usenix.org/event/hotos11/tech/slides/ackley.pdf) at HOTOS XIII has some interesting bullet points:

*   Computation must be born again.
*   Computation's Original Sin: Hardware Shall Be Reliable; Software Shall Be Efficient. Efficiency and robustness are mortal enemies.
*   The solution: Indefinite scalability
*   Sacrificing:
    *   Fixed-width addresses, unique node names.
    *   Logarithmic global communication cost
    *   Clock and phase synchronization
*   Embracing:
    *   Opportunistic reproduction for ||ism & robustness
    *   Movability for configuration, manifest destiny, ...
    *   Multilevel robustness: Up to the end-user

How all this can be programmed to solve problem in real-life, I don't know. But I really like the notion of starting to see the all of computing as a single programmable fabric instead of islands of chunky servers. It's time to [move the compute quanta down](http://highscalability.com/blog/2011/9/7/what-google-app-engine-price-changes-say-about-the-future-of.html) instead of up. Where this will lead, nobody knows. And they close their paper in that spirit: 

> And ﬁnally, to challenges on expressiveness—arguing the MFM will be too painful to program because our atom or event window is too small, or bonds too few, or lengths too short, or system dimensionality too low, we say: You might well be right. Let’s ﬁnd out.

## Related Articles

*   [Slide Deck](http://www.usenix.org/event/hotos11/tech/slides/ackley.pdf) for HotOS Presentation
*   [The Three Ages Of Google - Batch, Warehouse, Instant](http://highscalability.com/blog/2011/8/29/the-three-ages-of-google-batch-warehouse-instant.html)
*   YouTube: [A NAND gate in the Movable Feast Machine](http://www.youtube.com/watch?v=D5NVk12i3cM)
*   Move Feast Machine [Home Page](http://robust.cs.unm.edu/wiki/Main_Page)
*   YouTube: [Programming the Movable Feast Machine with λ-Codons](http://www.youtube.com/watch?v=DauJ51CTIq8)
*   [SFB/Individual project pages](http://robust.cs.unm.edu/wiki/SFB/Individual_project_pages)
*   [HOTOS XIII Conference Report](http://www.usenix.org/publications/login/2011-08/openpdfs/HOTOSXIIIreports.pdf)
*   [Pursue Robust Indefinite Scalability With The Movable Feast Machine](http://tuvalu.santafe.edu/events/workshops/images/b/b1/Ackley-csss-2011.pdf) by Dave Ackley

</div>