## [Scaling Mailbox - From 0 to One Million Users in 6 Weeks and 100 Million Messages Per Day](/blog/2013/6/18/scaling-mailbox-from-0-to-one-million-users-in-6-weeks-and-1.html)

    

    

![](http://farm4.staticflickr.com/3767/9072919945_5e8fe045d3.jpg)

You know your product is doing well when most of your [early blog posts](http://www.mailboxapp.com/blog/) deal with the status of the waiting list of hundreds of thousands of users eagerly waiting to download your product. That's the enviable position [Mailbox](http://www.mailboxapp.com/), a free mobile email management app, found themselves early in their release cycle. 

Hasn't email been done already? Apparently not. Mailbox scaled to one million users in a paltry six weeks with a team of [about 14 people](http://www.mailboxapp.com/blog/?p=1#to-grow-even-faster-mailbox-is-joining-dropbox). As of April they were delivering over [100 million messages per day](http://www.mailboxapp.com/blog/?p=1#mailbox-now-available-without-the-wait).

How did they do it? Mailbox engineering lead, [Sean Beausoleil](https://twitter.com/SeanBeaus), gave an [informative interview on readwrite.com](http://readwrite.com/2013/06/05/from-0-to-1-million-users-in-six-weeks-how-mailbox-planned-for-scale) on how Mailbox planned to scale... 

*   **Gather signals early**. A pre-release launch video helped generate interest, but it also allowed them to gauge early interest before even releasing. From the overwhelming response they knew they would need to have to scale and scale quickly. 
*   **Have something unique**. The average person might not think a mailbox app would be a fertile product space. There's a lot of competition, but most of it is lame. Mailbox had a lot of innovative ideas with their to-do list based approach to email and efficient UI swiping actions. That helped generate a tremendous amount of early buzz.
*   **Know the nature of your product**. Email is business critical and is resource heavy so they planned on having to scale early. There's wasn't a let's get it out the door and then plan to scale later attitude at all.
*   **Target**. When Mailbox released it only worked with Gmail and on the iPhone, clearly targeting a large tech savvy market that would be open to Mailbox's new inbox approach.
*   **Extend**. Mailbox wasn't a new mail service. It was a better interface to Gmail, an existing high quality email system. Kudos to Google for having an API that makes this possible. Users could adopt Mailbox without fear that their email would be lost.
*   **Modulate and iterate**. They followed a clear best practice in system design by designing modular components and iterating on the components as needed.
*   **Prototype**. A test system using IMAP was built to identify bottlenecks at production loads. These tests found problems that would be difficult to fix in production. This is a mature and important step often skipped by startups. 
*   **Keep number of technologies to a minimum**. They didn't want to become expert on a lot of different systems, they wanted to focus on the development of their product. 
*   **Rate limit new customers with a reservation system**. Early on Mailbox was probably better known for their reservation system than the product. It helped stimulate demand through perceived scarcity while also gating customers so load could be added slowly, in a controlled manner. The priority was the experience of the people using the system rather than acquiring new users. Genius all around. 
*   **Crazy devotion**. Developers worked continually to fix problems and improve the system. Focus on this early phase of development. It can be the most productive of the entire product life cycle.
*   **Pay attention to what users do**. Core infrastructure was tweaked, sharded, or removed in response to user usage patterns. 
*   **Things will inevitably fail and need to be fixed**. Even with their testing problems occurred in production. That's just something you can expect when release anything at scale. Iterate and keep on iterating. The system will get better as you learn more about your system, data, and users.
*   **Play toss the tech**. If you have legacy technology or you made stack or process choices that don't work anymore then replace it with something that will work. Spend time making new technology selections. Then run with them. 
*   **Leverage previous products**. Experience and code used on creating an iPhone To-Do app was directly used to bootstrap Mailbox. This gave them the experience needed to make an app that felt fast from the start.
*   **Cloud was cost-effective and efficient**. A good match for a startup with limited resources and tight deadlines. Servers [in the cloud](http://www.mailboxapp.com/blog/?p=2#were-ramping-up) "do things like send push notifications, download email as fast as possible, and handle 'snoozed' messages."
*   **It's the journey**. The phase where a team is building, designing, and fixing a product is essential learning that pays off through the lifecycle of the product. It can't really be short circuited or sided stepped with stack choices. It's part of how a good team and product are formed into one.  
*   [Be bought by Dropbox](http://www.mailboxapp.com/blog/?p=1#mailbox-now-available-without-the-wait). After about a month after the product release Mailbox was bought by Dropbox. The idea is Dropbox will help them grow bigger and faster.
*   **Communicate**. If you look [Mailbox's blog](http://www.mailboxapp.com/blog) they do a very good job of explaining what's going on in enough detail that customers feel comforted instead of confused.

Unfortunately we don't get a lot of technical details. It's easy to imagine however that the architecture for Mailbox is highly parallel and sharded as individual mailboxes can easily be processed in parallel in a shared nothing manner. Even without the technical details it's a fascinating portrait of how a successful cloud-mobile application is born.

## Related Articles

*   [Scaling Pinterest - From 0 To 10s Of Billions Of Page Views A Month In Two Years](http://highscalability.com/blog/2013/4/15/scaling-pinterest-from-0-to-10s-of-billions-of-page-views-a.html)
*   [How Mailbox Scaled To One Million Users In Six Weeks](http://readwrite.com/2013/06/05/from-0-to-1-million-users-in-six-weeks-how-mailbox-planned-for-scale) 

    