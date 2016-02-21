## [Getting Things Right: A Look at Centralized vs Decentralized Systems Through the Eyes of Instant Replay](/blog/2014/9/15/getting-things-right-a-look-at-centralized-vs-decentralized.html)

    

    

_    Three baseball umpires were sitting around a bar, talking about how they make calls on each pitch: First umpire: Some are balls and some are strikes, and I call them as they are. Second umpire: Some are balls and some are strikes, and I call them as I see 'em. Third umpire: Some are balls and some are strikes, but they ain’t nothin' until I call 'em.    _

    

<table cellpadding="5" align="CENTER">

<tbody>

<tr>

<td valign="TOP">![](https://farm4.staticflickr.com/3879/15060227809_36f5e5a3b2_m.jpg)  
AT&T's Global [Network Operations Center](http://www.nj.com/business/index.ssf/2011/08/att_gnoc_earthquake.html)</td>

<td valign="TOP">![](https://farm4.staticflickr.com/3857/15060477967_72e9bc08e7_m.jpg)  
MLB's [Instant Replay Bunker](http://sports.yahoo.com/news/inside-look-at-mlb-s-new-instant-replay-bunker-032951044.html)</td>

<td valign="TOP">![](https://farm6.staticflickr.com/5577/15247210155_a31b5edd55_m.jpg)  
NHL's [Situation Room](http://blogs.canoe.ca/krykslants/nfl/special-report-inside-the-nhls-central-video-review-operation-can-it-work-in-the-nfl-with-tweaks-it-sure-can/)</td>

</tr>

</tbody>

</table>

    

**Update**: [Inside the NFL’s Replay Command Center](http://mmqb.si.com/2014/11/11/inside-the-nfls-replay-command-center/)

It’s fun to look at how concepts we think of as belonging primarily to the domain of computer science play out in other fields. One intriguing example is how [Instant Repla](http://en.wikipedia.org/wiki/Instant_replay)y reflects and even helps [shape](http://www.nfl.com/videos/nfl-network-top-ten/09000d5d811133e8/Top-Ten-Things-that-Changed-the-Game-Instant-replay) the culture of a sport by how replay is implemented: [decentralized](http://en.wikipedia.org/wiki/Distributed_computing) or [centralized](http://en.wikipedia.org/wiki/Centralized_computing).

Lucrative TV deals have pumped huge sums of money into professional sports. With so much money in play, sports have shifted from being pure entertainment to wanting to **get things right**. The price of making a bad call is just too high to let the human element decide the fate of titans.

Getting things right is also a much talked about subject in computer science. In CS the language of getting things right uses terms like transaction, rollback, quorum, optimistic replication, linearizability, synchronization, lock, eventually consistent, compensating transaction, and so on.

In sports to get things right referees use terms like flag, penalty, by rule, ruling stands, reset the clock, down and distance, line to gain, the whistle blew, ruling confirmed, and ruling overturned.

Though the vocabulary is different, the intent is much the same. Correctness.

Intent is not all tech and sports have in common. As technology evolves we are seeing sports change to take advantage of the new capabilities technology offers. And those changes should be familiar to anyone in software. Sports have gone from a completely decentralized system of officiating to where we now see the [NBA](http://www.si.com/nba/point-forward/2014/05/18/nba-instant-replay-off-site-review-adam-silver), [NFL](http://espn.go.com/nfl/story/_/id/10670707/nfl-owners-allow-centralized-system-aid-instant-replay), [MLB](http://sports.yahoo.com/news/inside-look-at-mlb-s-new-instant-replay-bunker-032951044.html), and [NHL](http://blogs.canoe.ca/krykslants/nfl/special-report-inside-the-nhls-central-video-review-operation-can-it-work-in-the-nfl-with-tweaks-it-sure-can/), all converging on some form of a centralized system.

The NHL were the innovators, starting their centralized instant replay system in 2011\. It works something like this...officials sit in a war room located in Toronto that looks a lot like every [network operations center](http://en.wikipedia.org/wiki/Network_operations_center) ever built. Video feeds from all games flow into the room. When there is a controversy or an obvious review-worthy play, Toronto is contacted for a quick review and judgement on the correct call.  Every sport will implement their own centralized replay system in their own way, but that's the gist of it.

We’ve seen the exact same transformation as federated services like email have been replaced with centralized services like Twitter and Facebook. It turns out sports and computer science have some deeper similarities. What might those be?

## Instant Replay Was Invented to Get Things Right

Getting things right has evolved over the years. To start with you need a set of rules. Without rules there’s no way to get things right. With a set of rules in place games can fall into one of several categories: self-refereed games, no-refereed games, or refereed games.

A **no-referee game** example was portrayed in an episode of the [Outlander TV show](http://www.imdb.com/title/tt3006802/), which is primarily set in the 18th-century. It had a great scene showing a game being played that looks like it might be called Scots-style field  hockey. Two teams hit a  ball around using big sticks. So far so familiar. Before long however the sticks were used as weapons and players were clubbing each other all over the place. It made for a bloody good game, but there wasn’t much of an emphasis on getting things right. Getting even...yes.

**Casual games**, a get together to play a pick-up game of soccer, basketball, or whatever, are played by a set of rules, but they are usually self-refereed. No referees are there to make calls. The games aren’t that important. Players will call fouls on themselves or opposing players might call fouls on the other side, but it’s all very informal and can lead to heated disagreements.

## Game Play is Decentralized and Lock Free

At this point in the evolution of games, games are completely **decentralized**. Games are completely independent of each other. Any number of games can be played simultaneously. All you need are enough players and a place to play.

Also note that games are **lock free**, with no form of currency control at all. All activities on the field run in parallel and the game state on the field is whatever happens on the field.

With a no-referee game there is no way to repair rules violations after the fact. With self-refereed games there is only a limited ability to repair rules violations. Partly because the rules for casual games aren’t that elaborate and partly because nobody playing the game would accept such judgements from other players.

## Refereed Games are Eventually Consistent

Which brings up the role of objective third party judges within games. Or as we call them, [referees](http://en.wikipedia.org/wiki/Referee). Referees make it possible to have much more elaborate rule sets because only a specialized person would be expected to know all the rules and have the skill to apply them.

The addition of independent referees to games also enables something much more subtle, it turned games into being [eventually consistent](http://en.wikipedia.org/wiki/Eventual_consistency), in the sense that all values eventually converge on a correct value. This is an interesting way to think about refereed games.

Game state, which are the values mentioned above, is modified by play on the field, but the changes are not committed transactions, so to speak. Referees can call a penalty which potentially changes the state of the game using the equivalent of a [compensating transaction](http://en.wikipedia.org/wiki/Compensating_transaction) to make up for an infraction.

In the NFL, for example, it’s the refs who decide where to place the ball, keep time, keep score, keep order, and generally everything else that happens on the field. Referees are needed to tell you what really happened on a play, they decide what has approved of visibility in the system.

Another way to think about referees is that they function as the **Read and Repair** mechanism described in [Ask For Forgiveness Programming](http://highscalability.com/blog/2012/3/6/ask-for-forgiveness-programming-or-how-well-program-1000-cor.html). The article shows how we might efficiently program processors with a 1000+ cores. The idea is to let all those cores operate simultaneously, in parallel, and then continually bring answers closer to correctness with background tasks that are charged with figuring out what went wrong and making adjustments.

Doesn’t this sound remarkably like how games operate in the meat world? In a game each entity--player, coach, referee, camera, etc.--is a core connected in a network. Messaging is verbal, through hand signals, or through radio signals. Plays map to protocols.

During a play state flows between the cores as a consequence of actions made by entities. Some activities are directly linked. Some are indirectly linked. Some are independent.

Take a complex NFL play. A ball is tipped (or whas is?), there’s an interception (of was there?), then the ball was fumbled (or was it?), there’s another fumble (or was there?), someone picks the ball up and runs in for a TD. To top it off there were two flags on the play in completely different parts of the field.

What really happened on the play?

To decide the referees get together in a quorum. After a conflict resolving conference, that may take no time at all or next to forever, the referees will come to a conclusion.  Referees are essentially reading events stored in a “log” in their mind, deciding on the order, comparing the events to the rules, deciding which rules apply, which rules have precedence, and then making a determination about what officially happened.

Once decided the new game state is committed. Necessary repairs via compensating transactions will be implemented. Perhaps a 15 yard penalty is marked off. Perhaps a turn over was declared and the ball now belongs to the other team. There are hundreds of potential outcomes that could be asserted by the referees. Whatever the referees say is the law.

Notice like in software the price of correctness is increased latency. A no-referee system has the lowest latency because play never stops to fix problems after the fact. With referees latency takes a huge leap. It takes a lot of time to decide penalties and implement any corrections. 

Is what the referees say what happened really what happened? This is a deep question about what is “truth”, that we’ll completely ignore. Any fan will tell you of course not. Refs blow calls all the time. But that doesn’t matter. In a game, like in a database, after a conflict resolution, the merged result becomes the new state. There is no higher authority.

## Video Creates a Shared Memory Log that Can Be Challenged

But wait. The NFL has developed a **challenge mechanism** so calls can be reviewed after the fact. A coach throws a red flag and the last committed transaction will be looked at again to see if mistakes were made. Other sports have similar mechanisms.

The challenge system is a consequence of **technological innovation**. Once games were filmed they could be recorded and replayed later. Video extends and decouples the “log” of events in both space and time. In space because the number of angles a play can be viewed from is only limited by the number of cameras. In time because a play can be viewed in slow motion, or in real-time, both forward and backward in time.

With this new tool referees could do something almost unimaginable decades early, they could take another look at a play after it had already been played. Instant Replay was born!

If on reviewing a play the referees found their call was wrong, they will issue more commands to fix whatever problems were caused, bringing the game to a more correct and consistent state. A good example of using **read repair** and compensating transactions to fix problems.

Making a change to game state after a replay is like what Amazon does if they sell an item that the system thought was available, but was actually out of stock. Does the world end if you buy an out of stock item? Not at all. Amazon will send you a polite notice saying the product is no longer available and that your order has been reversed. For Amazon gaining a sale is more profitable than making and correcting a mistake every once-in-a-while. Letting play unfold on the field in real-time is also worth dealing with flags after a play ends.

Typically the commands a referee issues to fix a previous problem are not themselves reviewable. Though many sports have an appeal process where the league office can look at a call and then say yes, the referees got that wrong, but there’s nothing we can do about it. Sometimes, very rarely, a challenge can cause a game to be restarted from the point of the bad call.

A recent game between the San Francisco Giants and the Chicago Cubs was restarted [after a protest](http://espn.go.com/chicago/mlb/story/_/id/11384538/san-francisco-giants-win-appeal-finish-game-vs-chicago-cubs). The game was played at the Cubs home ballpark and it was called because of some equipment problems on the field. The Giants were losing at the time so this would have been a big loss for them. The Giants appealed. And won. The powers-that-be deemed it wasn’t fair that an equipment problem would have gave the home team an undeserved victory.

**Justice is not the goal of an appeals system**. Rarely can a problem be fixed after a game has been played. An apology letter might be sent. Maybe the rules commission will look into changing the rules next year. Perhaps fines can be reduced or suspensions voided. Amazon might give you a discount on your next purchase. But normally, once the final whistle blows that game state is committed and the game transaction returns with a success return code.

So far there are some intriguing parallels between what happens on a sports field and what happens in computer science. This is because underneath all refereed systems is a **common structure.** There are rules, state, activities, and referees that interpret how rules are applied to the result of activities on state.

Databases are just one example of a larger class of refereed systems. House inspections, a trial, academic papers under peer review, insurance claim adjustment, movies entered into a contest--all only take on meaning when a judge says what they really are.

Note that with instant replay latency has increased again. Watching video takes a lot of time, much more time than a no-referee system or even a referee-only system. 

## More Technology Means More Getting Things Right - Centralized Systems

Technology leaps and bounds forward into the future, dragging society along with it.

Cameras and video playback were enabling technologies for instant replay on the field. Forming what I think of as a federated architecture. Each game is autonomous and decentrally organized, but rules and information are shared between games.

Since replay was instituted we’ve seen the **invention of fast internet**, powerful computers, even more on field cameras, and the ability to create complex software. So it makes sense a new evolution of instant replay was ready to be invented. And it was. This time it uses a centralized architecture. In fact, the NBA, NHL, MLB, and NFL have all moved to a centralized instant replay approach or are moving to one.

The idea is simple. It’s now possible to **stream every game** into a central operations center which allows a dedicated team of specialists to look at video and take a look at calls.

Again note, with centralized instant replay latency has increased yet again. Now we have to go to a central replay center for a judgement. We must really think correctness is important?

## Read and Repair Mechanisms Outside of the Game

Instant replay isn't the only read and repair mechanism available.

In baseball and football, for example, game statistics are frequently **corrected after a game**. Not everything can be tabulated correctly in the context of a game. With a little reflection a half a sack might turn into a full sack, for example.

A lot is happening on a field so even the referees can't see all the nasty things going on. That's why there's a **fine mechanism**. A fine can be levied after a game as background consistency check, even if a penalty wasn’t called in a game. 

## Deep Learning, Drones, Sensors and More Cameras - Hybrid or Closed Loop?

The next evolution of technology will likely involve **sophisticated robots**, **deep learning systems**, and even **more cameras** as sensors and drones will blanket a field with coverage. You can imagine a 1000 cameras covering each game and an AI examining each stream looking for infractions. The AIs for a game could vote on infractions and bubble up those calls with the highest confidence up to human minders. Humans might make the final call or they might not. 

Robots are currently used in Tennis as [electronic line judges](http://en.wikipedia.org/wiki/Electronic_line_judge).

There are robots that can call [balls and strikes](http://bleacherreport.com/articles/1942413-should-major-league-baseball-ever-bother-with-a-robotic-strike-zone) in baseball, but they aren’t used because baseball has such a strong tradition of using umpires.

It's **human conceit** that will determine how future generations of technology are used. If the human element in sports is valued above correctness then what will likely develop is a hybrid system that conceptually has a lot in common with modern software systems. We’ll still have human referees and robots will pick their spots. A centralized AI mediated component will do most of the heavy lifting and humans will provide oversight where deemed appropriate. Humans must still feel important after all.

Technology tends to move in circles. So where a little technology increased system latency and moved officiating from a decentralized to a centralized architecture, even more technology could **reverse those developments**.

A closed loop system, with each playing field having it's own cameras and it's own AIs, where AIs would make calls directly, would create a low latency system on par with a no-referee system and remove the centralized component completely. **Every game would once again be fast and decentralized**. 

No matter how sophisticated the system, in our mind’s eye, we’ll always know what really happened.

## Related Articles

*   [Upon Further Review: A Brief History of Instant Replay](http://mentalfloss.com/article/26075/upon-further-review-brief-history-instant-replay)

*   [Why Don’t All Sports Use The NHL’s ‘Control Room’ Replay Review System?](http://www.thesportsfanjournal.com/columns/the-rev/all-sports-should-use-the-nhl-control-room-replay-review-system)

*   [Changes made to NFL replay system](http://espn.go.com/nfl/story/_/id/10670707/nfl-owners-allow-centralized-system-aid-instant-replay)

*   [SPECIAL REPORT: Inside the NHL’s central video-review operation. Can it work in the NFL? With tweaks, it sure can](http://blogs.canoe.ca/krykslants/nfl/special-report-inside-the-nhls-central-video-review-operation-can-it-work-in-the-nfl-with-tweaks-it-sure-can/)

*   [MLB Must Turn to NHL-Style Instant Replay System to Fix Umpiring](http://bleacherreport.com/articles/1634267-mlb-must-turn-to-nhl-style-instant-replay-system-to-fix-umpiring)

*   [Players, umpires need replay now more than ever](http://sports.yahoo.com/news/players--umpires-need-replay-now-more-than-ever.html)

*   [How to End Baseball's Epic Officiating Screwups: Use Instant-Replay Umpires](http://www.theatlantic.com/entertainment/archive/2013/05/how-to-end-baseballs-epic-officiating-screwups-use-instant-replay-umpires/275726/) - Reflects organization structure and culture.

*   [Peter Gammons: Angel Hernandez Blew That Call After Ignoring The Replay](http://deadspin.com/peter-gammons-angel-hernandez-blew-that-call-after-ign-499913975)

*   [MLB's Crappy Replay Tech: It's A Miracle Umps Ever Get A Call Right](http://deadspin.com/mlbs-crappy-replay-tech-its-a-miracle-umps-ever-get-499041275)

*   [Careful What you Wish For](http://www.sportsonearth.com/article/70183162/major-league-baseball-instant-replay-may-be-overwhelming) - Think you know how to watch a baseball game? Just wait until additional replays from every possible angle are available. (AP)

*   [MLB 2014: How Will Baseball's Instant Replay Work?](http://online.wsj.com/news/articles/SB10001424052702304688104579465230759769104)

*   [“It Ain’t Nothing Until I Call It!”](http://bill37mccurdy.wordpress.com/2010/08/23/it-aint-nothing-until-i-call-it/) - the story of how baseball was saved by making umpires Gods on the field.

*   [Ask For Forgiveness Programming - Or How We'll Program 1000 Cores](http://highscalability.com/blog/2012/3/6/ask-for-forgiveness-programming-or-how-well-program-1000-cor.html)

*   [What The Heck Are You Actually Using NoSQL For?](http://highscalability.com/blog/2010/12/6/what-the-heck-are-you-actually-using-nosql-for.html)

    