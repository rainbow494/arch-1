## [8 Ways Stardog Made its Database Insanely Scalable](/blog/2014/1/20/8-ways-stardog-made-its-database-insanely-scalable.html)

    

    

![](http://farm8.staticflickr.com/7422/11955152784_753d953b65_m.jpg)

[Stardog](http://stardog.com/) makes a commercial graph database that is a great example of what can be accomplished with a scale-up strategy on BigIron. In a [recent article](http://weblog.clarkparsia.com/2014/01/10/scalability-improvements-in-stardog-21) StarDog described how they made their new 2.1 release insanely scalable, improving query scalability by about 3 orders of magnitude and it can now handle 50 billion triples on a $10,000 server with 32 cores and 256 GB RAM. It can also load 20B datasets at 300,000 triples per second. 

What did they do that you can also do?

1.  **Avoid locks by using non-blocking algorithms and data structures**. For example, moving from BitSet to ConcurrentLinkedQueue.
2.  **Use ThreadLocal aggressively to reduce thread contention and avoid synchronization**.
3.  **Batch LRU evictions in a single thread**. Triggered by several LRU caches becoming problematic when evictions were being swamped by additions. Downside is batching increases memory pressure and GC times.
4.  **Move to SHA1 for hashing URIs, bnodes, and literal values**. Making hash collisions nearly impossible enable significant speedups and simplifies code. The actual mechanism of the speedup was not described, but probably reduced random disk accesses due to collisions using the previous hash algorithm.
5.  **Move from mmap on the JVM (which is very bad) to an off-heap memory allocation scheme**. Benefit is a fine-grained control over disk flushes, more efficient available system memory usage, and is (roughly) as fast as memory mapping.
6.  **Reduce GC pauses by engineering the code to create fewer objects**. For example, use a StringBuilder object in a RDF parser rather than create a new one each time. 
7.  **Reduce GC pauses by reducing the amount of cache use on the heap to relieve memory pressure**. Result is GC pauses are now taking 1% or less of the overall bulk loading time.
8.  **Queries use the off heap memory manager**. Need a large heap and a large heap kills you with garbage collection, so the solution is to manage memory by hand. Also, the new memory manager performs some static analysis of the query using database statistics to guide its behavior.

A well written article with extended explanations that are [well worth reading](http://weblog.clarkparsia.com/2014/01/10/scalability-improvements-in-stardog-21). 

    