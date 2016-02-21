## [We are leaving 3x-4x performance on the table just because of configuration.](/blog/2014/11/19/we-are-leaving-3x-4x-performance-on-the-table-just-because-o.html)

    

    

![](https://farm6.staticflickr.com/5036/14028263590_848c224784_n.jpg)

Performance guru [Martin Thompson](https://twitter.com/mjpt777) gave a great talk at [Strangeloop](https://thestrangeloop.com/): [Aeron: Open-source high-performance messaging](https://www.youtube.com/watch?v=tM4YskS94b0), and one of the many interesting points he made was how much performance is being lost because were aren't configuring machines properly.

This point comes on the observation that **"Loss, throughput, and buffer size are all strongly related."**

Here's a gloss of Martin's reasoning. It's a problem that keeps happening and people aren't aware that it's happening because most people are not aware of how to tune network parameters in the OS.

The separation of programmers and system admins has become an anti-pattern. Developers don’t talk to the people who have root access on machines who don’t talk to the people that have network access. Which means machines are never configured right, which leads to a lot of loss. We are **leaving 3x-4x performance on the table just because of configuration**.

We need to workout how to bridge that gap, know what the parameters are, and how to fix them.

So **know your OS network parameters and how to tune them**.

## Related Articles

*   [Aeron: Do We Really Need Another Messaging System?](http://highscalability.com/blog/2014/11/17/aeron-do-we-really-need-another-messaging-system.html)
*   [Strategy: Exploit Processor Affinity For High And Predictable Performance](http://highscalability.com/blog/2012/3/29/strategy-exploit-processor-affinity-for-high-and-predictable.html)
*   [12 Ways To Increase Throughput By 32X And Reduce Latency By  20X](http://highscalability.com/blog/2012/5/2/12-ways-to-increase-throughput-by-32x-and-reduce-latency-by.html)
*   [Busting 4 Modern Hardware Myths - Are Memory, HDDs, And SSDs Really Random Access?](http://highscalability.com/blog/2013/6/13/busting-4-modern-hardware-myths-are-memory-hdds-and-ssds-rea.html)
*   [Paper: Network Stack Specialization For Performance](http://highscalability.com/blog/2014/2/12/paper-network-stack-specialization-for-performance.html) 
*   [Strategy: Use Linux Taskset To Pin Processes Or Let The OS Schedule It?](http://highscalability.com/blog/2013/10/23/strategy-use-linux-taskset-to-pin-processes-or-let-the-os-sc.html)
*   [Big List Of 20 Common Bottlenecks](http://highscalability.com/blog/2012/5/16/big-list-of-20-common-bottlenecks.html)
*   [The Secret To 10 Million Concurrent Connections -The Kernel Is The Problem, Not The Solution](http://highscalability.com/blog/2013/5/13/the-secret-to-10-million-concurrent-connections-the-kernel-i.html)
*   [Russ’ 10 Ingredient Recipe For Making 1 Million TPS On $5K Hardware](http://highscalability.com/blog/2012/9/10/russ-10-ingredient-recipe-for-making-1-million-tps-on-5k-har.html)
*   [42 Monster Problems That Attack As Loads Increase](http://highscalability.com/blog/2013/2/27/42-monster-problems-that-attack-as-loads-increase.html)
*   [9 Principles Of High Performance Programs](http://highscalability.com/blog/2014/5/21/9-principles-of-high-performance-programs.html)

    