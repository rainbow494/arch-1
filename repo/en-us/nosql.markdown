## [RebornDB: the Next Generation Distributed Key-Value Store](/blog/2015/7/8/reborndb-the-next-generation-distributed-key-value-store.html)

    

    

![](https://lh3.googleusercontent.com/JX8mA9Itr73bazA4n43O54TGL5qDWmnzaF8hQXph_kWSoQArwpZKRB1pKoX3HQDggFHLs53g_aU7Vp8y1nuufj_n2ZxGTRm_R05C3rOp3xMaBfv5mrcEOt5-A4hS-WkTyZAFhNw)There are many key-value stores in the world and they are widely used in many systems. E.g, we can use a Memcached to store a MySQL query result for later same query, use MongoDB to store documents for better searching, etc.

For different scenarios, we should choose different key-value store. There is no silver-bullet key-value store for all solutions. But if you just want a simple key-value store, easy to use, very fast, supporting many powerful data structures, redis may be a good choice for your start.  

Redis is advanced key-value cache and store, under BSD license. It is very fast, has many data types(String, Hash, List, Set, Sorted Set …), uses RDB or AOF persistence and replication to guarantee data security, and supplies many language client libraries.

Most of all, market chooses Redis. There are many companies using Redis and it has proved its worth.

    Although redis is great, it still has some disadvantages, and the biggest one is memory limitation.  Redis keeps all data in memory, which limits the whole dataset size and lets us save more data impossibly.    

The official redis cluster solves this by splitting data into many redis servers, but it has not been proven in many practical environments yet. At the same time, it need us to change our client libraries to support “MOVED” redirection and other special commands, this is unacceptable in running production too. So redis cluster is not a good solution now.

# QDB

We like redis, and want to go beyond its limitation, so we building a service named QDB, which is compatible with redis, saves data in disk to exceed memory limitation and keeps hot data in memory for performance.

## Introduction

[QDB](https://github.com/reborndb/qdb) is a redis like, fast key-value store.It has below good features:

[Click to read more ...](/blog/2015/7/8/reborndb-the-next-generation-distributed-key-value-store.html)

    