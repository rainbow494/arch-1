## [7 Scaling Strategies Facebook Used to Grow to 500 Million Users](/blog/2010/8/2/7-scaling-strategies-facebook-used-to-grow-to-500-million-us.html)

    

    

![](http://farm5.static.flickr.com/4127/4847617633_12bd62e2b4_m.jpg)

    Robert Johnson,a director of engineering at Facebook, celebrated Facebook's monumental achievement of reaching 500 million users by sharing the [scaling principles that helped](http://www.facebook.com/note.php?note_id=409881258919) reach that milestone. In case you weren't suitably impressed by the 500 million user number, Robert ratchets up the numbers game with these impressive figures:    

    

*   1 million users per engineer
*   500 million active users
*   100 billion hits per day
*   50 billion photos
*   2 trillion objects cached, with hundreds of millions of requests per second
*   130TB of logs every day

    

How did Facebook get to this point?

    

1.  **People Matter Most**. It's people who build and run systems. The best tools for scaling are an engineering and operations teams that can handle anything.
2.  **Scale Horizontally**. Handling exponentially growing traffic requires spreading load arbitrarily across many machines. Using different databases for tables like accounts and profiles only doubles capacity. This approach hurts efficiency, but efficiency is a separate effort from scaling, efficiency by itself doesn't substantially impact scaling.
3.  **Move Fast**. At every level of scale there are surprises. Surprises are quickly dealt with using a highly qualified cross disciplinary team that is flexible and skilled enough to deal with anything that comes up. Flexibility is more important than any individual technical decision. By moving fast Facebook is also able to try more options and figure out which ones work best.
4.  **Change Incrementally**. Making small changes and measuring the result is the key to moving fast. Big things are broken up into distinct parts, changes are not batched. Changes can be rolled out on a few machines to a few users. New systems can be built in parallel to old systems with traffic slowly moved over to the new system while results are being measured. Overall system stability is increased by incremental change because you know sooner if a particular strategy is working. It's easier to figure out where things go wrong when dealing with smaller increments.
5.  **Measure Everything**. Production is where the really useful data comes from. Measure both system and application level statistic to know what's happening. Checkout what's happening in the 95th or 99th percentile as averages hide important issues. 
6.  **Small, Independent Teams**. Small teams allow work to be done efficiently, quickly, and carefully. Only three people work on photos, for example, the largest photo site on the internet. 
7.  **Control and Responsibility**. Responsibility requires control. If a team is responsible for something they must control it. For example, Facebook pushes code into production everyday. The person who wrote the code is there to fix anything that goes wrong. If the responsibility of pushing and wring code are split, then the code writer doesn't feel the effect of code that breaks the system. Robert puts it wonderfully: _The best way we know of to get great software to these 500 million people is to have a person who understands the importance of what they're doing make a good decision about something they understand and control._

    

These principles are not really new, but I think when you see them all laid out together like this it's easy to see how they all work together to make a self-reinforcing virtuous circle. You can't move fast unless you have small teams who have control and responsibility. You can't know how your changes are working unless you get those changes into production and measure results. You can't move code into production unless people feel responsible for moving out working code. You can't handle the scale unless you figure out how to scale horizontally, move fast, measure everything, etc. and that all comes down to good people. 

Will these principles be enough to grow the next 500 million users? Because of principle number one, my guess is yes. The world will change, there will be tremendous unforeseen challenges in the future, but good people given the right environment will learn and adapt. What will be the challenge, a challenge that Facebook has met so far, is keeping true to the principles that have got them here, and avoiding the organizational rot the infects so many organizations once they reach a size and complexity tipping point.

## Related Articles

*   [The Four Meta Secrets Of Scaling At Facebook](http://highscalability.com/blog/2010/6/10/the-four-meta-secrets-of-scaling-at-facebook.html)
*   [Why Are Facebook, Digg, And Twitter So Hard To Scale?](http://highscalability.com/blog/2009/10/13/why-are-facebook-digg-and-twitter-so-hard-to-scale.html)
*   [Facebook Related Articles on HighScalability](http://highscalability.com/display/Search?searchQuery=facebook)

    