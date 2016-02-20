## [Ask HighScalability: Facing scaling issues with news feeds on Redis. Any advice?](/blog/2012/8/13/ask-highscalability-facing-scaling-issues-with-news-feeds-on.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Monday, August 13, 2012 at 2:06PM</span></div>

<div class="body">

<span>We just released a social section to our iOS app several days ago and we are already facing scaling issues with the users' news feeds.</span>  

<span>We're basically using a Fan-out-on-write (push) model for the users' news feeds (posts of people and topics they follow) and we're using Redis for this (backend is Rails on Heroku).  However, our current 60,000 news feeds is ballooning our Redis store to almost 1GB in a just a few days (it's growing way too fast for our budget). Currently we're storing the entire news feed for the user (post id, post text, author, icon url, etc) and we cap the entries to 300 per feed.</span>  

<span>I'm wondering if we need to just store the post IDs of each user feed in Redis and then store the rest of the post information somewhere else?  Would love some feedback here.  In this case, our iOS app would make an api call to our Rails app to retrieve a user's news feed.  Rails app would retrieve news feed list (just post IDs) from Redis, and then Rails app would need to query to get the rest of the info for each post.  Should we query our Postgres DB directly?  But that will be a lot of calls to our DB.  Should we create another Redis store (so at least it's in memory) where we store all of the posts from our DB and we query this to get the post information?  Or should we forget Redis and go with MongoDB or Cassandra so we can have higher storage limits?</span>  

<span>Thanks for your help in advance.</span>

</div>