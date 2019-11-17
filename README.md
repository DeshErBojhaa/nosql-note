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
Data versioning is important for optimistic concurrency control. Larest data version helpd database to reach to eventual consistancy.

### Timestamp:
Obvious solution, but it's difficult to have a consistent `clock` across the system.

### Optimistic Locking:
A unique counter will be saved with each piece of data. When a data is to update, client have to provide the counter number it seeks to update.
This mechanism does not work well when a lot of servers are sponing and destroying.

### Multiversioning Storage:
In this scheme, for a given row, multiple version can exist concurrently. This provides optimistic concurrency control with compare and swap mechanism. 

### Vectior Clocks:



## 3.2 Partitioning
When data is no longer persostable in one machine, we need to think about partitioning it. Depending on _dynamicity_(how often and dynamically storage joins and leaves the system), there are different approaches for this issue.
- Memory caches
- Clustering
- Seperating Reads from Writes
- Sharding