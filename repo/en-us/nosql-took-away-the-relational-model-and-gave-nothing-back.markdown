## [NoSQL Took Away the Relational Model and Gave Nothing Back](/blog/2010/10/28/nosql-took-away-the-relational-model-and-gave-nothing-back.html)

    

    

**Update**: Benjamin Black said he was the source of the quote and also said I was wrong about what he meant. His real point: _The meaning of the statement was that NoSQL systems (really the various map-reduce systems) are lacking a standard model for describing and querying and that developing one should be a high priority task for them._

At the [A NoSQL Evening in Palo Alto](http://www.meetup.com/Silicon-Valley-NoSQL/calendar/14727419/?joinFrom=event&success=profileUpdatedEventWelcome), an audience member, sorry, I couldn't tell who, said something I found really interesting: **NoSQL took away the relational model and gave nothing back.**

The idea being thatNoSQL has focussed on ease of use, scalability, performance, etc, but it has lost the idea of how data relates to other data. True to its name, the relational model is very good at capturing a managing relationships. With NoSQL all relationships have been pushed back onto the poor programmer to implement in code rather than the database managing it. We've sacrificed usability. NoSQL is about concurrency, latency, and scalability, but it's not about data.

My ears perked up because I said something similar a while back while commenting on VoltDB's criticism of the NoSQL transaction model: _I agree completely that moving the repair logic to the programmer is a recipe for disaster. Having programmers worry about read repair, vector clocks, the commutativity of transactions, how to design compensatory transactions to make up for previous failed transactions, and the other very careful bits of design, is asking for a very fragile system. ACID transactions are clean and understandable and that's why people like them._

Relationships are very much in the same spirit. Managing relationships without explicit support or multiple object transaction support puts a huge burden on the programmer. At one level key-value systems are far simpler at every level to use. That's great. But, for more complex data all that work really comes back and falls on the overburdened shoulders of the programmer. What I liked about this comment during the event is that it put an emphasis on making the programmers life easier across a wider variety of use cases, which is always a good thing, and it was worth surfacing.

## Related Articles

*   Interesting [Hacker News Thread](http://news.ycombinator.com/item?id=1847575), especially the commentary on state of the art IO subsystems.

    