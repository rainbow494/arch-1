## [Designing for Resiliency will be so 2013](/blog/2012/12/31/designing-for-resiliency-will-be-so-2013.html)

    

    

<iframe align="right" width="250" height="141" src="http://www.youtube.com/embed/2S0k12uZR14" frameborder="0" allowfullscreen=""></iframe>

A big part of engineering for a quality experience is bringing in the [long tail](http://highscalability.com/blog/2012/3/12/google-taming-the-long-latency-tail-when-more-machines-equal.html). An improbable severe failure can ruin your experience of a site, even if your average experience is quite good. That's where building for [resilience](/blog/2012/12/3/resiliency-is-the-new-normal-a-deep-look-at-what-it-means-an.html) comes in. Resiliency used to be outside the realm of possibility for the common system. It was simply too complex and too expensive.

An evolution has been underway, making 2013 possibly the first time resiliency is truly on the table as a standard part of system architectures. We are getting the clouds, we are getting the tools, and prices are almost low enough.

Even Netflix, [real leaders](http://techblog.netflix.com/2012/11/hystrix.html) in the resiliency architecture game, took some heat for relying completely on Amazon's ELB and not having a backup load balancing system, leading to a prolonged [Christmas Eve failure](http://aws.amazon.com/message/680587/). Adrian Cockcroft, Cloud Architect at Netflix, [said they've investigated](https://twitter.com/adrianco/status/283364734907060224) creating their own load balancing service, but that "we try not to invest in undifferentiated heavy lifting."

So resiliency is still not part of the standard package. There's an ROI calculation that has to be made. Yet the path Netflix would have to take in creating a hybrid architecture is fairly clear, Netflix prefers to concentrate on features rather than long tail events. That's a big difference. At one time designing for resiliency would have been unthinkable, now it's becoming a choice. 

A good New Year's resolution might be to learn more about resilience. It's a new way of thinking compared to straightforward high availability. It's a full stack, full team, full system, environment centric mode of thought.

Fortunately, [Dr. Richard Cook](http://velocityconf.com/velocity2012/public/schedule/speaker/135765), Professor of Healthcare Systems Safety and Chairman of the Department of Patient Safety at the Kungliga Techniska Hogskolan, has been thinking about resilience for a long time. And he gave a fascinating talk: [How Complex Systems Fail](http://www.youtube.com/watch?v=2S0k12uZR14) on resilience, that is just detailed enough to be practical and high level enough to inspire new directions.

Here's a gloss of the essentials from his talk:

##     Why Don’t Systems Fail More Often?    

    The normal world is not well behaved. The real surprise is not that there are so many accidents but there are so few. Is this because of or in spite of our system designs? We all  have had the sense of barely escaping our just getting by. It seems like we should have crashes all the time. Why is that? What does that mean about IT design implementation and ops?    

##     Summary of 25 years of Research    

*   **Story**. A hospital replaced all its pumps at once. A year later 20% of the pumps failed at the same time. The cause was a forced software upgrade set one year into the future that was required to happen or the unit would simply not accept new commands. Each individual step seemed to work and seemed fine at the time, but nobody saw the one year time bomb ticking.
*   **The real world often produces surprises**, things you haven’t seen before. Sometimes these are an existential threat, that compromise your core missions.
*   **Demand and op temp varies**. It’s not constantly being beat on. Things aren’t always going wrong. It’s a real variable world. You can’t predict it very well.
*   **Stuff don’t work as advertised**. There’s lots of stuff in our systems that we don’t try out for many years and then when we do try them they don’t work as expected.
*   **Novel conditions are common**. Things that you haven’t seen before do occur relatively frequently.
*   **There’s lots of adapting and tailoring**. Systems aren’t working out of the box. They have to be tweaked and tuned and kissed and cuddled. Services are being started and stopped as parameters are being changed to get the system to work in the way you want.
*   **Unexpected uses**. People start using your system for things you didn’t expect, like sending very large files over email.
*   **Continuous change of technology and people**. The world you live in is anything but static.
*   **Coping with shallow and deep conflicts**. Shallow conflicts: can we make a little more, can this run a little better, is this guy giving me hard time on the phone. Deep conflicts: we have trade offs. Are we going to take the system down to fix it or fix it on the fly?

##     System as Imagined vs System as Found    

*   **System as imagined**. Few people have contact with systems as found. They are imagined in state diagrams and layouts and other diagrams. It’s how we make stuff. They are static and deterministic. You rarely see pictures of people doing work, sitting in front of screens and making the system run. It’s encountered during design and development and reviews of outcomes when a system fails. Archetype is state transition diagram or fault tree analysis.
*   **Systems as found**. They are dynamic and stochastic. It’s constantly changing. It’s only predictable in some statistical way. Performance can’t be deterministically defined. We are always doing some kind of maintenance. Do not touch any of these wires. It’s what we encounter during implementation, operations, maintenance, and recovery from faults. They are very hard to draw these things. One way is using a FRAM (functional resonance analysis method) diagram.

##     What are people doing in these As Found systems? What should operations look like?    

    Resilience is the combination in systems of these four activities:    

*   **Monitoring**. People look at the system to see what’s going on.
*   **Responding**. Not in a reacting sense, but understanding what is going on in the system to figure out where it’s going and trying to make changes to deal with that.
*   **Adapting**. Make the system work differently to get it to behave in a better way.
*   **Learning**. Key component. It’s happening in communities of operators, shift personal etc, of people that we don’t have contact with.

    These are terms of what we are trying to describe as resilience.    

##     Reliability is made out of these things at design time:    

*       Stiff boundaries    
*       Layers, formalisms    
*       Defence in depth    
*       Redundancy    
*       Interference protection, security, abstraction, hiding of details    
*       Assurance    
*       Accountability    

##     What we really want is resilience:    

*       Withstand transients    
*       Recovery swiftly & smoothly from failures    
*       Prioritize to serve high level goals when the system is changing and we have to sacrifice something    
*       Recognize and respond to abnormal situations, including situations that were never considered at design time    
*       Adapt to change. Find someway to make system designed in the mindset of 4-7 years ago work for the next 3-5 years while we are building the next systems that will be obsolete when we install them.    

##     How do we design for resilience?    

*   **We have the two quite separate worlds as Systems as Imagined as Systems as Found**. They look at different things and they work on different sorts of principles. Can you bring them together? Can you can make an As Engineered world that includes the quality of resilience? Is it possible to engineer systems ahead of time so that it is possible in operational time to have the resilience you want them to have?
*   **Support for continuous maintenance**. Maintenance is not periodic. They are constantly being maintained in some way. That includes things like software, hardware, personnel,  physical plant. Maintenance needs to become part of the design rather than something added on later.
*   **Reveal the controls of the system to the operators**. When it’s 3AM in the morning the only people who are going to fix the problem are the operators and control people sitting in front of the screens that actually control the system. It’s essential that we trust operators with the controls to our systems. This is subversive in that typical designs are built around the idea of making it impossible for people to do things as a form of protection. If resilience is the goal then we have to develop some way of trusting people because we need them to act in situations where they are the only people available to act. Show the lift points. Almost all heavy machinery has markers to show where to lift it. We do that because we know the system is going to be moved. We have to start showing lift points in the system itself. Where can I pick it up and move it? How could I take it and move it outside the current system that I have and replace it with something else? We don’t do this well with code, structures, or organizations.
*   **Support mental simulation**. People need clear ideas about how the system is configured and running. You need to have operators mentally simulate if they take some action. This requires deep knowledge of the system and its status. Designers have to present that information to operators so they can see what’s going on. You need to think about the kind of situations operators are going to confront and the kinds of work they are going to do and you have  to support the mental simulation they are going to do as they are trying to figure out how to make the system work or recover.
*   **Open your objects and open your methods**. Hiding everything about the details of your system in levels of abstraction has run its course now. That idea that we can make these black box devices that we only know the shell of and have no knowledge of the internal workings of is a deep mistake. It turns out to be able to reason about how the system is working we have to have knowledge of what’s inside the black box.
*   **Deep-six the don’t touch me’s**. There are no parts of the systems that are closed.
*   **Empower operator learning**.

##     What’s the resilience agenda?    

*       Operations are competent to hold the keys to the systems we build. You should be willing to hand over the keys to the operators.    
*       Make resilience engineering the first priority of design for next gen systems.    
*       Commit resources to discovering, understanding and supporting resilience through the system life-cycle.    

##     Final Thoughts    

[DevOps](http://en.wikipedia.org/wiki/DevOps) for sometime has been leading in the direction of unifying the System as Imagined with the System as Found, so disparate communities aren't formed around a system. Learning is being pushed up to the developers and back down through the code so the System as Found can become wedded to the System as Imagined through the entire stack.

    But what Dr. Cook asks for is something developers can’t deliver: such a clear understanding of a complex system that you can hold it in the palm of your hand, turn it, twist it, interrogate it, and make it dance to your tune. Complex systems can only be built incrementally, which means there is only ever an incremental understanding of how the whole thing works, which means it can never be opened to the degree he wishes. A system will always be in large part subconscious, just like how in the the human brain the conscious mind is only the     [    smallest window    ](http://www.brainsciencepodcast.com/bsp/review-of-self-comes-to-mind-by-antonio-damasio-bsp-90.html)     on a vast subconscious mind.    

##     Related Articles    

*       [CTLAB](http://www.ctlab.com/) /     [How Complex Systems Fail](http://www.ctlab.org/documents/How%20Complex%20Systems%20Fail.pdf)
*   [    How Complex Systems Fail: A WebOps Perspective    ](http://www.kitchensoap.com/2009/11/12/how-complex-systems-fail-a-webops-perspective)     by  John Allspaw    
*       [Netflix: Run Consistency Checkers All The Time To Fixup Transactions](http://highscalability.com/blog/2011/4/6/netflix-run-consistency-checkers-all-the-time-to-fixup-trans.html)    
*   [Continuous Deployment at Etsy](http://www.slideshare.net/chaddickerson/continuous-deployment-at-etsy-sxsw-lean-startup-track)
*       [Taleb on Black Swans, Fragility, and Mistakes](http://www.econtalk.org/archives/2010/05/taleb_on_black_1.html)    
*   [    Introducing Hystrix for Resilience Engineering    ](http://techblog.netflix.com/2012/11/hystrix.html)
*       [Startups Are Creating A New System Of The World For IT](http://highscalability.com/blog/2012/5/7/startups-are-creating-a-new-system-of-the-world-for-it.html)    

    