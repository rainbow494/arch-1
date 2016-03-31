## [The Four Meta Secrets of Scaling at Facebook](/blog/2010/6/10/the-four-meta-secrets-of-scaling-at-facebook.html)

    

    

![](http://creative.ak.fbcdn.net/ads3/creative/pressroom/jpg/b_1234209334_facebook_logo.jpg)

Aditya Agarwal, Director of Engineering at Facebook, gave an excellent [Scale at Facebook](http://www.infoq.com/presentations/Scale-at-Facebook) talk that covers their architecture, but the talk is really more about how to scale an organization by preserving the best parts of its culture. The key take home of the talk is: 

> You can get the code right, you can get the products right, but you need to get the culture right first. If you don't get the culture right then your company won't scale.

This leads into the four meta secrets of scaling at Facebook:

1.  Scaling takes Iteration
2.  Don't Over Design
3.  Choose the right tool for the job, but realize that your choice comes with overhead.
4.  Get the culture right. Move Fast - break things. Huge Impact - small teams. Be bold - innovate.

## Some Background 

*   **Facebook is big**: 400 million active users; users spend an average of 20 minutes a day; 5 billion pieces of content (status updates, comments, likes, photo uploads, video uploads, chat messages, inbox messages, group events, fan pages, friend connections) are shared on Facebook every week; 3 billion photos uploaded each month; 250 applications that have more than 1 million users a month; 80,000 connect applications, 500,000 applications; 2 million developers; 150 million memcache operations per second; thousands of memcache servers with 10s of terabytes of memory; 300 developers.
*   **Facebook is difficult to scale**. Each type of content has its own access pattern which makes scaling difficult. Everyone uses Facebook in a different way. Every experience is unique. Most websites have a scaling story of adding more servers because the data can be partitioned. Scale horizontally. In Facebook users can't be partitioned because users can join any network. It's a global network that tries to capture everyone in the world, it allows anyone to friend anyone, and can represent any relationship between users. Every new user can access any other user's data which means there's no way to partition users by geography, or any other metric. Every user on Facebook has 130 friends, but there's no cut that allows you to partition the data so that access is only in that cluster.
*   **Overall architecture has 4 main components**: Load Balancer, Web Servers (written in PHP), Services (fast, complicated, search, ad, scribe), Memcached (fast, simple), Databases (slow, persistent).

## The Four Scaling Meta Secrets         

**1\. Scaling takes Iteration**. Solutions of often work in the beginning, but you'll have to modify them as you go on. What works in year one may not work later. PHP, for example, is simple to use at first, but is not a good choice when you have 10s of thousands of web servers.

Another example is photos. They currently serve 1.2 million photos a second. The first generation was build it the easy way. Don't worry about scaling that much. Focus on getting the functionality right. Uploader stored the file in NFS and the meta-data was stored in MySQL. It worked for the first 3 months and caused a lot of sleepless nights. He would still do it the same way today. Time to market was the biggest competitive advantage they had. Having the feature was more important than making sure it was a fully thought out scalable solution.

The second phase was optimization. Are there different access patterns that can be optimized for? It turns out smaller images are access more frequently so those became cached. They also started using a CDN. NFS was not designed to store 80 billion small files, so all meta-data wouldn't fit in memory, so lookups would take 2 or 3 disk IOs which was slow.

The third generation is an overlay system that creates a file that is a blob stored in the file system. Images are stored in the blob and you know the offset of the photo in the blob. One IO per photo.

**2\. Don't Over Design**. Just use what you need to use as you scale your system out. Figure out where you need to iterate on a solution, optimize something, or completely build a part of the stack yourself. They spent a lot of time trying to optimize PHP, they ended up writing HipHop, a code transformer to convert PHP into C++. It generated a massive amount of memory and CPU savings. You don't have to do this on day one, but you may have to. Focus on the product first before you write an entire new language.

**3\. Choose the right tool for the job, but realize that your choice comes with overhead**. If you really need to use Python then go ahead and do so, we'll try to help you succeed. Realize with that choice there is overhead, usually across deployment, monitoring, ops, and so on. 

If you choose to use a services architecture you'll have to build most of the backend yourself and that often takes quite a bit of time. With the LAMP stack you get a lot for free. Once you move away for the LAMP stack how do things like service  configuration and monitoring is up to you. As you go deeper into the services approach you have to reinvent the wheel.

**4\. Get the culture right**. **Move Fast** - break things. **Huge Impact** - small teams. **Be bold** - innovate. Build an environment internally which promotes building the right thing first and then fixing as needed, not worrying about innovating, not worrying about breaking things,  thinking big, thinking what is the next thing you need to build after the building the first thing. You can get the code right, you can get the products right, but you need to get the culture right first. If you don't get the culture right then your company won't scale.

1.  **Move fast**. Get to market first. It's OK if you break things. For example, they now run their entire web tier on HipHop which was developed by three people. Very risky, it brings the site down regularly (out of memory, infinite loops), but there's a big payoff as they figure out how to make it work. The alternative would be to have 3 month testing process for every change, it slows down everything. This is a particular instance, but the most important thing is the culture, getting people to believe that the most important thing is how quickly they can move.
2.  **Huge Impact**. Small teams can do great things. Search, photos, chat, HipHop were all small teams doing major features. Get the right set of people, empower them, and let them work. 
3.  **Be bold**. Don't be afraid of failure. It's OK to try things and fail. It's crazy to say let's make a new JVM, but the payoff is amazing when it works.

There are no product owners at Facebook. Everyone owns the product. Give people ownership of what they work on. If you give ownership to one person then the chances are nobody else will contribute to pushing it to the next level. Ideas come from users and people internally. If you can't push responsibility down and you isolate the number of people who feel they are real owners, then the only people you'll be able to motivate are the people who think they are the real owners. So instead why not distribute that entire responsibility? 

Isolate the part of the culture that you value and want to preserve. It doesn't happen automatically. Facebook organizes hackathons, the point of which is to show new engineers that if they come in at 8AM they can get a new feature up on the site in 12 hours. Move fast isn't just a platitude, a company has to come up with ways to make people feel it's a reality.

## **Related Articles**

*   [Should Startups Worry about Their Company Culture?](.readwriteweb.com/start/2010/06/should-startups-worry-about-th.php) by [Audrey Watters](http://www.readwriteweb.com/start/author/audrey-watters.php).
*   [Facebook Related Articles](http://highscalability.com/blog/category/facebook)

    