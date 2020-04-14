## Origin
The term **NoSQL** first used in 1998 for a relational database that does not use SQL.
The term was picked up again in 2009 and used for conference for non relationl databases.

### Things to consider choosing a database
Choosing database is no simple task. Specially at the early stage of the project when requirements are not
well understood. But if we analyze what little requirements we have, we can mitigare the pain of choosing a wrong database.

Things to Consider When Choosign a New DB
| Topic | Description |
| :--- | :--- |
| Data type/ Data structure | If data is highly structured or not. How often data are updating |
| Data Volume | Do we need horizontal of vertical scalability? How fast we need to scale up. |
| Write Frequency | If the write requests are append only? What is the percentage of write queries are update requests. |
| Read Frequency | What kinnd of queries we are doing? Range queries based on time? Lots of aggregration? Adhoc queries? Very structured queries? |
| Data Consistency | ACID vs BASE |
| Hosting | Self, Cloud, DB as service |
| Cost | :moneybag: |
| Integration Constraints| How easy it's for your infra and devs to start with this DB. How much resource available online. |

### Problems With Relational DB
- It's too structured. if you don't knnow the structure of the data precisely up front or the data is unstructured or semi
structured, this may cause problem.
- Requires complicated ETL process.
- Pessimistic concurrency control leads to often row locking. Which can block read (or write) operations for undeterministic time.

### Why Use NoSQL
- Features and strict ACID properties are not always needed for real applications.
- High Throughput
- Horizontal sclability and fault tolarance. (This thins are difficult to in RDBMS)
- Avoidance of expencive ORM. NoSQL saves data in easy to manipulatable format.
- Easy to setting up clusters. Ofrer sharding is a intrinsic feature.
- Modern applications often chosing performance over reliability.
- Requirements for cloud computing. Cloud want basically these 3 features
  - Data warehouse for large scale map/reduce jobs
  - Simple key/value :department_store:
  - More features than key/value (Document database for example) <br />
For these things NoSQL is better match than RDBMS.
- Less overhead and memory footprint than RDBMS.
- WEB friendly (RPC call access and so on).
- Optional forms of data queries.

**Side note**: When people build dustributed application, they have the following 8 assumptions, which are wrong and cause a lot of pain down the road.
1. The network is reliable
2. Larency is zero
3. Bandwidth is infinite
4. The network is secure
5. Topology does not change
6. There is one Admin
7. Transport cost is zero


## Classifiacation and Comparison of NoSQL Databases
**By data model**

| Category | Matching Database |
| :--- | :--- |
| :old_key: - :spiral_notepad: :department_store: | Redis <br> Voldemort <br> Riak |
| Document :department_store: | MongoDB <br> Elasticsearch <br> CouchDB |
| Extensible Record :department_store: | Bigtable <br> HBase <br> Cassandra |

**More find grainde caregorization**

| Term | Matching Databases |
| :--- | :--- |
| :old_key: - :spiral_notepad: cache | Memcached <br> Velocity <br> Repcached |
| :old_key: - :spiral_notepad: :department_store: | keyspace <br> Flare |
| Eventually Consistent :old_key: - :spiral_notepad: :department_store: | Dynamo <br> Voldemort <br> Mo8onDb |
| Ordered :old_key: - :spiral_notepad: :department_store: | Light Cloud <br> MemcacheDB |
| Data structure server | Redis |
| Tuple :department_store: | Apache river |
| Object Database | ZopeDB |
| Document :department_store: | CoucnDB <br> Mongo <br> Elasticsearch (it not quite a documet store, it's more like a seacrch engine that is optimized for full text searching) |
| Wide Column :department_store: | Bigtable <br> Hbase <br> Cassandra <br> KDI |

**Comparison between different databases**

| Type | Performance | Scalability | Flexibility | Complexity | Functionality |
| :--- | :---: | :---: | :---: | :---: | :---: |
| :old_key: - :spiral_notepad: :department_store: | :arrow_up: | :arrow_up: | :arrow_up: | :negative_squared_cross_mark: | :wavy_dash:	 (:negative_squared_cross_mark:) |
| Column :department_store: | :arrow_up: | :arrow_up: | moderate | :arrow_down: | minimal |
| Document :department_store: | :arrow_up: | :wavy_dash:	( :arrow_up: ) | :arrow_up: | :arrow_down: | :wavy_dash:	 ( :arrow_down: ) |
| Graph Database | :wavy_dash:	 | :wavy_dash:	 | :arrow_up: | :arrow_up: | graph theory |
| Relational Database | :wavy_dash:	 | :wavy_dash:	 | :arrow_down: | moderate | relational algebra |

### Comparison by Scalability, Data and Query Model, Persistance Design
**Scalability**
Read scalability is trivial. Write scalability is what matters. In that perspective, we can categorize NoSQL data bases in 2 main categories.
- Support for multiple datacenter
- Possibility to add machine live on an existing cluster.

Most modern NoSQL data bases support those features out of the box.

**Data and Query Model**

**Persistence Design**

| Database | Persostence design |
| :--- | :--- |
| Cassandra | Memtable / SSTable |
| HBase | Memtable/SSTable on HDFS |
| Elasticseach | Mostly Memtable/SSTable |
| CouchDB | Append-only B-Tree |
| MongoDB | B-Tree |
| Neo4j | On-disk linked list |
| Redis | In-memory with background snapshot |

**Memtable/SSTable scheme**
Write operations are buffered in memory in a *Memtable*. After they are written to a append only commit log to ensure durability. After a certain amount of time, the memetable flushed into the disk as a whole.
Elasticsearch does something similar with theie segments.
