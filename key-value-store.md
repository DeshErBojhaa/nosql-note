# Key-Value Store
- Scalability over consistency
- No complex queries (no joins and aggregrations)
- Usually there is a upper bound in key length

## AWS Dynamo DB
#### Concepts applied at dynamo DB
- Consistent hashing with replication as partitioning scheme.
- Object stored are multi versioned.
- Every node has same responsibility. No distinguished nodes have special roles.

### Dynamo DB system design
**Table** AWS dynamo summary of techniques
| Problem | Technique | Advantages |
| :--- | :--- | :--- |
| Partitioning | Consistent hashing | Incremental Scalability |
| High availability for writes | Vector clocks with reconsiliations during reads | Version size is decoupled from update rates |
| Handling temporary failures | Sloppy Quorum abd hinted handoff | Provides high availability and durability despite some of the replica not available |
| Recovering from permanent failure | Anti-entropy using Merkle tree | Synchronizes divergent replicas in the background |
| Memnership and failure detection | Gossip based membership protocol and failure detection | Prevents symmerty and avoids having centralized registry for storing membership and node liveness information |

#### Partition Algorithm
Dynamo DB uses consistent hashing. But raw consistent hashing has 2 main problems
- Randome mapping of storage onto a ring can cause a unbalanced mapping
- It does not take into account of the node's physical ability.

To solve these two problems, Dynamo DB uses one or more virtual nodes for each real node. The number of virtual nodes is proportional to the physical ability of that node.

#### Replication
Dynamo DB let users to configure the *replication factor* (default being 3). That menas :old_key:s and values of one node is replicated to the next 3 nodes in clockwise order.

#### Execution of GET and PUT operations
Dynamo DB allows any node in the cluster to accept *read* or *write* request for any :old_key:. All nodes knows which node should take main responsibility for any incoming request.

To provide a consistent view to the client, Dynamo DB applies a quorum like consistency. For this quorum like system to work perfectly, the following condition must hold true.
                `R + W > N`
Where `R` is the least number of nodes to return a read request.
And `W` is the least numbrer of nodes to satisfy a write request.

Upon write requests the first answering node on the preference list (called coordinator in Dynamo) creates a new vector clock for the new version and writes it locally. Following this procedure, the same node replicates the new version to other storage hosts responsible for the written data item (the next top `N` storage hosts on the preference list). The write request is considered successful if **at least** `W − 1` of these hosts respond to this update.

Once a storage host receives a read request it will ask the `N` top storage hosts on the preference list for the requested data item and waits for `R − 1` to respond. **If these nodes reply with different versions that are in conflict (observable by vector-clock reasoning), it will return concurrent versions to the requesting client.**

