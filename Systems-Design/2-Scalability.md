# Scalability

## Vertical Scaling
> Scaling by adding more power (CPU, RAM) to existing machine
## Horizontal Scaling
> Scaling by adding more machines into pool of resources
## Caching
> Software component that stores data so that future requests for the same data can be served quickly; may be due to an earlier computation or a copy of data stored elsewhere
## Load Balancing
> Computing concept that involves distributing workloads across multiple computing resources such as computers, clusters, cpus, or disk drives
## Database Replication
> Database process that involves the frequent sharing of data from a database in one computer or server to a database in another so that all users share the same level of information; used to improve reliability, fault-tolerance, or accessibility 
## Database Partitioning
> Database process where very large tables are divided into smaller parts, queries that access only a fraction of data can run faster b/c less data to scan (hash partitioning)

# Scalability (cont'd)
## Clones
- Scalability rule is to ensure that every server contains the same code and does not store users-related data
- User-related data should be stored in an external data source/cache that can be accessed by all servers (persistent cache)
## Databases
### Path 1
- hire DBA
- master-slave replication (read to slave databases, write to master)
- add more and more RAM
- every action after sharding, denormalization, SQL tuning  will be more expensive
### Path 2
- denormalize from the beginning
- switch to mongoDB (more scalability)
- joins are now done in app code
- even after you switch to noSQL, requests can still get slower which leads to caching
## Caches
- cache is a simple key-value store and is in-memory in this case
- user requests will begin in the cache before getting data from the source
- reads and writes are very fast
### Strategy 1- Cached DB Queries
- whenever you query the db, you store the result dataset in cache
- hashed query of the query is the cache key
- main issue is expiration- when one piece of data is changed, you need to delete all cached queries that include that piece of data
### Strategy 2- Cached Objects
- let the class store the complete instance of the assembled dataset in the cache
- cache object made up of several db requests that would be cached with method 1
- cache after class has finished assembling
- you can easily get rid of the object when something changes
## Asynchronism
### Async #1
- "bake the breads at night and sell them in the morning"
- doing time-consuming work in advance and serve work with low request time
### Async #2
- jobs are sent to a job queue while user continues 
