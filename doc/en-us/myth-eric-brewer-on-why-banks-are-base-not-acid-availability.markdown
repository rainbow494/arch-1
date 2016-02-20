## [Myth: Eric Brewer on Why Banks are BASE Not ACID - Availability Is Revenue ](/blog/2013/5/1/myth-eric-brewer-on-why-banks-are-base-not-acid-availability.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, May 1, 2013 at 9:00AM</span></div>

<div class="body">

![](http://farm9.staticflickr.com/8538/8692301719_113dce65e6_m.jpg)

In [<span>NoSQL: Past, Present, Future</span>](http://www.infoq.com/presentations/NoSQL-History) [Eric Brewer](https://twitter.com/eric_brewer) has a particularly fine section on explaining the often hard to understand ideas of [<span>BASE</span>](http://queue.acm.org/detail.cfm?id=1394128) (Basically Available, Soft State, Eventually Consistent), [<span>ACID</span>](http://en.wikipedia.org/wiki/ACID) (Atomicity, Consistency, Isolation, Durability), [<span>CAP</span>](http://en.wikipedia.org/wiki/CAP_theorem) (Consistency Availability, Partition Tolerance), in terms of a pernicious long standing myth about the sanctity of consistency in banking.

<span>**Myth**</span><span>: Money is important, so banks</span> <span>must</span> <span>use transactions to keep money safe and consistent, right?</span>

<span>**Reality**</span><span>: Banking transactions are inconsistent, particularly for ATMs. ATMs are designed to have a normal case behaviour and a partition mode behaviour. In partition mode Availability is chosen over Consistency.</span>

**Why?** **1)** Availability correlates with revenue and consistency generally does not. **2)** Historically there was never an idea of perfect communication so everything was partitioned.

<span>Your ATM transaction must go through so Availability is more important than consistency. If the ATM is down then you aren’t making money. If you can fudge the consistency and stay up and compensate for other mistakes (which are rare), you'll make more money. That’s the space most enterprises find themselves so BASE is more popular than it used to be.</span>

<span>This is not a new problem for the financial industry. They’ve never had consistency because historically they’ve never had perfect communication. Instead, the financial industry depends on auditing. What accounts for the consistency of bank data is not the consistency of its databases but the fact that everything is</span> <span>[written down twice and sorted out later](https://en.wikipedia.org/wiki/Double-entry_bookkeeping_system) using</span><span> a permanent and unalterable record that is reconciled later. The idea of financial compensation for errors is an idea built deeply into the financial system.</span>

During the Renaissance, when the [modern banking system](http://en.wikipedia.org/wiki/Luca_Pacioli) started to take shape, everything was partitioned. If letters, your data, are transported by horse or over ships, then it's likely you data will have a very low consistency, yet they still had an amazingly rich and successful banking system. Transactions were unnecessary. 

<span>ATMs, for example, chose</span> [<span>commutative operations</span>](http://en.wikipedia.org/wiki/Serializability) <span>like increment and decrement, so the order in which the operations are applied doesn’t matter. They are reorderable and can be made consistent later. If an ATM is disconnected from the network and when the partition eventually heals, the ATM sends sends a list of operations to the bank and the end balance will still be correct. The issue is obviously you might withdraw more money than you have so the end result might be consistent, but negative, which can’t be compensated for by asking for the money back, so instead, the bank will reward you with an overdraft penalty.</span>

<span>The hidden philosophy is that you are trying to</span> <span>**bound and manage your risk,**</span> <span>yet still have all operations available. In the ATM case this would be a limit on the maximum amount of money you can take out at any one time. It’s not that big of a risk. ATMs are profitable so the occasional loss is just the risk of doing business.</span>

<span>Consistency it turns out is not the Holy Grail. What trumps consistency is:</span>

*   <span>Auditing</span>
*   <span>Risk Management</span>
*   <span>Availability</span>

In a post-internet world where write availability is key the real world looks more like _weak consistency + delayed exceptions + compensation_ rather than a mistake free world of perfect communication and transactions. Just like the old days, but now you have far more options on the ACID <---> CAP spectrum.

## Related Articles 

*   [Discuss on Hacker News](https://news.ycombinator.com/item?id=5642010)
*   [](http://highscalability.com/blog/2012/9/24/google-spanners-most-surprising-revelation-nosql-is-out-and.html)[Google Spanner's Most Surprising Revelation: NoSQL Is Out And NewSQL Is In](http://highscalability.com/blog/2012/9/24/google-spanners-most-surprising-revelation-nosql-is-out-and.html)

</div>