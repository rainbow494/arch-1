## [10 Program Busting Caching Mistakes](/blog/2014/7/16/10-program-busting-caching-mistakes.html)

<div class="journal-entry-tag journal-entry-tag-post-title"><span class="posted-on">![Date](/universal/images/transparent.png "Date")Wednesday, July 16, 2014 at 9:00AM</span></div>

<div class="body">

![](https://farm6.staticflickr.com/5488/14460128067_74aedf3edf_m.jpg)

While [Ten Caching Mistakes that Break your App](http://www.codeproject.com/KB/web-cache/cachingmistakes.aspx?azid=74) by Omar Al Zabir is a few years old, it is still a great source of advice on using caches, especially on the differences between using a local in-memory cache and when using a distributed cache.

<div id="_mcePaste">Here are the top 10 mistakes (summarized):</div>

<div id="_mcePaste">

1.  <span>**Relying on a default serializer**. Default serializers can use a lot of CPU, especially for complex types. Give some thought to the best serialization and deserialization method for your language and environment.</span>
2.  <span>**Storing large objects in a single cache item**. Because of serialization and deserialization costs, under concurrent load, frequent access to large object graphs can kill your server's CPU. Instead, break up the larger graph into smaller subgraphs and cache them separately. Retrieve only the smallest unit you need.</span>
3.  <span>**Using cache to share objects between threads.** Race conditions, when writes are involved, develop if parts of a program are accessing the same cached items simultaneously. Some sort of external locking mechanism is needed. </span>
4.  <span>**Assuming items will be in cache immediately after storing them.** Never assume an item will be in a cache, even after it was just written, because a cache can flush items when memory gets tight. Code should always check for a null return value from a cache.</span>
5.  <span>**Storing entire collection with nested objects.** Storing an entire collection when you need to get a particular item results in poor performance because of the serialization overhead. Cache individual items separately so they can be retrieved separately. </span>
6.  <span>**Storing parent-child objects together and also separately.** Sometimes an object will simultaneously be contained in two or more parent objects. To not have the same object stored in two different places in the cache store it on its own under its own key. </span>The parent objects will then read the objects when access is needed.
7.  <span>**Caching Configuration settings.** Store configuration data in a static variable that is local to your process. Accessing cached data is expensive so you want to avoid that cost when possible.</span>
8.  <span>**Caching Live Objects that have open handle to stream, file, registry, or network.** Don't cache objects the have references to resources like files, streams, memory, etc. When the cached item is removed from the cache those resources will not be deleted and system resources will leak. </span>
9.  <span>**Storing same item using multiple keys.** It can be convenient to access an item by a key and an index number.  This can work when a cache is in-memory because the cache can contain a reference to the same object which means changes to the object will be seen through both access paths. When using a remote cache any updates won't be visible so the objects will get out of sync. </span>
10.  <span>**Not updating or deleting items in cache after updating or deleting them on persistent storage.** Items in a remote cache are stored as a copy, so updating an object won't update the cache. The cache must specifically be updated for the changes to be seen by anyone else. With an in-memory cache changes to an object will be seen by everyone. Same for deletion. Deleting an object won't delete it from the cache. It's up to the program make sure cached items are deleted correctly.</span>

</div>

</div>