## Origin
The term **NoSQL** first used in 1998 for a relational database that does not use SQL.
The term was picked up again in 2009 and used for conference for non relationl databases.

### Why Use NoSQL
- Features and strict ACID properties are not always needed for real applications.
- High Throughput
- Horizontal sclability and fault tolarance. (This thins are difficult to in RDBMS)
- Avoidance of expencive ORM. NoSQL saves data in easy to manipulatable format.
- Easy to setting up clusters. Ofrer sharding is a intrinsic feature.
- Modern applications often chosing performance over reliability.
- Requirements for cloud computing. Cloud want basically these 3 features
  - Data warehouse for large scale map/reduce jobs
  - Simple key/value store
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

