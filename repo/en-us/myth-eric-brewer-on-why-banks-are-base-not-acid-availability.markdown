## [Myth: Eric Brewer on Why Banks are BASE Not ACID - Availability Is Revenue ](/blog/2013/5/1/myth-eric-brewer-on-why-banks-are-base-not-acid-availability.html)

    

    

![](http://farm9.staticflickr.com/8538/8692301719_113dce65e6_m.jpg)

In [    NoSQL: Past, Present, Future    ](http://www.infoq.com/presentations/NoSQL-History) [Eric Brewer](https://twitter.com/eric_brewer) has a particularly fine section on explaining the often hard to understand ideas of [    BASE    ](http://queue.acm.org/detail.cfm?id=1394128) (Basically Available, Soft State, Eventually Consistent), [    ACID    ](http://en.wikipedia.org/wiki/ACID) (Atomicity, Consistency, Isolation, Durability), [    CAP    ](http://en.wikipedia.org/wiki/CAP_theorem) (Consistency Availability, Partition Tolerance), in terms of a pernicious long standing myth about the sanctity of consistency in banking.

    **Myth**        : Money is important, so banks         must         use transactions to keep money safe and consistent, right?    

    **Reality**        : Banking transactions are inconsistent, particularly for ATMs. ATMs are designed to have a normal case behaviour and a partition mode behaviour. In partition mode Availability is chosen over Consistency.    

**Why?** **1)** Availability correlates with revenue and consistency generally does not. **2)** Historically there was never an idea of perfect communication so everything was partitioned.

    Your ATM transaction must go through so Availability is more important than consistency. If the ATM is down then you aren’t making money. If you can fudge the consistency and stay up and compensate for other mistakes (which are rare), you'll make more money. That’s the space most enterprises find themselves so BASE is more popular than it used to be.    

    This is not a new problem for the financial industry. They’ve never had consistency because historically they’ve never had perfect communication. Instead, the financial industry depends on auditing. What accounts for the consistency of bank data is not the consistency of its databases but the fact that everything is         [written down twice and sorted out later](https://en.wikipedia.org/wiki/Double-entry_bookkeeping_system) using         a permanent and unalterable record that is reconciled later. The idea of financial compensation for errors is an idea built deeply into the financial system.    

During the Renaissance, when the [modern banking system](http://en.wikipedia.org/wiki/Luca_Pacioli) started to take shape, everything was partitioned. If letters, your data, are transported by horse or over ships, then it's likely you data will have a very low consistency, yet they still had an amazingly rich and successful banking system. Transactions were unnecessary. 

    ATMs, for example, chose     [    commutative operations    ](http://en.wikipedia.org/wiki/Serializability)     like increment and decrement, so the order in which the operations are applied doesn’t matter. They are reorderable and can be made consistent later. If an ATM is disconnected from the network and when the partition eventually heals, the ATM sends sends a list of operations to the bank and the end balance will still be correct. The issue is obviously you might withdraw more money than you have so the end result might be consistent, but negative, which can’t be compensated for by asking for the money back, so instead, the bank will reward you with an overdraft penalty.    

    The hidden philosophy is that you are trying to         **bound and manage your risk,**         yet still have all operations available. In the ATM case this would be a limit on the maximum amount of money you can take out at any one time. It’s not that big of a risk. ATMs are profitable so the occasional loss is just the risk of doing business.    

    Consistency it turns out is not the Holy Grail. What trumps consistency is:    

*       Auditing    
*       Risk Management    
*       Availability    

In a post-internet world where write availability is key the real world looks more like _weak consistency + delayed exceptions + compensation_ rather than a mistake free world of perfect communication and transactions. Just like the old days, but now you have far more options on the ACID <---> CAP spectrum.

## Related Articles 

*   [Discuss on Hacker News](https://news.ycombinator.com/item?id=5642010)
*   [](http://highscalability.com/blog/2012/9/24/google-spanners-most-surprising-revelation-nosql-is-out-and.html)[Google Spanner's Most Surprising Revelation: NoSQL Is Out And NewSQL Is In](http://highscalability.com/blog/2012/9/24/google-spanners-most-surprising-revelation-nosql-is-out-and.html)

    