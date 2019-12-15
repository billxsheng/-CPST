# Trade-offs
## Performance vs Scalability
> A service is scalable if it results in increased performance in a manner proportional to resources added.
> Generally, increasing performance means serving more units of work, but it can also be to handle larger units of work such as when datasets grow
- if you have a performance problem, your system is slow for a single user
- if you have a scalability problem, your system is fast for a single user but slow under heavy load
## Latency vs Throughput
> Latency is the time to perform some action or to produce some result. Throughput is the number of such actions/results per unit of time
- generally you aim for maximal throughput with acceptable latency
## Availability vs Consistency
## CAP theorem- Consistency, Availability, Partition Tolerance
- You can only support two of the following guarantees IN A DISTRIBUTED COMPUTER SYSTEM

Consistency- Every read receives the most recent write or an error. (ATM is always updated)

Availability- Every request receives a response, without guarantee that it contains the most recent version of the information (ATM is always available)

Partition Tolerance- The system continues to operate despite arbitrary partitioning due to network failures. (ATM can withstand network failures)

> Networks aren't reliable, so you'll need to support partition tolerance. You need to make a trade-off between consistency and availability

### CP
> Waiting for a response from the partitioned node might result in a timeout error. CP is a good choice if your business needs require atomic reads and writes. 

### AP
> Responses return the most recent version of the data available on a node, which might not be the latest. Writes might take some time to propagate when the partition is resolved. AP is a good choice if the business needs allow for eventual consistency or when the system needs to continue working despite external errors. 
### Why is there a conflict?
- any machine of 100 can give the same answer
- any machine should always give an answer
- not possible if they cannot communicate due to networks
## Consistency Patterns
- With multiple copies of the same data, we are faced with options on how to synchronize them so data is consistent. Every read requires the most recent write or an error.
### Weak Consistency
- After a write, reads may or may not see it. A best effort approach is taken.
- Works well in real-time use cases such as multiplayer games. For example, if you are on the phone and lose reception for a few seconds. When you regain connect you don't hear what was spoken during connection loss.
### Eventual Consistency
- After a write, reads will eventually see it. Data is replicated asynchronously. 
- Approach is seen in email, DNS, and other high-availability systems. 
### Strong Consistency
- After a write, reads will see it
- Seen in file systems and relational databases. Works well in systems that need transactions.
## Availability Patterns
### Fail-over
#### Active-passive
- Active-passive failover sends heartbeats between active and the standby passive server. If the heartbeat is interrupted, the passive server takes over the active's IP and resumes service. Only active server manages traffic. Aka master-slave failover.
#### Active-active
- Both servers are managing traffic, spreading the load between them. Aka master-master failover. 
#### Disadvantage
- fail-over adds complexity 
- there is a potential loss of data if the active system fails before data can be replicated to the passive 
### Replication
- explained in databases section (master-slave and master-master)

