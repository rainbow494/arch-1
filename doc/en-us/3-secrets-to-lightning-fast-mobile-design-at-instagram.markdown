## [3 Secrets to Lightning Fast Mobile Design at Instagram](/blog/2012/6/7/3-secrets-to-lightning-fast-mobile-design-at-instagram.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Thursday, June 7, 2012 at 9:15AM</span></div>

<div class="body">

![](http://farm8.staticflickr.com/7011/6464246201_bddb8c499e_o.jpg)

In [Secrets to Lightning Fast Mobile Design](https://speakerdeck.com/u/mikeyk/p/secrets-to-lightning-fast-mobile-design), Instagram co-founder Mike Krieger shares three strategies Instagram uses to outsmart high latency mobile networks and make mobile apps feel faster than they really are, which helped Instagram reach 12 million users in 12 months:

*   **The motivation:** "mobile experiences fill gaps while we wait. no one wants to wait while they wait"
*   **Perform actions optimistically**. Make the user feel productive. Likes, comments, and follows are registered on screen while the like operation is being sent in the background. 
*   **Adaptively preload content**. Load content before it's needed. Don't images according to time, it will take too long, instead re-prioritize based on interest. Show what the social network things is the most popular. Listen to what your user's flicks and taps are telling you.
*   **Move bits when no-one is watching.** Picture uploading starts **before** you share them, most apps wait until after the share screen. The rule is send data as soon as it's ready, then match it with user actions later. This doubles the number of requests, but the perceived performance is improved, even if a cancel is sent out later. Data is deleted on a cancel.

Smart and simple.

## Related Articles

*   [Instagram on HighScalability](http://highscalability.com/display/Search?moduleId=4876569&searchQuery=instagram)

</div>