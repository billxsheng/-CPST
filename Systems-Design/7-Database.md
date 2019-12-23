# Database

## RDBMS
- A relational database is a collection of data items organized in tables

### ACID
- Set of properties of relational db transactions
    - Atomicity- Each transaction is all or nothing (no partial transactions)
    - Consistency- Any transaction will bring the db from one valid state to another
    - Isolation- Executing transactions concurrently has the same results as if the transactions were executed serially
    - Durability- Once a transaction has been committed, it will remain so. Database should hold updates even if system fails or restarts.

### Scaling a RDBMS
#### Master-slave Replication
- Master serves reads and writes, replicating writes to one or more slaves, which serve only reads
- Slaves can also replicate to additional slaves in a tree fashion
- If master goes offline, the system can continue to operate in read-only mode

#### Master-master replication
- Both masters serve reads and writes and coordinate with each other on writes
- If either master goes down, the system can continue to operate with both reads and writes 
- disadvantage is that you need a load balancer and these systems are loosely consistent and have increased write latency
- there is also a potential loss of data if the master fails before new written data can be replicated

#### Federation
- Splits up databases by function
- For example, three databases instead of a single monolithic database
- Results in less read and write traffic to each db and less replication lag
- Disadvantage is that its not effective if schema requires huge functions/tables, more hardware and added complexity 

#### Sharding
- Distributes across different databases such that each database can only manage a subset of the data
- As number of users increases, more shards are added to the cluster
- Less read/write traffic, less replication, and more cache hits
- Replication must be added to avoid data loss when a shard goes down
- Like federation, there is no single central master serializing writes, allowing you to write in parallel with increased throughput
- Common way to shard is to use user last name initial or user location
- Disadvantage is that you will need to update app logic to work with shards, complex SQL queries, data distribution can become lopsided (rebalancing adds additional complexity or you can use a sharding function to properly distribute data)
- Joining data from multiple shards as well as adding more hardware increases complexity 

#### Denormalization
- Improves read performance at the expense of write performance
- Redundant copies of data are written in multiple tables to avoid expensive joins
- Once data becomes distributed with federation and sharding, managing joins increases complexity (denormalization can overcome the need for complex joins) 
- In some systems, reads can heavily outnumber writes
- A read resulting in a complex database join can be expensive
- Disadvantage is that data is duplicated, constraints can help redundant copies stay in sync which increases complexity of database design, denormalization database under heavy write load might perform worse than normalized counterpart

#### SQL Tuning
- Important to benchmark and profile to simulate and reveal bottlenecks
Benchmark- Simulate high-load situations with tools such as ab
Profile- Enable tools such as slow query log to track performance issues

- They might point you to the following optimizations

Tighten up the schema
- char instead of varchar
- int for larger numbers up to 4 billion
- not null to improve search performance
- avoid storing large blobs, store location of where to get object instead
Use good indices

Use good indices
- columns that you are querying (select, group by, order by, join) could be faster with indices
- placing index can keep data in memory, requiring more space
- writes could also be slower since index needs to be updated
- when loading large amounts of data, it might be faster to disable index, load the data, then rebuild the index

Avoid expensive joins
- denormalize where performance demands it 

Partition tables
- break up the table by using hot spots in a separate table to help keep in memory 

Tune query cache
- query cache could lead to performance issues

## NoSQL
- A collection of data items represented in key-value store, document store, wide column store, or a graph database 
- data is denormalized and joins are done in app code
- most NoSQL stores lack true ACID transactions and favor eventual consistency 

### BASE

Basically available- system guarantees availability

Soft state- state of system may change over time even without input

Eventual consistency- the system will become consistent over a time period given that the system does not receive input during that period

### Key-value store
- abstraction of hash table
- allows for constant time reads and writes and backed by memory or SSD 
- data stores can maintain keys allowing efficient retrieval of ranges
- can also store metadata with a value
- used for simple data models or for rapidly-changing data such as in-memory cache layer
- complexity shifted to app layer since operations are limited

### Document Store
- abstraction of key-value store with documents as values
- centered around JSON, XML, binary documents where a document stores info of a given object
- document stores provide APIs to query based on internal document structure
- organized by collections, tags, metadata, or directories
- high flexibility and are often used for changing data

### Wide Column Store
- abstraction of nested map
- wide column store's basic data unit is a column
- columns can be grouped into column families and super column families further group column families
- you can access each column independently with a row key and columns with the same row key form a row
- Cassandra and HBase maintain keys in lexicographical order allowing retrieval of key ranges
- wide column stores offer high availability and scalability, for large data sets 

![](https://camo.githubusercontent.com/823668b07b4bff50574e934273c9244e4e5017d6/687474703a2f2f692e696d6775722e636f6d2f6e3136694f476b2e706e67)

### Graph Database
- abstraction of a graph
- in a graph database, each node is a record and each arc is a relationship between two nodes
- optimized to represent complex relationships with many foreign keys or many-to-many relationships
- high performance for data models with complex relationships such as a social network
- Neo4J

## SQL or NoSQL

### Reasons for SQL
- structured data
- strict schema
- relational data
- need for complex joins
- transactions
- clear patterns for scaling
- more established
- lookups by index are very fast

### Reasons for NoSQL
- semi-structured data
- dynamic or flexible schema
- non-relational data
- no need for complex joins
- store TB of data
- very data intensive workload 