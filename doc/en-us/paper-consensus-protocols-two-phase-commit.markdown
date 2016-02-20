## [Paper: Consensus Protocols: Two-Phase Commit  ](/blog/2009/2/9/paper-consensus-protocols-two-phase-commit.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, February 9, 2009 at 12:28AM</span></div>

<div class="body">

Henry Robinson has created an excellent series of articles on consensus protocols. Henry starts with a very useful discussion of what all this talk about consensus really means: _The consensus problem is the problem of getting a set of nodes in a distributed system to agree on something - it might be a value, a course of action or a decision. Achieving consensus allows a distributed system to act as a single entity, with every individual node aware of and in agreement with the actions of the whole of the network._  

In this article Henry tackles Two-Phase Commit, the protocol most databases use to arrive at a consensus for database writes. The article is very well written with lots of pretty and informative pictures. He did a really good job.  

In conclusion we learn 2PC is very efficient, a minimal number of messages are exchanged and latency is low. The problem is when a co-ordinator fails availability is dramatically reduced. This is why 2PC isn't generally used on highly distributed systems. To solve that problem we have to move on to different algorithms and that is the subject of other articles.

</div>