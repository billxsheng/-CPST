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
- 