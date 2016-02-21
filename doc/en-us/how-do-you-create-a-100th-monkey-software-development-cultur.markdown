## [How do you create a 100th Monkey software development culture?](/blog/2013/7/17/how-do-you-create-a-100th-monkey-software-development-cultur.html)

    

    

![](http://farm8.staticflickr.com/7423/9303214200_17f4406e9d_m.jpg)

    Someone reading my now ancient [C++ coding standard](http://www.possibility.com/Cpp/CppCodingStandard.html) recommendation for using doxygen to automatically generate documentation from source code, asked a great question:    

> I've often considered using doxygen, I always ask myself - is this really useful? Would I use it if I were new to a project? Would programmers working on the project use it?

I'll rephrase their question to more conveniently express a point I've thought a lot about: **Why do companies put so little effort into automating their own development process to make development easier?**

It's like the hair stylist whose own hair looks like someone cut it using a late night infomercial vacuum cleaner attachment. Or it's like the interior decorator whose own house looks like a monk's cell.

Software organizations rarely build software to make developing software easier. Why is that?

    Because there are three ways changes are made in an organization:    

*       **Top Dog** - Someone so high up in the org chart you would need a telescope to see him decided because his brother-in-law sells this very expensive Whatever system, you should use it too. Of course it will never work, even after spending all those plump and tasty consulting dollars.    

*       **Drunken Clown** - Someone accidentally did something some way once a while back and that's just the way it works now. I think of this whenever I need to add a copyright to an html page. Instead of a clear "Add Copyright" directive I instead have to create a footer and then I need to insert a "special" character into the footer to see the copyright. What's up with that? I've always wanted to ask the original perp who wrote this feature just what were they were thinking? How am I ever supposed to remember copyright  == special character? But it doesn't matter anymore, that's just how it works and people can't even imagine it working a different way. It just seems the natural and right way now even though it makes no sense at all!    

*       **100th Monkey** - Someone who had a problem took on the responsibility on their own to make something useful. What they built naturally spreads because other people find it useful too. It's usually not the best of all possible systems, but it gets the job done. The [100th Monkey approach](http://en.wikipedia.org/wiki/Hundredth_monkey_effect) (a phenomenon in which a behavior or thought spreads rapidly from one group to all related groups once a critical number of initiates is reached) to improvement is often actively discouraged. People don't have any "extra" time because they are fully loaded with work backlog. If you can't show continual progress on your assigned tasks then a manager somewhere will get itchy. All that wood must be behind an arrow or it's useless.    

And of course you can't get time to work on anything extra because where is the ROI for your customers? If bathroom breaks weren't physically required they would outlawed because there's no ROI in it.

Let's say you do get time to work on that extra problem. You probably won't get enough time to do a good job so you end up with one hack built on top of another hack because people can't assign value to all the infrastructure that often makes the critical difference between project success or failure. 

So someone makes a quick little hack. But the next person or group that comes along won't like it because it doesn't do X and the person who did the original hack doesn't have time to make any changes. There's no ROI in it for them. 

On learning the tool they may want to use won't be improved the new group has a choice: improve the existing tool or make something new. 

Nine times out of ten the best strategy is to make something new because that is what will solve their problem faster. The existing tool isn't supported,it's not well documented, it's a long ways from being general enough to solve new problems, so why take the hit of trying to fix it? There's no ROI in it.

An organization can end up with dozens of similar systems and none of them is ever quite good enough to win over enough hearts and minds needed to dominate. 

Even worse and more likely is that people in an organization don't even try anymore because they know any efforts to make the development process better won't work out in the end. Why bother? There's no ROI in it.

I've seen this same cycle over and over and over again. 

They say when bears are thriving in an area it means the ecosystem in that area is pretty healthy because bears are at the top of the food chain. If bears are doing well it means the food chain below them must doing pretty well too. We can say the same about many infrastructure components supporting development.

Automatically generated documentation is one of those things that usually doesn't get done because it takes too long to do right and there's no ROI in it. 

But automated documentation generation is not so much about the technology needed to make it happen, it's more about the culture needed to maintain it and make it useful. It's your culture more than anything that will make a project successful. 

Automated documentation assumes people wrote the documentation to begin with. It assumes a tool was selected and integrated into the build system and more over that there even is a build system. It assumes people maintain it. It indicates that someone thought about what they were doing enough that they could explain it coherently to someone else. It means that when someone wants to know if a capability is available then they can do a search of the documentation and hopefully find what they are looking for.

And just maybe they will feel the code is good enough to extend and use rather than rewrite. It means you can have a culture where people are building on each others work rather than replacing it. That's the secret to hyperproductivity.

You can make a similar case about many other development practices: unit tests, source code control, bug tracking, training, having a methodology, automated builds, automated regression tests, continuous deployment, monitoring, auto-provisioning, etc etc. These practices are often ignored because the ROI isn't direct enough. But if these practices are in place it indicates a pretty healthy development ecosystem in which people are probably pretty productive.

Another tool that doesn't get enough use is a company or development wide wiki (or equivalent). If they are in place the productivity gains are huge. And again the active use of a wiki indicates a healthy development ecosystem.

No matter how useful wikis are they are hard to get adopted. A while ago I created a number of wiki pages describing how I have I worked to get wikis adopted at several companies. I now think the approach described in [Getting Your Wiki Adopted](http://www.possibility.com/wiki/index.php?title=GettingYourWikiAdopted) doesn't just apply to wikis, it applies to most any tool or process you want to add without 100% corporate buy-in. It's a sort of 100th Monkey manifesto. Give it a read and consider how it might be transmuted to apply to your project and corporate culture.

Let's take the automated documentation system as an example of how to apply the ideas. I'm not fully fleshing the idea out, I am just giving a few thoughts on each item.

*   **Getting Automated Documentation System Adopted is Tough**. Yep, it will be. Anything new will take continual effort to make work. 
*   **Have a Champion**. Someone needs to have enough passion to see the process through from beginning to end.
*   **Remove Objections**. For people to adopt a Automated Documentation System system you need to remove every possible objection they could have towards using it.  

    *   Bake it into your build system.
    *   Make it a make target and easy to set up.
    *   Come up with courses and good quick online doc for telling people how to create the doc.
    *   Make the doc attractive, useful, and full linked.
*   **Create Content**
    *   You must document enough code so that people see that it has value.
    *   Make sure all the code you create or touch has doc.
*   **Enmesh the Automated Document Generation In Company Processes**
    *   Make the doc searchable so people will find themselves turning it when they have a question
    *   Make it required. Add verification steps during the nightly build. Put doc in as a code review item.
    *   When someone has a question you have to be able to refer them to the doc most of the time. If you can't, why not? If it's not usable then why should anyone use it?
    *   Tie it into the build system so every nightly and release build has a link to the doc for the build. 
*   **Evangelize**
    *   When you go a class and it doesn't have doc ask the author to put doc in.
    *   When the doc sux ask the author to improve it.
    *   Find people to help you.
*   **Don't Give Up**. Hang in there. It's a bootstrapping process that has to start somewhere.
*   **Just Do It! Don't Wait For a Budget**
    *   Pick a tool and add it to make and the build yourself.
    *   Evangelize with examples, email, meetings, and success stories.
*   **Have a Transition Plan**
    *   Make it easy for your tools group to adopt any of the changes you have made.
    *   Try and make changes in a style consistent with how things are done currently so the changes will be less objectionable.

So don't give up. Next time you want to make a change and nobody else seems to care then go 100th Monkey and see what happens. You may be able do the impossible: change an organization for the better.

## Related Articles

*   [On Reddit](http://www.reddit.com/r/programming/comments/1ii27u/how_do_you_create_a_100th_monkey_software/)

    