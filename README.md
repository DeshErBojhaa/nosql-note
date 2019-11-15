# ACID vs BASE
BASE -> Basically Available, Soft-state, Eventual consistency

## SOME CONSISTANCY TYPES

### Strict Consistancy: 
All write operations must return data from the latest completed write operation, regardless of which replica serves the query.

This can be achieved in 2 ways
- Use the same node for reading and writing
- Use two phase commit or Paxos

### Eventual Consistancy:
User may see different data in different time. But all the nodes will come to a
point where it shows the latest completed write data.

### Read Your Own Write (RYOW) Consistance:
Client sees his updates immediately after they hace issued an successful update. Regardless of which node client write and read from. Updates by other clients my not be visible to him instantly.

### Session Consistancy:
RYOW consistance, but active for only the session scope in which the write occurred.

### Casual Consistancy:
If one client reads version x and subsequently writes version y, any client reading version y will also read version x.

### Monotonic Read Consistancy:
Client will see updated data monotonicly.

