## [What can the Amazing Race to the South Pole Teach us About Startups?](/blog/2013/8/19/what-can-the-amazing-race-to-the-south-pole-teach-us-about-s.html)

    

    

[![](http://farm6.staticflickr.com/5344/9544732758_90d1d29178_m.jpg)](http://farm6.staticflickr.com/5344/9544732758_fc9a21bb20_o.jpg)

At the heart of every software adventure exists a journey in service of a quest. Melodramatic much? Sorry, but while wandering dazzled through [Race to the End of the Earth](http://explore.royalbcmuseum.bc.ca/), a fantastic exhibit at the [Royal BC Museum](http://royalbcmuseum.bc.ca/) on the 1911-1912 [race to the South Pole](http://www.bbc.co.uk/history/british/britain_wwone/race_pole_01.shtml) between Norwegian explorer [Roald Amundsen](http://en.wikipedia.org/wiki/Roald_Amundsen) and British naval officer [Robert Scott](http://en.wikipedia.org/wiki/Robert_Falcon_Scott), I couldn’t help but think of the two radically different approaches each team took to the race and it shocked me to see that some of the same principles that lead to success or failure in software development also seem to lead to success or failure in exploration.

I wish I could reproduce the experience of [walking through the exhibit](http://www.nydailynews.com/new-york/amnh-newest-exhibit-race-earth-gallery-1.49086). Plaque after plaque I remember wondering out loud at Scott’s choices and then nod in agreement with Amundsen’s approach. The core conflict was straight out of any ancient Agile (Amundsen) vs Waterfall (Scott) thread you can find on Usenet. And Waterfall lost.

    As background here are some sources you may want to read to understand more about the race.     [    Race to The End: Amundsen, Scott, and the Attainment of the South Pole    ](http://www.amazon.com/Race-The-End-Amundsen-Attainment/dp/1402770294)     is one of the books they sold at the museum. There are plenty of other books to choose from as well.     [    The race to the South Pole    ](http://www.rmg.co.uk/explore/sea-and-ships/in-depth/race-to-south-pole/the-race-to-the-south-pole)     seems like a good online source for the story as does     [    The Tragic Race to Be First to the South Pole    ](http://www.wired.com/wiredscience/2010/05/polar-race-gallery/)     in Wired.     

    In short: the goal of each expedition was to be first to the South Pole. Each leader approached their task in radically different ways, stemming from their different goals, experiences, and temperaments.  Amundsen’s team arrived in good health 33 days before Scott’s malnourished and exhausted team learned the devastating news that they were too late. Amundsen’s team returned home without losing a single life. Tragically, all five men in Scott's polar party died on the ice returning from the pole.     

    For detailed list of the difference between the two teams take a look at     [    Comparison of the Amundsen and Scott Expeditions    ](http://en.wikipedia.org/wiki/Comparison_of_the_Amundsen_and_Scott_Expeditions)    ,     [    10 Mistakes That Caused the Most Punishing Nature Expedition in History    ](http://io9.com/5874145/ten-deadly-mistakes-made-on-the-real-terra-nova-expedition)    , and     [    The South Pole Fifty Years After    ](http://arctic.synergiesprairies.ca/arctic/index.php/arctic/article/view/3571)    . Of course I didn’t have access to this information at the time, so I’ll weave in this data into some software development lessons:     

**Single focus**. This was the biggest flaming obvious difference to me. Amundsen had a single goal: get to the South Pole as fast as possible. Scott had twin goals: win the race and conduct scientific studies. 

    For Scott the science mission that had priority in terms of resources and personnel. While Amundsen was able to plan a race where everything else was secondary. What mattered was getting there and getting back, alive and healthy.    

    In contrast Scott’s twin goals were deeply and categorically in conflict. There’s certainly a long history of scientific studies on sailing ships. Darwin, for example, tagged along as a self-funded gentleman naturalist on the Beagle while the main mission was the charting the coastline of South America. But the Beagle wasn’t in a race, it’s a big ship, science was always a secondary mission, and they weren’t in the most inhospitable environment one can ever imagine.    

    Fred Wilson talks about the     [    power of focus using Steve Jobs    ](http://www.avc.com/a_vc/2013/08/focus.html)     as an example. He relates how Jobs “brought focus to the product line, and thus everything else” which translated into “we are going to make one computer for each quadrant and we are going to kill all of the other product lines".    

    Focus is the power behind ideas like Minimal Viable Products and time box scheduling. Amundsen was able to make every decision with a single mission in mind which lead to much better choices for the task at hand. Scott ended up with     [    big ball of mud    ](http://en.wikipedia.org/wiki/Big_ball_of_mud)     design.     

    **Use simple, proven technology**        . The strangest aspect of Scott’s plan was the use of a complex mix of unproven transportation technologies.    

    Amundsen’s’ plan was simple. Since they were already great skiers they used dog sledding teams the whole way to the pole. This makes sense. Dog sledding teams are a proven technology in that environment. To not use dogs was strange.    

    But Scott had bad experiences with dogs on previous trips so he didn’t use them. Instead Scott planned on using a motor-sledge, which is an early version of the snowmobile. At the time this was experimental technology and all three ended up breaking down. The real weakness here was not discovering why they had had such a bad experience with dogs when they clearly work so well for indigenous peoples. Not following up that discrepancy in the data was fatal.     

        ![](http://farm8.staticflickr.com/7332/9544767026_4b3a1ef402_m.jpg?__SQUARESPACE_CACHEVERSION=1376926197653)        The two remaining motor sledges failed relatively early in the main expedition because of repeated faults.        

    Ponies were tasked with a lot of the hauling. But the their small hooves made them bad haulers on snow they were very vulnerable to wet and cold. 9 ponies were lost before the journey began. And food for the ponies (and the motor-sledges) had to be stored on the ship and transported over the snow. Dogs on the other hand could eat off the land, feasting on the the seal and penguin meat found in Antarctica.    

    Man-hauling, that’s men pulling heavy sleds over the snow,  was to be used for part of the trip, but the loss of the ponies and the motor-sledges meant man-hauling had to be used to cover three quarters of the distance. Adding to their burden were rocks they collected along the way as part of their scientific mission. The problem was this was exhausting work and they weren’t eating enough calories. They didn’t know it, but they     [    were on a starvation diet    ](http://www.dailymail.co.uk/sciencetech/article-2166089/Captain-Scotts-lack-modern-knowledge-calorie-counting-doomed-expedition---mention-cocaine.html)    , eating 4000 calories a day when they needed between 6000 and 10, 000 calories a day.    

            ![](http://farm3.staticflickr.com/2877/9545248087_a2afe5017f_m.jpg?__SQUARESPACE_CACHEVERSION=1376924846815)        Robert F. Scott and two of his four companions set out for the South Pole pulling a sled. The expedition found Roald Amundsen had arrived a month early, and all five men perished on the route back.              
        

**Customize**. **Test. Repeat.** Amundsen was a meticulous planner, leaving nothing to chance. When their equipment wasn’t good enough, Amundsen made his own.  He designed his own goggles, skis, dog harnesses, and pemmican. And this led to a [    self-reliance every developer    ](http://www.artofmanliness.com/2012/04/22/what-the-race-to-the-south-pole-can-teach-you-about-how-to-achieve-your-goals/) will appreciate:

>     All of Amundsen’s equipment was field-tested at base camp and refined again and again. Amundsen saw all this tinkering and crafting as having two invaluable benefits: 1) the gear turned out much better than those mass-produced, and 2) having had a hand in making it, the men were much more confident in how the gear would perform out on the march.     

        ![](http://farm8.staticflickr.com/7387/9548007342_f0f860b4e7_m.jpg?__SQUARESPACE_CACHEVERSION=1376924648004)        Roald Amundsen's underground workroom where the team worked on tools and took shelter from the harsh weather conditions above them.        

    **Be ruthless**        . The handmaiden of Focus. Focus requires the ability to see only what’s important and do only what’s necessary.    

Walter Sullivan, in _The South Pole Fifty Years After_, illustrates this idea perfectly in a wonderful passage:

>     The moon was reached by expending a succession of rocket stages and then casting each aside; the Norwegians used the same strategy, sacrificing the weaker animals along the journey to feed the other animals and the men themselves.     

    **Be flexible**        .  Amundsen had planned to find the North Pole, but on hearing that two Americans had already made it to the North Pole, he immediately pivoted his plans and went for the prize in the South.     

**Be realistic**. For Scott [man-haulin](http://en.wikipedia.org/wiki/Manhauling)g was more than just a mode of transportation, it was a [romantic stance](http://en.wikipedia.org/wiki/Romanticism) to the world. I can almost hear Wagner playing in the background as Scott wrote:

> In my mind no journey ever made with dogs can approach the height of that fine conception which is realised when a party of men go forth to face hardships, dangers, and difficulties with their own unaided efforts…Surely in this case the conquest is more nobly and splendidly won.

With so much is on the line it's time to give up childish things.

    **Skills make all the difference**        . Amundsen recruited a team of experienced skiers with years of experience. Scott’s team were not experienced skiers and were not given the training they needed.    

    **Select the right team**        . I didn’t know this, but I’ve read that British expedition was largely composed of British gentlemen. Amundsen hired multi-talented craftsmen who were also outdoorsmen.    

    **Mistakes add up**        . Scott’s team may have been destined not to find the pole first, but their deaths were a great tragedy that turned on a series of confounding problems. The weather conditions were much colder than expected which on their return trip from the pole caused them not to make the mileages they needed to get to the next depot. A depot is a cache of food, fuel, and other supplies. When Scott, Wilson and Bowers died they were just 11 miles short of the One-Ton Depot. As a result of the failure of the ponies and the motor-sledges and a questionable depot laying plan, the last depot was placed farther away than it should have been. Otherwise they would have made it to the depot and survived. It wasn’t one thing that doomed them, but a series of unfortunate choices and even more unfortunate events.    

    **Hindsight is always 20 20**        . Nothing is ever so clear and obvious at the beginning, but fortune becomes fate once the outcome is known. Scott at the time made what seemed to him like sound decisions and other people agreed with him. And so it is with every project. These were brave men who did not give up their lives willingly or out of stupidity. The number of paths to a goal are few and it’s easy to misstep. A kind soul will keep this in mind when judging the actions of others.    

    As you read more and more of this fascinating story you’ll no doubt find lessons at every turn. I just hit some of the main points without going into elaborate detail.    

    At one level I should not be surprised there are so many lessons that can be drawn. In many ways programming really does have the structure of a     [    journey    ](http://theliterarylink.com/metaphors.html)     with     [    the goal    ](http://theliterarylink.com/metaphors.html)     of fulfilling a     [    quest    ](http://en.wikipedia.org/wiki/The_Hero_with_a_Thousand_Faces)    . That’s why it can be so much fun. And from experience we know there are best practices when tackling difficult objectives.    

    In retrospect are we surprised that the winners were a small, driven, well led, focussed, highly skilled and experienced team with a robust plan, plenty of resources, and a great ability to adapt to conditions on the ground?     

    In his book _The South Pole_, Amundsen concludes:    

>     I may say that this is the greatest factor – the way in which the expedition is equipped – the way in which every difficulty is foreseen, and precautions taken for meeting or avoiding it. Victory awaits him who has everything in order – luck, people call it. Defeat is certain for him who has neglected to take the necessary precautions in time; this is called bad luck.    

##     Related Articles    

*   [    Rare Pictures: Scott's South Pole Expedition, 100 Years Later    ](http://news.nationalgeographic.com/news/2012/01/pictures/120117-scott-south-pole-anniversary-hundred-years-science/)
*   [    The Gear That Won the Race to the South Pole    ](http://gizmodo.com/5550966/the-gear-that-won-the-race-to-the-south-pole)
*   [    One Man Won the Battle; the Other Won Hearts    ](http://www.nytimes.com/2010/05/29/arts/design/29race.html?_r=0)
*   [    The South Pole; an account of the Norwegian Antarctic expedition in the Fram, 1910-12    ](http://librivox.org/south-pole-by-roald-amundsen-2/)     (free, audio format)    
*   [    Leadership Lessons From the Shackleton Expedition    ](http://www.nytimes.com/2011/12/25/business/leadership-lessons-from-the-shackleton-expedition.html)     - Shackleton abandoned reaching the Pole when he realized his team would die if they didn’t turn back.    
*   [    The Worst Journey in the World    ](http://www.amazon.ca/Worst-Journey-World-Apsley-Cherry-Garrard/dp/1619491877)
*   [    Roald Amundsen 1872-1928    ](http://www.south-pole.com/p0000101.htm)
*   [    Captain Scott's lack of modern knowledge of calorie-counting may have doomed expedition    ](http://www.dailymail.co.uk/sciencetech/article-2166089/Captain-Scotts-lack-modern-knowledge-calorie-counting-doomed-expedition---mention-cocaine.html)        
*       [What the Race to the South Pole Can Teach You About How to Achieve Your Goals](http://www.artofmanliness.com/2012/04/22/what-the-race-to-the-south-pole-can-teach-you-about-how-to-achieve-your-goals/)    
*   [What a 100 Year-Old Race to the South Pole Teaches Us About Design](http://erlebacher.org/2010/06/17/what-a-race-to-the-south-pole-teaches-us-about-design/)

    