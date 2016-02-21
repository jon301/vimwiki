# MongoDB

## Replication
- Group on mongod instances
- Contains several data bearing nodes & optionnally 1 arbiter node
- Can support up to 50 members in total
- See https://docs.mongodb.org/manual/core/master-slave/ if your deployment requires more nodes
- Note: master-slave replication lacks the automatic failover capabilities

### Data bearing nodes

#### Primary
- Receives all write operations
- MongoDB applies write operations on it and then records the operations on the primary's oplog

#### Secondaries
- Replicate primary's oplog and apply the operations to their data set in an asynchronous process
- If the primary is unavailable, an eligible secondary will hold an election to elect itself the new primary
- Specific purpose configuration: https://docs.mongodb.org/manual/core/replica-set-secondary/

### Arbiter node
- Arbiters do not keep a copy of the data and cannot become a primary
- Arbiters play a role in the elections that select a primary if the current primary is unavailable
- Can add a vote in elections : arbiters have exactly 1 vote election
- Allow replica sets to have an uneven number of members
- Respond to heartbeat and election requests by other replica set members
- Cheap resource cost
- Important : Do not run an arbiter on systems that also host the primary or the secondary members of the replica set
- Only add an arbiter to sets with even numbers of members
- If you add an arbiter to a set with an odd number of members, the set may suffer from tied elections
- See https://docs.mongodb.org/manual/tutorial/add-replica-set-arbiter/ to add an arbiter to a replica set

### Automatic failover
- When a primary does not communicate with the other members of the set for more than 10 seconds
- An eligible secondary will hold an election to elect itself the new primary
- See https://docs.mongodb.org/manual/core/replica-set-elections/ for more details

### Read Operations
- All members of the replica set can accept read operations
- By default, clients directs its read operations to the primary
- See https://docs.mongodb.org/manual/core/read-preference/Clients for changing the default read behavior

### Replica set requirements
- The minimum requirements: A primary, a secondary, and an arbiter
- Most deployments, however, will keep three members that store data: A primary and two secondary members

