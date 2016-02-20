## [Strategy: Sample to Reduce Data Set](/blog/2008/4/29/strategy-sample-to-reduce-data-set.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Tuesday, April 29, 2008 at 4:01PM</span></div>

<div class="body">

**Update:** [Arjen](http://arjen-lentz.livejournal.com/112554.html) links to video [Supporting Scalable Online Statistical Processing](http://youtube.com/watch?v=KYUay3dCWBc) which shows  
"rather than doing complete aggregates, use statistical sampling to provide a reasonable estimate (unbiased guess) of the result."  

When you have a lot of data, [sampling](http://en.wikipedia.org/wiki/Sampling) allows you to draw conclusions from a much smaller amount of data. That's why sampling is a scalability solution. If you don't have to process all your data to get the information you need then you've made the problem smaller and you'll need fewer resources and you'll get more timely results.

Sampling is not useful when you need a complete list that matches a specific criteria. If you need to know the exact set of people who bought a car in the last week then sampling won't help.  

But, if you want to know many people bought a car then you could take a sample and then create estimate of the full data-set. The difference is you won't really know the exact car count. You'll have a confidence interval saying how confident you are in your estimate.  

We generally like exact numbers. But if running a report takes an entire day because the data set is so large, then taking a sample is an excellent way to scale.

</div>