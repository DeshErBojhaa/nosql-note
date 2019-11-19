## Topics
- Consistency
- Partitioning
- Storage Layout
- Query Models
- Distributed Data Processing via MapReduce

# ACID vs BASE
BASE -> Basically Available, Soft-state, Eventual consistency

## SOME CONSISTANCY TYPES

### Strict Consistancy: 
All write operations must return data from the latest completed write operation, regardless of which replica serves the query.

This can be achieved in 2 ways
- Use the same node for reading and writing
- Use two phase commit or Paxos

### Eventual Consistancy:
User may see different data in different time. But all the nodes will come to a point where it shows the latest completed write data.

### Read Your Own Write (RYOW) Consistance:
Client sees his updates immediately after they hace issued an successful update. Regardless of which node client write and read from. Updates by other clients my not be visible to him instantly.

### Session Consistancy:
RYOW consistance, but active for only the session scope in which the write occurred.

### Casual Consistancy:
If one client reads version x and subsequently writes version y, any client reading version y will also read version x.

### Monotonic Read Consistancy:
Client will see updated data monotonicly.

# Versioning of Data in Distributed System:
Data versioning is important for optimistic concurrency control. Latest data version helpd database to reach to eventual consistancy.

### Timestamp:
Obvious solution, but it's difficult to have a consistent `clock` across the system.

### Optimistic Locking:
A unique counter will be saved with each piece of data. When a data is to update, client have to provide the counter number it seeks to update.
This mechanism does not work well when a lot of servers are sponing and destroying.

### Multiversioning Storage:
In this scheme, for a given row, multiple version can exist concurrently. This provides optimistic concurrency control with compare and swap mechanism. 

### Vectior Clocks:



## Partitioning
When data is no longer persostable in one machine, we need to think about partitioning it. Partitioning (and thus replicating) also helps in scalability such as load balancing and reliability. Depending on _dynamicity_(how often and dynamically storage joins and leaves the system), there are different approaches for this issue.
- **Memory caches** [Replicating]
- **Clustering** [Partitioning]
- **Seperating Reads from Writes** <br />
    Means there are dedicated servers for write operations. Data can be lost if master fails before replication data to slaves. 
    Does not work if strict consistency is required. Works well when read/write ratio is high (more read less write). 
    Changes can be propageted from master to slave by stare transfer or by operation transfer.
- **Sharding** <br />
    Sharding means distributing data in servers in such a way that every server have similar load. Also data requested and updated together     resids on the same server.
    Each shard can have multiple replicas for fault taulance and load balancing.
    To get the partition of the shard, we use `partition = hash(o) mod n`
    Where `o` is the object and `n` is the number of nodes.
    But in this scheme, if one node is added or removed, we have to calculare all the hash values again (because partition depends on `mod n`).
    To solve this problem, enters the **consistent hashing**. <br />

### Consistent Hashing
Think of some people sitting on a round table. And foods are placed randomly on the edge of the table.
Each person takes any food that served to his right but not taking another person's food. That's consistent hashing.
Think of eaters as nodes and foods are incoming data. Now, to ensure each eater gets roughly same ammount of food, we can have vertual clone of each eater distributed on the edge of the table. <br />

### Read Write Operation on Partitioned Data
Three numbers are important
**N** Number of replicas.
**R** Number of machines contained in read operation.
**W** Number of machines that have to be blocked in write operation.
In interest of *Read your own write* consistency, the following relation between these numbers are necessary.
                `R+W > N`
This scheme is neither immediately consistent nor isolated.
- If write operation is successful, that means, at least W nodes have executed the opetation.
- If write opetation fails, that means W nodes failed to execute the operation.
  - If at least one node is successfully executed, we can have eventual consistency
  - If no node is able to execute the write operation, client have to re issueing the write request to achieve consistency.

### Memebership Changes
Node canbe added into the system, or can leave from the system.
**If node joins**
1. The newly arriving node announces its presence and its identifier to adjacent nodes or to all nodes
via broadcast.
2. The neighbors of the joining node react by adjusting their object and replica ownerships.
3. The joining node copies datasets it is now responsible for from its neighbours. This can be done in bulk and also asynchronously.
4. If, in step 1, the membership change has not been broadcasted to all nodes, the joining node is now announcing its arrival.

**If node leaves**
1. System have to detect if a node is ubavailable.
2. Neighboring nodes have to share data among themselves.


## Storage Layout
1. Row-Based storage layout
2. Columnar Storage layout
3. Columnar Storage layout with Locality Groups
4. Log Structured Merge Tree (LSM-tree)

Id   | Pros | Cons | Usages 
---- | ---- | ---- | -------
1. | Comapact, Whole dataset can be read/write with a single IO, Good locality of access of different columns. | Operations on columns are expensive | When reading whole row is important 

<!-- 2. | Operations on columns are faster | Operations on row are expensive | Statistical models where column operations are important 
3. | Good segregation of physical storage. | probabilistic, not guranteed to work all the time | Google big table
4. | Can support diffrent data types; Practical; efficient | Need to merge frequently, otherwise aggragration results will be wrong | Elasticsearch  -->