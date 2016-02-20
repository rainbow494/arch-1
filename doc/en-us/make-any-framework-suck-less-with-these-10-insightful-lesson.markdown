## [Make Any Framework Suck Less With These 10 Insightful Lessons](/blog/2014/11/26/make-any-framework-suck-less-with-these-10-insightful-lesson.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, November 26, 2014 at 8:56AM</span></div>

<div class="body">

![](https://farm8.staticflickr.com/7499/15681853468_b1a1c20ddf_m.jpg)

[Alexey Migutsky](https://twitter.com/mr_mig_by) in [2 years with Angular](http://www.fse.guru/2-years-with-angular) has a lot to say about Angular, which I can't comment on at all, not being an Angular user. But burried in his article are some lessons for building better frameworks that obviously come from deep experience. Frameworks will always suck, but if you follow these lessons will your frameworks suck less? Yes, I think they will.

Here are Alexey's Lessons for framework (and metaframework) developers:

<div id="_mcePaste">

1.  **You should have as small as possible number on abstractions**.
2.  **You should name things consistent with your "thought domain"**.
3.  **Do not mix several responsibilities in your components**. Make fine-grained abstractions with well-defined roles.
4.  **Always describe the intention for your decisions and tradeoffs in your documentation**.
5.  **Have a currated and updated reference project/examples**.
6.  **You abstractions should scale "from bottom up"**. Start with small items and then fit them to a Composite pattern. Do not start with the question "How do we override it globally?".
7.  **Global state is pure evil**. It's like darkness in the horror films - you never know what problems you will have when you tread into it...
8.  **The dataflow and data changes should be granular and localized to a single component**.
9.  Do not make things easy to use, **make your components and abstractions simple to understand**. People should learn how to do stuff in a new and effective way, do not ADAPT to their comfort zone.
10.  **Do not encode all good things you know in the framework**.

</div>

</div>