## [5 Scalability Poisons and 3 Cloud Scalability Antidotes](/blog/2011/9/21/5-scalability-poisons-and-3-cloud-scalability-antidotes.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, September 21, 2011 at 9:03AM</span></div>

<div class="body">

[Sean Hull](http://www.iheavy.com/company/about/) with two helpful posts:

[5 Things That are Toxic to Scalability](http://www.iheavy.com/2011/08/26/5-things-are-toxic-to-scalability):

1.  **Object Relational Mappers.** Create complex queries that hard to optimize and tweak.
2.  **Synchronous, Serial, Coupled or Locking Processes.** Locks are like stop signs, traffic circles keep the traffic flowing. Row level locking is better than table level locking. Use async replication. Use eventual consistency for clusters.
3.  **One Copy of Your Database.** A single database server is a choke point.Create parallel databases and let a driver select between them.
4.  **Having No Metrics.** Visualize what's happening to your system using one of the many monitoring packages.
5.  **Lack of Feature Flags.** Be able to turn off features via a flag so when a spike hits features can be turned off to reduce load.

[3 Ways to Boost Cloud Scalability](http://www.iheavy.com/2011/07/30/3-ways-to-boost-cloud-scalability/):

1.  **Use Auto-scaling.** Spin-up new instances when a threshold is passed and back down again when traffic drops.
2.  **Horizontally Scale the Database Tier.** MySQL in a master-master active passive cluster configuration. As load grows roll in read-only slaves. As an alternative use Amazon's RDS. Scale up to a larger EC2 instance is the load on a single master is a problem.
3.  **Use Striped EBS Volumes.** For better EBS performance use Linux's software raid technology. EBS already has redundancy built in so you can stripe across a number of EBS volumes. 4 are recommended.

 This is just a brief summary. Please read the original articles for the full details.

</div>