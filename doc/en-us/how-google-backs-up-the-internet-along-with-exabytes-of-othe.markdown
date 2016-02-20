## [How Google Backs Up the Internet Along With Exabytes of Other Data](/blog/2014/2/3/how-google-backs-up-the-internet-along-with-exabytes-of-othe.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, February 3, 2014 at 8:56AM</span></div>

<div class="body">

![](http://farm2.static.flickr.com/1278/5179816576_2b186d41b3.jpg)

[<span>Raymond Blum</span>](https://plus.google.com/115038404694487056892/posts?hl=en) <span>leads a team of Site Reliability Engineers charged with keeping Google's data secret and keeping it safe. Of course Google would never say how much data this actually is, but from comments it seems that it is not yet a</span> [<span>yottabyte</span>](http://en.wikipedia.org/wiki/Yottabyte)<span>, but is many</span> [<span>exabytes</span>](http://en.wikipedia.org/wiki/Exabyte) <span>in size. GMail alone is approaching low exabytes of data.</span>

<span>Mr. Blum, in the video </span>[<span>How Google Backs Up the Internet</span>](http://www.youtube.com/watch?v=eNliOm9NtCM)<span>, explained common backup strategies don’t work for Google for a very googly sounding reason: typically they</span> <span>**scale effort with capacity**</span><span>. If backing up twice as much data requires twice as much stuff to do it, where stuff is time, energy, space, etc., it won’t work, it doesn’t scale.  You have to find efficiencies so that capacity can scale faster than the effort needed to support that capacity. A different plan is needed when making the jump from backing up one exabyte to backing up two exabytes. And the talk is largely about how Google makes that happen.</span>

<span>Some major themes of the talk:</span>

*   <span>**No data loss, ever**</span><span>. Even the infamous GMail outage did not lose data, but the story is more complicated than just a lot of tape backup. Data was retrieved from across the stack, which requires engineering at every level, including the human.</span>

*   **Backups are useless**. It’s the restore you care about. It’s a restore system not a backup system. Backups are a tax you pay for the luxury of a restore. Shift work to backups and make them as complicated as needed to make restores so simple a cat could do it.

*   **You can’t scale linearly**. You can’t have 100 times as much data require 100 times the people or machine resources. Look for force multipliers. Automation is the major way of improving utilization and efficiency.

*   **Redundancy in everything**. Google stuff fails all the time. It’s crap. The same way cells in our body die. Google doesn’t dream that things don’t die. It plans for it.

*   <span>**Diversity in everything**</span><span>. If you are worried about site locality put data in multiple sites. If you are worried about user error have levels of isolation from user interaction. If you want protection from a software bug put it on different software. Store stuff on different vendor gear to reduce large vendor bug effects.</span>

*   <span>**Take humans out of the loop**</span><span>. How many copies of an email are kept by GMail? It’s not something a human should care about. Some parameters are configured by GMail and the system take care of it. This is a constant theme. High level policies are set and systems make it so. Only bother a human if something outside the norm occurs.</span>

*   <span>**Prove it**</span><span>. If you don’t try it it doesn’t work. Backups and restores are continually tested to verify they work. </span>

<span>There’s a lot to learn here for any organization, big or small. Mr. Blum’s</span> <span>[talk](http://www.youtube.com/watch?v=eNliOm9NtCM) </span><span>is entertaining, informative, and well worth watching. He does really seem to love the challenge of his job.</span>

<span>Here’s my gloss on this very interesting talk where we learn many secrets from inside the beast:</span>

*   **<span>Data availability must be 100%</span><span>.</span> ****No data loss ever**.

    *   <span>Statistically if you lose 200K from a 2GB file it sounds good, but the file is probably now useless, think an executable or a tax return.</span>

    *   <span>Availability of data is more important the availability of access. If a system is down it’s not the end of the world. If data is lost, it is.</span>

    *   <span>Google guarantees you are covered for all of the following in every possible combination:  </span>

        *   <span>location isolation</span>

        *   <span>isolation from application layer problems</span>

        *   <span>isolation from storage layer problems</span>

        *   <span>isolation from media failure</span>

    *   <span>**Consider the dimensions you can move the sliders around on**</span><span>. Put software on the vertical and location on the horizontal. If you want to cover everything you would need a copy of layer of the software in different locations. You can do that with VMs in different locations.</span>

*   <span>**Redundancy is not the same as recoverability**</span><span>.</span>

    *   <span>Making many copies does not help meet the no loss guarantee.</span>

    *   <span>Many copies is effective for certain kinds of outages. If an asteroid hits a datacenter and you have a copy far away you are covered.</span>

    *   <span>If you have a bug in your storage stack, copying to N places doesn’t help because the bug corrupts all copies. See the GMail outage as an example.</span>

    *   <span>There aren’t as many asteroids as there are bugs in code, user errors, or writes of a corrupt buffer.</span>

    *   <span>Redundancy is good for locality of reference.  Copying is good when you want all data references as close as possible to where the data is being used.</span>

*   **The entire system is incredibly robust because there’s so much of it**.

    *   **Google stuff fails all the time**. It’s crap. The same way cells in our body die. We don’t dream that things don’t die. We plan for it. Machines die all the time.

    *   <span>**Redundancy is the answer**</span><span>. The result is more reliable on the aggregate than a single high quality machine. A single machine can be destroyed by an asteroid. Machines put in 50 different locations are much harder to destroy.</span>

*   <span>**Massively parallel system have more opportunities for loss**</span><span>.</span>

    *   <span>MapReduce running on 30K machines is great, until you have a bug. You have the same bug waiting to run everywhere at once which magnifies the effect.</span>

*   <span>**Local copies do not protect against site outages**</span><span>.</span>

    *   <span>If you have a flood in your server room RAID doesn’t help.</span>

    *   <span>Google File System (GFS), used throughout Google until about a year ago, takes the concept of RAID up a notch. Using</span> [<span>coding techniques</span>](http://en.wikipedia.org/wiki/Erasure_code) <span>to write to multiple datacenters in different cities at once, so you only need N-1 fragments to reconstruct the data. So with three datacenters once can die and you still have the data available.</span>

*   <span>**Availability and integrity are an organization wide characteristic**</span><span>.</span>

    *   <span>Google engineers, BigTable, GFS, Colossus, all know data durability and integrity is job one. Lots of systems in place to check and correct any lapses in data availability and integrity.</span>

*   <span>**You want diversity in everything**</span><span>.</span>

    *   <span>If you are worried about site locality put data in multiple sites.</span>

    *   <span>If you are worried about user error have levels of isolation from user interaction.</span>

    *   <span>If you want protection from a software bug put it on different software. Store stuff on different vendor gear to reduce large vendor bug effects.</span>

*   <span>**Tape to back things up works really really well**</span><span>.</span>

    *   <span>**Tape is great because it’s not disk**</span><span>. If they could they would use punch cards.</span>

    *   <span>Imagine if you have a bug in a device driver for SATA disks. Tapes save you from that. It increases your diversity because different media implies different software.</span>

    *   <span>Tape capacity is following Moore’s law, so they are fairly happy with tape as a backup medium, though they are working on alternatives, won’t say what they are.</span>

    *   <span>Tapes are encrypted implying that nefarious forces would have a hard time getting anything useful from them.</span>

*   <span>**Backups are useless. It’s the restore you care about**</span><span>.</span>

    *   <span>Find out if there’s a problem before someone needs the data. When you need a restore you really need it.</span>

    *   <span>**Run continuous restores**</span><span>. Constantly select at random 5% of backups and restore them to compare them. Why? To find out if backups work before data is lost. Catches a lot of problems.</span>

    *   <span>**Run automatic comparisons**</span><span>. Can’t compare to the original because the originals have changed. So checksum everything and compare the checksums. Get it back to the source media, disk, or flash, or wherever it came from. Make sure the data can do a round trip. This is done all the time.</span>

*   <span>**Alert on changes in rates of failure**</span><span>.</span>

    *   <span>**If something is different you probably want to know about it**</span><span>. If everything is running normally don’t tell me.</span>

    *   <span>Expect some failures, but don’t alert on a file that doesn’t restore on the first attempt.</span>

    *   <span>Let’s say the rate of failure on the first attempt is typically N. The rate of failure on the second attempt is Y. If there’s a change in the rates of failure then something has gone wrong.</span>

*   <span>**Everything breaks**</span><span>.</span>

    *   <span>Disk breaks all the time time but you know when it happens because you are monitoring it.</span>

    *   <span>With tape you don’t know it’s is broken until you try to use it. Tapes last a long time, but you want to test the tape before you need it.</span>

*   **[<span>RAID4</span>](http://en.wikipedia.org/wiki/Standard_RAID_levels#RAID_4) <span>on tape</span>**<span>.</span>

    *   <span>Don’t write to just one tape. They are cartridges. A robot might drop them or some magnetic flux might occur. Don’t take a chance.</span>

    *   <span>When writing to tape</span> <span>**tell the writer to hold on to the data**</span> <span>until we say it’s OK to change. If you do you’ve broken a contract.</span>

    *   <span>Build up 4 full tapes and then generate a 5th code tape by XORing everything together. You can lose any one of the 5 tapes and recover the data.</span>

    *   <span>Now tell the writer they can change the source data because the data has made it to its final physical location and is now redundant.</span>

    *   <span>Every bit of data Google backs up goes through this process.</span>

    *   <span>Hundreds of tapes a month are lost, but don’t have hundreds cases of data loss per month because of this process.</span>

    *   <span>If one tape is lost it is detected using the continuous restore and the sibling tapes are used to rebuild another tape and all is well. In the rare case where two tapes are corrupted you’ve only lost data if the same two spots on the tapes are damaged, so reconstruction is done at the subtape level.</span>

    *   <span>Don’t have data loss because of these techniques. It’s expensive, but it’s the cost of doing business.</span>

*   **Backups are a tax you pay for the luxury of a restore**.

    *   <span>**It’s a restore system not a backup system**</span><span>. Restores are a non-maskable interrupt. They trump everything. Get the backup restored.</span>

    *   <span>Make backups as complicated and take as long as they need. Make restores as quick and automatic as possible.</span><span></span>

    *   <span>Recovery should be stupid, fast, and simple. Want a cat to be able to trigger a central restore.</span>

    *   <span>Restores could happen when you are well rested or when you are dog tired. So you don’t want the human element to determine the success of restoring a copy of your serving data. You are under stress. So do all the work and thinking when you have all the time in the world, which is when making the backup.</span>

    *   <span>Huge percentage of systems work this way.</span>

    *   <span>Data sources may have to be able to store data for a period, perhaps days, before it can be promised that it is backed up. But once backed up, it can be restored and restored quickly.</span>

    *   <span>No longer make the most efficient use of media on backups in order to make restores faster. Taking two hours to read a tape is bad. Write only half a tape and read them in parallel so you get the data back in half the time.</span>

*   <span>**Scale is a problem**</span><span>.</span>

    *   <span>When you have exabytes of data there are real world constraints. If you have to copy 10 exabytes then it could take 10 weeks to backup every day’s data.</span>

    *   <span>With datacenters around the world you have a few choices. Do you give near infinite backup capacity to every site? Do you cluster all the backup by region? What about bandwidth to ship the data? Don’t you need the bandwidth to serve money making traffic?</span>

    *   <span>Look at relevant costs. There are compromises. Not every site has backup facilities. Must balance available capacity on the network. Where do you get the most bang for the buck. For example, backups must happen at site X because it has the bandwidth.</span>

*   **You can’t scale linearly**.

    *   <span>Can’t just say you want more network bandwidth and more tape drives. Drives break, so if you have 10,000 the number of drives you’ll need 10,000 times the number of operators to replace them. Do you have 10,000 times the amount of loading dock to put the tape drives on until a truck picks them up. None of this can be linear.</span>

    *   <span>Though the number of tape libraries has gone up a full order of magnitude, there isn’t 10 times as many people involved. There are some number more, but far from a linear increase.</span>

    *   <span>Example was an early prediction that as the number of telephones grew 30% of the US population would be employed as telephone operators. What they didn’t see coming is automated switching.</span>

*   <span>**Automate everything**</span><span>.</span>

    *   <span>Scheduling is automated. If you have a service you say I have a datastore and I need a copy every N and restores must happen in M. Internally systems make this happen. Backups are scheduled, restore testing is run, and integrity testing is run, etc. When a broken tape is detected it is automatically handled.</span>

    *   <span>You as a human don’t see any of this. You might someday ask how many tapes are breaking on average. Or an alert might go out if the rate of tape breakage changes from 100 tapes per day to 300 tapes per day. But until then don’t tell me if 100 tapes a day broke if that’s within the norm.</span>

*   <span>**Humans should not be involved in steady state operations**</span><span>.</span>

    *   <span>Packing up and shipping drives is still a human activity. Automated interfaces prepare shipping labels, get RMA numbers, check that packages have gone out, get acknowledgement of receipt, and if this breaks down a human as to intervene.</span>

    *   <span>**Library software maintenance likewise**</span><span>. If there’s a firmware update a human doesn’t run to every system and perform the upgrade. Download it. Let it get pushed to a canary library. Let it be tested. Let the results be verified as accurate. Then let it be pushed out. A human isn’t involved in normal operations.</span>

*   <span>**Handle machine death automatically**</span><span>.</span>

    *   <span>Machines are dying twice a minute. If a machine is dying during a MapReduce job that uses 30,000 machines don’t tell me about it, just handle it and move on. Find another machine, move the work, and restart.</span>

    *   **If there are dependencies then schedule a wait**. If you wait too long then let me know. You handle your own scheduling. This is the job for an algorithm, not a human.

*   <span>**Keep efficiency improving with growth**</span><span>.</span>

    *   <span>Improve utilization and efficiency a lot. Can’t have 100 times as much data require 100 times the people or machine resources.</span>

*   <span>**The Great GMail Outage and Restoral of 2011**</span><span>. Story of how Google dropped data and got it back.</span>

    *   <span>At 10:31AM on a Sunday he got a page that said “Holly Crap call xxx-xxxx”.  More on the outage</span> [<span>here</span>](http://gmailblog.blogspot.com/2011/02/gmail-back-soon-for-everyone.html)<span>.</span>

    *   <span>Gmail is approaching low exabytes of data. That’s a lot of tapes.</span>

    *   <span>100% recovery. Availability was not 100%. It wasn’t all there on the first or second day. But at the end of a period it was all there.</span>

    *   <span>A whole series of bugs and mishaps occurred in the layer where replication happens. Yes we had three identical files, but they are all empty. Even with unit tests, system tests, and integration tests, bugs get through.</span>

    *   <span>Restored from tape. Massive job. This is where restoral time is relative to scale. Getting back a gigabyte can be done in milliseconds to seconds. Getting back 200,000 inboxes of several gig each will take a while.</span>

    *   <span>Woke up a couple colleagues in Europe because they are were fresher. An advantage of distributed workforce.</span>

    *   <span>Data was restored from many tapes and verified. It didn’t take weeks or months, it took days. Which they were happy with. Other companies in similar situations have taken a month to realized they couldn’t get the data back. Steps have been taken to make sure the process would be faster next time.</span>

    *   <span>One tape drive takes 2 hours to read. The tapes were located all over the place. Otherwise no single location would have enough power to read all the tapes involved in the restoration process.</span>

    *   <span>With compression and checksums they actually didn’t need to read 200K tapes.</span>

    *   <span>The restoral process has been much improved since then.</span>

*   <span>**Prioritize restores**</span><span>.</span>

    *   <span>Archived data can be restored after more important data like your current inbox and sent email.</span>

    *   <span>Accounts that have not been touched in a month can wait while more active users are restored first.</span>

*   <span>**Backup system is viewed as a huge global organism**</span><span>.</span>

    *   <span>Don’t want GMail backups just in New York for example, because if that datacenter grew or shrank the backups would need to scale appropriately.</span>

    *   <span>Treat backup as one giant world spanning system. When a backup occurs it might be somewhere else entirely.</span>

    *   <span>A restore on a tape has to happen where the tape is located. But until it makes the tape the data could be in New York and the backup is in Oregon because that’s where there was capacity. Location isolation is handled automatically, no client is told where their data is backed up.</span>

    *   <span>Capacity can be moved around. As long as there’s global capacity and the network can support it it doesn’t matter where the tapes are located.</span>

*   <span>**The more data you have the more important it is to keep it**</span><span>.</span>

    *   <span>The larger things are the more important they are as a rule. Google used to be just search. Now it is Gmail, stuff held in drives, docs, etc. It’s both larger now and more important.</span>

*   <span>**Have a good infrastructure**</span><span>.</span>

    *   <span>Really good to have general purpose swiss army knives at your disposal. When MapReduce was written they probably never thought of it being used for backups. But without already having MapReduce the idea of using it for backups couldn’t have occurred.</span>

*   **Scaling is really important and you can’t have any piece of it--Software, Infrastructure, Hardware, Processes--that doesn’t scale**.

    *   <span>You can’t say I’m going to deploy more tape drives with having the operations staff. If you are going to hire twice as many people do you have twice as many parking spots? Room in the cafeteria? Restrooms? Everything has to scale up. You’ll hit one single bottleneck and it will stop you.</span>

*   <span>**Prove it**</span><span>.</span>

    *   <span>Don’t take anything for granted. Hope is not a strategy.</span>

    *   <span>If you don’t try it it doesn’t work. A restore must happen to verify a backup. Until you get to the end you haven’t proven anything. This attitude has found a lot of failures.</span>

*   <span>**DRT. Disaster recovery testing**</span><span>.</span>

    *   <span>Every N months a disaster scenario is played out. Simulate the response at every level of the organization.</span>

    *   <span>How will the company survive without whatever was taken away by the disaster? Must learn to adapt.  </span>

    *   <span>Finds enormous holes in infrastructure and physical security.</span>

    *   <span>Imagine a datacenter with one road leading to it and there are trucks loaded with fuel for the backup generators on the road. What happens when the road is lost? Better have another road and another supplier for diesel fuel.</span>

    *   <span>Do have supply chain redundancy strategies.</span>

*   <span>**Redundancy in different software stacks at different locations at different points in in time**</span><span>.</span>

    *   <span>**Don’t let just data migrate through the stack**</span><span>. Keep the data in different layers of the stack for a particular dwell period. So if you lose this and this you have the data somewhere in an overlap. Time, location, and software.</span>

    *   <span>Consider the GMail outage example. How if replication was corrupted could no data be lost? This was a question from the audience and he didn’t really want to give details. Data is constantly being backed up. Let’s say we have the data as of 9PM. Let’s say the corruption started at 8PM but hadn’t made it to tape yet. The corruption was stopped. Software was rolled back to a working release. At some point in the stack all the data is still there. There’s stuff on tape. There’s stuff being replicated. There’s stuff on the front-ends. There’s stuff in logs. There was overlap from all these sources and it was possible to reconstruct all the data. Policy is to not take data out of one stuck until N hours after it has been put in another stack for just these cases.</span>

*   <span>**Delete problem**</span><span>.</span>

    *   <span>I want to delete this. Not going to rewrite tapes just to delete data. It’s just too expensive at scale.</span>

    *   <span>One approach is to do something smart with encryption keys. He didn’t tell us what Google does. Perhaps the key is lost which effectively deletes the data?</span>

*   <span>**A giant organization can work when you trust your colleagues and shard responsibilities**</span><span>.</span>

    *   <span>Trust that they understand their part.</span>

    *   <span>Make sure organizational and software Interfaces are well defined. Implement verification tests between layers.</span>

*   **Whitelisting and blacklisting**.

    *   <span>Ensure data is in a guaranteed location and guaranteed not to be in a certain location, which goes against much of the rest of the philosophy which is location diversity and location independence.</span>

    *   <span>Was not originally a feature of the stack. Had to add in to support government requirements.  </span>

    *   <span>Responsibility pushed down as low as possible in the stack. Fill out the right profile and it magically happens.</span>

##  Related Articles

*   [On Reddit](http://www.reddit.com/r/programming/comments/1wzsmk/how_google_backs_up_the_internet_along_with/)

</div>