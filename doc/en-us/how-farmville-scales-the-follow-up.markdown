## [How FarmVille Scales - The Follow-up](/blog/2010/3/10/how-farmville-scales-the-follow-up.html)

    

    

![](http://farm5.static.flickr.com/4049/4335031559_fa690c1f81_m.jpg)

Several readers had follow-up questions in response to [How FarmVille Scales to Harvest 75 Million Players a Month](/blog/2010/2/8/how-farmville-scales-to-harvest-75-million-players-a-month.html). Here are Luke's response to those questions (and a few of mine).

###     How does social networking makes things easier or harder?  
    

    The primary interesting aspect of social networking games is how you wind up with a graph of connected users who need to be access each other's data on a frequent basis. This makes the overall dataset difficult if not impossible to partition.    

###     What are examples of the Facebook calls you try to avoid and how they impact game play?    

    We can make a call for facebook friend data to retrieve information about your friends playing the game. Normally, we show a friend ladder at the bottom of the game that shows friend information, including name and facebook photo.     

###     Can you say where your cache is, what form it takes, and how much cached there is? Do you have a peering relationship with Facebook, as one might expect at that bandwidth?    

    We use memcache as our caching technology. Can't comment on the peering relationship.  
    

###     What is the impact in game play when functionality is disabled in response to load?    

    What users will see is that some part of the application doesn't work as it normally does. We effectively have an ordering of which things are less important to gameplay and turn those things off first. For example, on our neighbors page and in a friend ladder at the bottom of the flash application, you can see a list of your friend playing the game and their game stats. In some cases, when we have high load, we turn that off. It saves us some work on the backend and has a relatively small effect on user experience.    

###     How do you integrate with Facebook? Do you try and stay out of there backend as much as possible? And advice on working with Facebook?    

    We integrate with Facebook through the REST api. Since we are such a large application builder on Facebook, we have a substantial amount of high level communication back and forth on technical matters.    

###     Your degradable approach is very interesting. It sounds like you can play most of the game in the client without talking to the backend for long periods of time. I take it flows from the nature of the application where farms are relatively isolated from each other? Is there any attempt to clump farms together as some other multiplayer games do?    

    Yes, one of the benefits of having an interactive client is that we have a bit of isolation between server latency and observed client latency. We do verify each action performed in the game; however, we do it asynchronously and queue the transactions on the client.    

###     Do you use MySQL? If you use SQL how is it used?    

    We use MySql.    

###     What "P" do you use in LAMP? Python etc.    

     We use PHP.    

###     How do you talk to the backend? Is it Request-response, XHR, long-polling, Flash XML sockets, or "COMET"?    

    We use a standard HTTP request/response protocol called AMF. The AMF transactions happen asynchronously from the client and if the server sees something it doesn't think the client should be sending, it returns to the client an "Out of Sync" message which tells the client it is in an invalid state and the client reloads itself.    

###     Do you run a 200 (or some other count) node Vertica cluster?    

    We do not do this for FarmVille    

###     Do you run in a cloud or have dedicated servers? Do you use EC2?    

    We do run in a cloud. The key characteristics here are that we use commodity, virtualized hardware. Thus, we greatly cut down the time from when we decide we want additional capacity to when the hardware is available.    

###     Could you please give examples of service degradation?    

    An example inside of FarmVille is where we have a friend ladder inside the flash at the bottom of the page. Normally, we query facebook for the name and profile picture and our own backend for the game stats and avatar data.    

    This is a high engagement piece of the application but lower in priority than users doing farming actions. Thus, if our backend is having performance problems, we can turn that off and the friend ladder will only show facebook name and profile picture. Likewise, if facebook is having performance problems, we can turn it off and the friend ladder will not show up.    

    The End  
    

    