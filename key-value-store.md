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
Dynamo DB let users to configure the *replication factor* (default being 3). That menas keys and values of one node is replicated to the next 3 nodes in clockwise order.

