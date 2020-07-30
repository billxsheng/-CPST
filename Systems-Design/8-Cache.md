# Cache

![](https://camo.githubusercontent.com/7acedde6aa7853baf2eb4a53f88e2595ebe43756/687474703a2f2f692e696d6775722e636f6d2f51367a32344c612e706e67)

- caching improves page load times and can reduce load on servers and databases
- in the above model, the dispatcher first checks to see if the request has been made before and tries to find the previous result in order to save the actual execution
- Databases often benefit from a uniform distribution of reads and writes across partitions
- Popular items can skew distribution causing bottlenecks
- Putting a cache in front of a database can absorb uneven loads and spikes 

## Client Caching
- Caches can be located on the client side, server, side, or in a distinct cache layer

## CDN Caching
- CDNs are considered a type of cache

## Web Server Caching
- Reverse proxies and caches can serve static/dynamic content directly
- servers can also cache requests and returning responses without having to contact app servers

## Database Caching
- Your database usually includes some level of caching in default configurations
- Tweaking this can lead to boosts in performance

## Application caching
- in-memory caches such as Redis and Memcached are key-value stores between app and data storage 
- data is held in RAM which is faster than typical disk storage
- RAM is more limited than disk storage so we use cache invalidation algorithms such as LRU (least recently used) to invalidate cold entries and keep hot data in RAM
- cache levels fall into two general categories, database queries and objects 
- we want to avoid file-based caching because it makes cloning and auto-scaling hard

### LRU Cache
- Cache data structure used to figure out what should be evicted when the cache is full
- Goal is to always have least recently used item accessible in O(1) times
- Doubly linked list with a hash map
- Evicts least recently used items when memory is low


## Caching at the database query level
-  whenever you call the database, cache the query as the key and result as the value 
- this approach suffers expiration issues 
    - hard to delete a cached result with complex queries
    - if one piece of data changes, you need to delete all queries that include the changed cell

## Caching at the object level
- See your data as an object similar to what you do with your application code
- Have the application assemble the dataset into a class instance
    - remove the object from cache if data has changed
    - allows for async processing, workers assemble objects by consuming latest cached object 
    - user sessions, activity streams

## When to update cache
- since you can only store a limited amount of data in cache, you will need to determine which cache update strategy works for your use case

### Cache-aside
- the app is responsible for reading/writing from storage
- the cache does not interact with storage directly
    - app looks for entry in cache, resulting in cache missing
    - load entry from db
    - add entry to cache
    - return entry
- Memcached approach
- only requests data is cached, avoids filling up cache with data that is not requested
- disadvantage is that data can become stale if it is updated in the database (TTL time-to-live which forces an update of the cache entry or by using write-through)
![](https://camo.githubusercontent.com/7f5934e49a678b67f65e5ed53134bc258b007ebb/687474703a2f2f692e696d6775722e636f6d2f4f4e6a4f52716b2e706e67)

### Write-through
- Application uses cache as the main data store, reading/writing data while the cache is responsible for reading/writing to the database
    - app adds/updates cache entry
    - cache synchronously writes entry to data store
    - return
- slow overall operation due to write but subsequent reads are fast
- users are more tolerant of latency when updating data than reading data 
- data in cache is not stale
- disadvantage is that most data written might never be read (minimized with TTL) 
- when a new node is created due to failure, the new node will not cache entries until entry is updated in the database

![](https://camo.githubusercontent.com/56b870f4d199335ccdbc98b989ef6511ed14f0e2/687474703a2f2f692e696d6775722e636f6d2f3076426330684e2e706e67)

### Write-behind
- application
    - add/update entry in cache
    - async write entry to the data store, improving write performance
- disadvantage is that there could be data loss if cache goes down before contents hit data store
- complex to implement than it is to implement cache-aside or write-through

![](https://camo.githubusercontent.com/8aa9f1a2f050c1422898bb5e82f1f01773334e22/687474703a2f2f692e696d6775722e636f6d2f72675372766a472e706e67)

## Refresh-ahead
- configure cache to automatically refresh any recently accessed cache entries prior to expiration 
- results in reduced latency vs read-through if cache can predict which items are likely to be needed in the future
- disadvantage is that not accurately predicting items can result in reduced performance 

![](https://camo.githubusercontent.com/49dcb54307763b4f56d61a4a1369826e2e7d52e4/687474703a2f2f692e696d6775722e636f6d2f6b78746a7167452e706e67)

## Cache Disadvantages
- maintain consistency (cache invalidation)
- cache invalidation is a difficult problem, additional complexity with when to update the cache 