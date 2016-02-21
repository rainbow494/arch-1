## [Product: SciDB - A Science-Oriented DBMS at 100 Petabytes](/blog/2010/4/29/product-scidb-a-science-oriented-dbms-at-100-petabytes.html)

    

    

![](http://www.scidb.org/images/sci-db.gif)

Scientists are doing it for themselves. Doing what? Databases. The idea is that most databases are designed to meet the needs of businesses, not science, so scientists are banding together at [scidb.org](http://scidb.org/) to create their own Domain Specific Database, for science. The goal is to be able to handle datasets in the 100PB range and larger.

> SciDB, Inc. is building an open source database technology product designed specifically to satisfy the demands of data-intensive scientific problems. With the advice of the world's leading scientists across a variety of disciplines including astronomy, biology, physics, oceanography, atmospheric sciences, and climatology, our computer scientists are currently designing and prototyping this technology
> 
> The scientists that are participating in our open source project believe that the SciDB database — when completed — will dramatically impact their ability to conduct their experiments faster and more efficiently and further improve the quality of life on our planet by enabling them to run experiments that were previously impossible due to the limitations of existing database systems and infrastructure. Many of the world's leading computer scientists with expertise in database systems have contributed to the design and architecture of the system to meet the needs of the world's scientists.

SciDB looks like a cool project and follows what might be considered a trend, instead of beating a general tool into submission, build a specialized tool that does what you need it to do. More details about SciDB can be found in the paper [A Demonstration of SciDB: A Science-Oriented DBMS](http://scidb.org/Documents/SciDB-VLDB09-paper.pdf). A nice succinct [poster](http://scidb.org/Documents/SciDB-VLDB09-poster.pdf) is available summarizing the product.

Some interesting bits from the paper:

*   The data model used is that of _multi dimensional, nested array model with array cells containing records, which in turn can contain components that are mult-dimensional arrays_. Arrays can have any number of named dimensions. Sparse, ragged, and unbounded arrays are supported.
*   Science is not a monolith. Different specialties would like different data representations. Users in chemistry, biology, and genomics would like to see a graph database. Users in solid modeling would like a mesh. But the array is a natural for astronomy, oceanography, fusion, remote sensing, climate modeling, and seismology.
*   Postgres-style user defined functions coded in C++ allow for custom operators. There are no built-in operators, all operators are UDFs.
*   No overwrite. It's not transactional. Scientists don't want to update values in place. If a cell is declared updateable there's a history mechanism that traces the history of changes to each cell. A delta compression mechanism to reduce data usage. 
*   Grid. A shared nothing cluster to 1000s of nodes. Data is split horizontally over. Partitioning is flexible. Partitions are not fixed, they can change over time to adapt to changing data. Partitions can be based on dimensions or attributes.
*   Queries. Queries refer to a single array and the query planner takes care of splitting the query across all the nodes. UDFs are run in parallel across the cluster.
*   "In Situ" Data. No load process required. Data analysis is often preceded by a data load phase which can take forever with large datasets. Adaptors will be created so data not in the SciDB format can be used without loading.
*   Integrated Cooking. Data often needs to be cooked/changed/transformed/normalized to be useful. The cooking operators will be integrated into SciDB.
*   Provenance. The steps for how an array was created will be remembered so scientists can show how results were derived.
*   Uncertainty. Attributes supports a value and error components so uncertainty can be represented natively in the database.
*   Open source. A non-profit has been setup to create a commercial quality open source database for the scientific community.

Not your typical looking database. The Domain Specific aspects create something unique and hopefully uniquely useful. I wonder if we'll see more Domain Specific Database efforts in the future?

    