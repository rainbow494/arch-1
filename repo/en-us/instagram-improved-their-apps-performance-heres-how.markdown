## [Instagram Improved their App's Performance. Here's How.](/blog/2014/9/29/instagram-improved-their-apps-performance-heres-how.html)

    

    

![](http://farm8.staticflickr.com/7011/6464246201_bddb8c499e_o.jpg)

Is [flat design](http://en.wikipedia.org/wiki/Flat_UI_Design) just another pretty face or is it a huge performance hack cloaked as a UI revolution? It turns out flat design is a stone cold performance win.

This and more is expertly explained by [Tyler Kieft](http://tylerkieft.com/), Engineer at Instagram, in a crisp and content filled talk he gave at the [@scale conference](http://atscaleconference.com/): [Instagram on Typical Android](https://www.youtube.com/watch?v=GHTO2WKDO6I#t=8927). This talk was part of [series of talks](/blog/2014/9/22/how-facebook-makes-mobile-work-at-scale-for-all-phones-on-al.html) given by Facebook on how to design for the reality of mobile applications across the globe, where phones are slower, screens are smaller, and networks are slower than they are in the US.

Designing for a typical phone rather than a high-end phone required the Instagram team to rethink their design in a deep way. One of the revelations in Tyler's talk was that **moving to a flat design** was huge in making the application more beautiful, more usable, and it also substantially increased performance.

This was quite a surprise. I've only ever thought of flat design as just a way to think about how to build pretty UIs. Silly me. Thanks to Tyler for explaining the benefits of flat design so clearly and forcefully, using Instagram as a great example of what is possible.

Flat design is the anti-skeuomorphism, going digital native, eschewing a slavish obsession with the appearance of reality, adopting simple elements, simple typography, flat colors, and simple designs.

Using flat design Instagram was able shave off 120ms from its cold start times. It was also able to reduce the number of assets it took to display the feed screen from 29 assets down to 8 assets. All while making the application more beautiful, more usable, with more focus given to the content across different phone sizes.

How did flat design make all this possible? 

## Moving to a Flat Design

*   Instagram rewrote their UI to focussing on better performance across a wide range of UIs available on Android.

*   When Instagram was released on Android in 2012 it was built in about 4 months by 3 people. Two engineers and one designer. The Android version used the same design as for the iOS version.

*   The design used luscious gradients and lots of UI elements.

*   The transition to iOS7, to flat design, made for a much simpler and more beautiful product. No more gradients. Took out boxes. No more shadows. 

*   From the experience of adapting to flat design **they learned**:

    *   **Flat design is an opportunity to do less**, develop code faster, and ship products faster. Which is great for developers.

    *   **Flat design is a lot more performant**. Not only is the developer doing less, the phone is doing less to display the UI.

*   Goals for the new Android version using what was learned from the iOS7 flat design redesign:

    *   **Make it flat, make it faster**. It was not a rewrite. Navigation patterns didn't change.

    *   **Be screen space conscious**. Take a fresh look at every screen and figure out how to better adapt to all the screen sizes out there.

    *   **Make it beautiful**. It underlies everything they do at Instagram.

*   The overall effect is dramatic simplification. What changes were made? 

    *   **Took everything out of the chrome**. Took out all the gradients and glossy buttons. Went to outline shapes instead of gradient buttons for the icons. What remained were solid colors and flat shapes. Want the UI to fade into the background. 

    *   **Took out the comment icon**, making comments the full width of the screen, giving more space to the comment text. Make content what stands out on the screen. With less UI people using small screens have more room to read the text.

    *   **Forked the phone layout for taking pictures**. On small phones they use a design with action buttons on the top of the screen. For larger screens all the commands are at the bottom.

    *   **Unnecessary UI all over the app was removed to give more focus to content**. Three layers of chrome on the search screen was reduced to two layers. This gave back a lot of space on small phones. For the Samsung Galaxy buttons were slimmed, which is easy to do programmatically with flat design, giving back a lot of space to the content.

    *   Note, the talk has a lot of good pictures of the different designs so is well worth watching just for the before and after pictures.

## Why flat design?

*   **Ship fewer assets with Asset Tinting**. Which means APK size is smaller which is great on a small network. The magic is **Asset tinting** **(**something I've never heard of before). [Asset tinting](http://blog.danlew.net/2014/08/18/fast-android-asset-theming-with-colorfilter/) means assets, which in this case are images, can be colorized programmatically. For example, a grey heart can be colored red programmatically. Asset tinting means less assets need to be shipped. Traditionally separate images are needed for each button state, pressed, unpressed, selected, etc. With tinting all the images for different states do not need to be shipped anymore. Only on image is required and the different states can be tinted.

*   **Load fewer assets**. Which means the UI displays faster and less memory is used to store bitmaps. Every asset that needs to be displayed must be read off of flash memory and decoded into a bitmap. The less that is done the faster that app becomes.

*   **Faster iteration times**. If you want to change colors or new development you don’t need a designer anymore. Just change the code and recompile.

*   The Results:

    *   **Before flat design it took 29 different assets to display the feed screen**. After flat design it took **8 assets** to display the same page. Just the shapes were needed to display icons and the logo. Everything else was drawn in code as solid colors and rectangles. Just that shaved off **120ms** from the cold start time on all devices.

    *   **With flat design the whole app got faster**. With every screen less was being done. Less assets were loaded and the whole app felt peppier. Users in their reviews commented on how fast the app felt after the redesign. People really loved it. People appreciated the speed gains that come from a design that matches the platform.

## Improving Cold Start Times

*   Cold start time is the time it takes for an application to start and become responsive. From tapping the icon to clicking around the app and it works. The goal is to get the **app to start up super fast** so users on low end phones have a great experience.

*   A few years ago on a low-end Galaxy Y the start up time for Instagram was 3 seconds. on a high-end Galaxy S5 the startup time was 750ms .

*   Now on a Galaxy Y Instagram takes 1.5 seconds to start up. On a Galaxy S5 it takes **400ms**.

*   How? (besides removing assets)

    *   **Profile the app**.

        *   Figure out what is slowing the app down.

        *   On Android you can use **method tracing** and you can put timing statements in the code. Method tracing over counts smaller methods. **Timing statements** is wall clock time, not machine time. Using both will give a good feel for what’s slow.

    *   **Fix the slowest thing**.

        *   **Lazy load**. Remove items from the cold start path.

        *   **Rewrite slow code**. For example, slow JSON parsing code was rewritten to be faster.

        *   **Defer to background thread**. Don’t do in the UI thread which can be done in the background.

    *   **Iterate**. Start the profile step again.

*   **App-wide singletons were found to be slow**. Found through timings.

    *   A lot of **heavy singletons** were started before the app could start: HTTP Client, Cookie Store, Image Cache, Video Cache. Don’t need really need these things to show the UI to the user. They can be loaded in the background in parallel. 

    *   **Two-part Lazy-Loading**

        *   Want to **initialize the singleton in the background** yet have programmers still treat it like a singleton that’s always available to the app. Don't want programmers to have to check if a singleton is available yet, because then it's not a singleton.

        *   **Create enough of the object on UI thread** so that the public API is completely functional and can be used by programmers. Defer hard work to background thread. For the cache this means opening and reading from disk storage. For the Client certificates are loaded in the background. Cookies are deserialized and decoded in the background. With these changes the UI appears on the screen much faster.

*   **Newsview was slow**. Found through method tracing.

    *   Newsview, which shows all your likes and comments, was originally written as a webview. It was required to be loaded on startup to show the user their data as quickly as possible. 

    *   The problem is there is no control over the webview. It has it's own stack and caching system. **Converted it to native**. It took 2-4 weeks. After the native conversion there was a **30% reduction** in the cold start time.

## Lessons Learned

*   **Fast cold start times are achievable**. If they are fast they can get even faster. Profile, fix, iterate.

*   **Use pixels judiciously**. Look at every screen to see what isn’t needed. Users in other countries are using dramatically smaller phones than are used in the US.

*   **Mobile phones love simple designs and so do mobile developers**. It's a lot easier and a lot faster.

## Related Articles

*   [On Reddit](http://www.reddit.com/r/programming/comments/2iqfr7/how_instagram_improves_their_apps_performance/)
*   [The Instagram Architecture Facebook Bought For A Cool Billion Dollars](http://highscalability.com/blog/2012/4/9/the-instagram-architecture-facebook-bought-for-a-cool-billio.html)
*   [Instagram Architecture Update: What’s New With Instagram?](http://highscalability.com/blog/2012/4/16/instagram-architecture-update-whats-new-with-instagram.html)
*   [Can someone articulate Google's material design vs Apple's flat design?](http://www.quora.com/Can-someone-articulate-Googles-material-design-vs-Apples-flat-design)

    