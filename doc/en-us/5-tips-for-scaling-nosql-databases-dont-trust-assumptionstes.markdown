## [5 Tips for Scaling NoSQL Databases: Don’t Trust Assumptions—Test, Test, Test!](/blog/2014/9/24/5-tips-for-scaling-nosql-databases-dont-trust-assumptionstes.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, September 24, 2014 at 9:08AM</span></div>

<div class="body">

![](https://farm4.staticflickr.com/3907/15155548569_94b0c59654_m.jpg)

Alex Bordei, product manager for Bigstep’s Full Metal Cloud, in [Scaling NoSQL databases: 5 tips for increasing performance](http://radar.oreilly.com/2014/09/scaling-nosql-databases-5-tips-for-increasing-performance.html), shares a nice set of lessons he's learned about how NoSQL databases scale:

*   **Never assume linearity in scaling**. Hardware prices grow exponentially as the specs increase, but not all software can take full advantage of all that power. So you may be paying for hardware your database can't use. Find the sweet spot for price and hardware capabilities.
*   **Tests speak louder than specs**. Don't trust vendor documentation. It's cheap to spin up new instances so test the specs for yourself.
*   **Mind the details: Memory & CPU numbers matter**. For in-memory databases the specs on your memory modules matter. Faster memory means faster performance. Same for CPU frequencies. Pay attention to what your money is buying.
*   **Do not neglect network latency**. Paying for fast memory and fast CPU won't do a lot of good if your network is slow. 
*   **Avoid virtualization with NoSQL databases**. Virtualization can exact a 20-200% performance penalty. Noisy neighbors also help ruin the neighborhood. Up to 400% performance gains can be seen by switching away from virtualization and adopting bare metal clouds.

Lots of good advice. Each of these points in discussed in more detail in the [original article](http://radar.oreilly.com/2014/09/scaling-nosql-databases-5-tips-for-increasing-performance.html), which is well worth reading.

</div>