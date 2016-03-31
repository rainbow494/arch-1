## [Paper: Consensus Protocols: Paxos  ](/blog/2009/3/10/paper-consensus-protocols-paxos.html)

    

    

**Update: **[Barbara Liskov’s Turing Award, and Byzantine Fault Tolerance](http://hnr.dnsalias.net/wordpress/2009/03/barbara-liskovs-turing-award-and-byzantine-fault-tolerance/#).  

Henry Robinson has created an excellent series of articles on consensus protocols. We already covered his [2 Phase Commit](http://highscalability.com/paper-consensus-protocols-two-phase-commit) article and he also has a [3 Phase Commit](http://hnr.dnsalias.net/wordpress/?p=103) article showing how to handle 2PC under single node failures.  

But that is not enough! 3PC works well under node failures, but fails for network failures. So another consensus mechanism is needed that handles both network and node failures. And that's [Paxos](http://the-paper-trail.org/blog/?p=173).  

Paxos correctly handles both types of failures, but it does this by becoming inaccessible if too many components fail. This is the "liveness" property of protocols. Paxos waits until the faults are fixed. Read queries can be handled, but updates will be blocked until the protocol thinks it can make forward progress.  

The liveness of Paxos is primarily dependent on network stability. In a distributed heterogeneous environment you are at risk of losing the ability to make updates. Users hate that.

So when companies like Amazon do the seemingly insane thing of creating [eventually consistent databases](http://highscalability.com/scalability-perspectives-5-werner-vogels-amazon-technology-platform), it should be a little easier to understand now. Partitioning is required for scalability. Partitioning brings up these nasty consensus issues. Not being able to write under partition failures is unacceptable. Therefor create a system that can always write and work on consistency when all the downed partitions/networks are repaired. 

## Related Articles

*   [Google's Paxos Made Live – An Engineering Perspective](http://highscalability.com/googles-paxos-made-live-engineering-perspective)*   [ZooKeeper - A Reliable, Scalable Distributed Coordination System](http://highscalability.com/zookeeper-reliable-scalable-distributed-coordination-system)*   [Impossibility of Distributed Consensus with One Faulty Process](http://groups.csail.mit.edu/tds/papers/Lynch/pods83-flp.pdf) by Lynch et al*   [Consensus, impossibility results and Paxos](http://www.cs.cornell.edu/Courses/cs614/2007fa/Slides/FLP_and_Paxos.pdf) by Ken Birman*   [Paxos for System Builders](http://www.cnds.jhu.edu/pub/papers/cnds-2008-2.pdf) by Jonathan Kirsch and Yair Amir    