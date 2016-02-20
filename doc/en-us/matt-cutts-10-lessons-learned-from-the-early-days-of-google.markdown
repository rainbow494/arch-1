## [Matt Cutts: 10 Lessons Learned from the Early Days of Google](/blog/2015/2/4/matt-cutts-10-lessons-learned-from-the-early-days-of-google.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, February 4, 2015 at 9:35AM</span></div>

<div class="body">

![](https://farm8.staticflickr.com/7419/16417842916_10ebfa1020_m.jpg)I mainly know of [Matt Cutts](https://plus.google.com/+MattCutts/posts), long time Google employee (since 2000) and currently head of Google's Webspam team, from his appearances on [TwiT](http://twit.tv/) with Leo Laporte. On TwiT Matt always comes off as smart, thoughtful, and a really nice guy. This you might expect.

<span>What I didn’t expect is in this talk he gave,</span> [<span>Lessons learned from the early days of Google</span>](https://www.mattcutts.com/blog/lessons-from-early-days-of-google/)<span>, is that Matt also turns out to be quite funny and a good story teller. The stories he’s telling are about Matt’s early days at Google. He puts a very human face on Google. When you think everything Google does is a calculation made by some behind the scenes AI, Matt reminds us that it’s humans making these decisions and they generally just do the best they can.</span>

<span>The primary theme of the talk is innovation and problem solving through creativity. When you are caught between a rock and a hard place you need to get creative. Question your assumptions. Maybe there’s a creative way to solve your problem?</span>

<span>The talk is short and well worth watching. There are lots of those fun little details that only someone with experience and perspective can give. And there’s lots of wisdom here too. Here’s my gloss on Matt’s talk:</span>

## 1\. Sometimes creativity makes a big difference.

<span>Matt Cutts’ first major project at Google was working on porn filters. After doing work on his own for awhile he couldn’t get people to help him, so his wife baked some cookies, Matt offered a cookie to anyone who could find porn on a server. This tactic was very successful and the cookies became known as Porn cookies. Other groups also adopted this tactic. Offer a challenge and reward people with a little token of appreciation, it can work wonders.</span>

## 2\. When faced with conflicting constraints a good executive can often find creative solutions that satisfy apparent conflicts.

<span>Google's first major controversy was not searching for</span> <span>_More Evil than Satan Itself_</span> <span>and returning</span> <span>Microsoft</span> <span>as the answer, it was around</span> [<span>DMCA takedown requests</span>](http://www.dmca.com/FAQ/What-is-a-DMCA-Takedown)<span>. Google received around 350 million DMCA takedown requests last year. Obviously you can’t process these all by hand, but at the same time it’s difficult to know if a request is valid or not. A takedown request for “Reefer Madness” may seem legitimate, but since it’s in the public domain it’s not really valid.</span>

<span>The very first takedown request was from the Church of Scientology to suppress a critic. The site they were trying to suppress was in Norway and the site owners didn’t want to issue a counter notification because they didn’t want to risk a lawsuit in the US. What should Google do?</span>

<span>When the going gets tough the tough get creative. </span>Google’s solution was to:

*   <span>Remove the page, but add a notice that said a search result was removed because of a DMCA takedown request.</span>

*   <span>Complaint were made available at</span> [<span>chillingeffects.org</span>](https://www.chillingeffects.org/)<span>, a group that talks about the effects of free speech and copyright on freedom of expression.</span>

<span>Google started doing the same thing for every legal removal they had to do. In China, when search results about Tiananmen Square are removed, there’s also a notice. It helps people get context on what the situation is.</span>

## <span>3\. Be proactive. No one cares about your career or your money than you do. Ask for what you want. You miss 100% of the shots you don’t take.</span>

<span>Matt was “volunteered” to work on the front-end of the ads product, where he worked for about a year. During this experience Matt could see people starting to spam Google. So Matt went to a VP of Engineering and said “I want to work on spam” and the VP said OK. Just like that. Before that Matt largely went along with what others thought he should do.</span>

<span>You would be amazed what a difference it makes to just tell someone “I would like to do X.” If you are manager, if someone wants to work on something they will work twice as hard.</span>

## <span>4\. Make your assumptions explicit and question them.</span>

<span>If all you have is conventional wisdom then it’s hard to set yourself apart from the crowd. It’s hard to get momentum. It’s hard to do something different. In that space is often where you find the most disruption and the most opportunity. What do you believe that nobody else believes?</span>

## 5\. The assumption questioning exercise.

<span>We are generally bad at questioning our own assumptions. One way to approximate that is to take something from current events and ask: what does that change? What is different in the world compared to how it used to be?</span>

<span>For example, the Affordable Care Act. Two big differences: 1) You can get health care with pr-existing conditions; 2) You don’t have to get insurance through your employer, you can go through exchanges.</span>

<span>The implications: 1) probably an increase in people going into business for themselves; 2) companies that rely on independent contractors, like Uber, might support the law.</span>

<span>Something like health insurance could have a really big ripple effect.</span>

<span>Here’s a Google example. Before Google search engines relied on really big iron. Huge expensive machines. Google scaled out on cheap commodity hardware. This meant that Google could grow a lot more cheaply. It also meant with more machines there’s more failure, so you had to figure out how to make the</span> [<span>whole more reliable than the parts</span>](http://highscalability.com/blog/2012/3/12/google-taming-the-long-latency-tail-when-more-machines-equal.html)<span>. It’s not easy.</span>

<span>And it’s not just buying cheap hardware. There are lots of insights you can have at inflection points. For example, a mechanical hard drive has a seek time of 10 msecs. If you have everything in RAM you can do a lot of seeks per second. Putting your entire index of the web in RAM is a lot of money, you can get much higher throughput, so the tradeoff might be worth it.</span>

## <span>6\. Was commodity hardware why Google succeeded?</span>

<span>No, success is hundreds of innovations. It’s not like there’s one shining epiphany and everything is set. So it wasn’t just cheap hardware, it wasn’t just Page Rank, it was Map Reduce, Spanner, and so on. It takes many innovations to make a successful company or a successful career.</span>

## <span>7\. Assumptions have to be questioned, circumstances change, you have to adapt to it. You can do a lot of cool stuff with data.</span>

<span>For a long time AI was stuck. In 1999 it was AI is stupid, it will never do anything. Things have changed today. A lot of the reason why is there’s a lot more data in the world.</span>

[<span>Brain</span>](http://www.wired.com/2012/06/google-x-neural-network/) <span>is system built by Google that uses a deep neural network to watch Youtube and see what it could learn. It developed it’s own neural network to recognize cats. This same technology was used to build better word recognition, so every Android phone has much better speech recognition due to deep learning, with a 30% drop in error rates and a compact model that could fit on a phone.</span>

<span>Now the technology has advanced to the point where Google can recognize pictures of waterfalls, or buildings, or Yosemite, palm trees, sea, snow. The computer can now even write a caption of a picture.</span>

## <span>8\. Things will go badly and you have to prepare yourself.</span>

<span>It wasn’t all cookies and success at Google. Matt remembers all the lawsuits and depositions, which isn’t fun for an engineer. It started with lawsuits from companies, then the lawsuits started coming from nations.</span>

<span>There will be dark days. There will be problems. So prepare yourself.</span>

## <span>9\. No matter what bad things are going to happen. Have some fun along the way and take some pictures.</span>

<span>You could record every meeting if you wanted too. Digital is cheap. Ten years from now you’ll want to remember that ping pong table where eight of you would sit around and talk about making Google search quality better. You’re going to want to remember that huge dog. You’re going to want to remember the good old days.</span>

<span>Try to take pictures when something funny or quirky happens. One example was a giant cookie sent in by someone wanting reinclusion into search. Other examples are April fools pranks and Halloween traditions.</span>

<span>There was a bet Matt made with his team where they could do anything they wanted to his hair of they did a really good job fighting spam for a quarter. They</span> [<span>cut it all off</span>](https://www.youtube.com/watch?v=zOVW2x-s0GM)<span>.</span>

<span>Interesting story about weekly meetings where employees could grill the execs about why they made the decisions they made.</span>

## <span>10\. Whatever you are working on, try to make sure it matters, it’s something you care about, and it’s something people want.</span>

<span>Matt has always thought of Google as a tool and they try to make it the best tool they can.</span>

<span>Fred Brooks in</span> [<span>The Computer Scientist as Toolsmith</span>](http://www.cs.unc.edu/~brooks/Toolsmith-CACM.pdf)<span>:</span>

> <span>If we perceive our role aright, we then see more clearly the proper criterion for success: a toolmaker succeeds as, and only as, the users of his tool succeed with his aid.</span>

## <span>Related Articles</span>

*   <span>Matt</span><span> [on Twitter](https://twitter.com/mattcutts)</span>

*   [Matt Cutts, Head of Web Spam Team at Google [Cool Tools Show Episode #20]](http://kk.org/cooltools/archives/23193)

*   [Matt on TWiT](http://twit.tv/show/this-week-in-google/227)

</div>