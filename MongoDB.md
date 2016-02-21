# MongoDB

## Replication
- Group on mongod instances
- Contains several data bearing nodes & optionnally 1 arbiter node
- Can support up to 50 members in total
- See https://docs.mongodb.org/manual/core/master-slave/ if your deployment requires more nodes
- Note: master-slave replication lacks the automatic failover capabilities

### Basics
#### Data bearing nodes

##### Primary
- Receives all write operations
- MongoDB applies write operations on it and then records the operations on the primary's oplog

##### Secondaries
- Replicate primary's oplog and apply the operations to their data set in an asynchronous process
- If the primary is unavailable, an eligible secondary will hold an election to elect itself the new primary
- Specific purpose configuration: https://docs.mongodb.org/manual/core/replica-set-secondary/

#### Arbiter node
- Arbiters do not keep a copy of the data and cannot become a primary
- Arbiters play a role in the elections that select a primary if the current primary is unavailable
- Can add a vote in elections : arbiters have exactly 1 vote election
- Allow replica sets to have an uneven number of members
- Respond to heartbeat and election requests by other replica set members
- Cheap resource cost: you may run an arbiter on an application server or other shared process
- Important : Do not run an arbiter on systems that also host the primary or the secondary members of the replica set
- Only add an arbiter to sets with even numbers of members
- If you add an arbiter to a set with an odd number of members, the set may suffer from tied elections
- See https://docs.mongodb.org/manual/tutorial/add-replica-set-arbiter/ to add an arbiter to a replica set

#### Automatic failover
- When a primary does not communicate with the other members of the set for more than 10 seconds
- An eligible secondary will hold an election to elect itself the new primary
- See https://docs.mongodb.org/manual/core/replica-set-elections/ for more details

#### Read Operations
- All members of the replica set can accept read operations
- By default, clients directs its read operations to the primary
- See https://docs.mongodb.org/manual/core/read-preference/Clients for changing the default read behavior

#### Requirements
- The minimum requirements: A primary, a secondary, and an arbiter
- Most deployments, however, will keep three members that store data: A primary and two secondary members

### Deployment strategies

#### Number of members
- Deploy an odd number of members (e.g. 1 primary, 2 secondaries)
- Ensures that the replica set is always able to elect a primary
- If you have an even number of members, add an arbiter to get an odd number

#### Consider fault tolerance
- See: https://docs.mongodb.org/manual/core/replica-set-architectures/#consider-fault-tolerance

#### Use hidden and delayed members for dedicated functions
- Dedicated functions such as backup or reporting
- See: https://docs.mongodb.org/manual/core/replica-set-architectures/#use-hidden-and-delayed-members-for-dedicated-functions

#### Load balance on read-heavy deployments
- In a deployment with very high read traffic, you can improve read throughput by distributing reads to secondary members
- As your deployment grows, add or move members to alternate data centers to improve redundancy and availability
- Always ensure that the main facility is able to elect a primary

#### Add capacity ahead of demand
- The existing members of a replica set must have spare capacity to support adding a new member
- Always add new members before the current demand saturates the capacity of the set

#### Distribute members geographically
- To protect your data if your main data center fails, keep at least one member in an alternate data center
- Set these members’ `members[n].priority` to 0 to prevent them from becoming primary

#### Keep a majority of members in one location
- When a replica set has members in multiple data centers, network partitions can prevent communication between data centers
- To ensure that the replica set members can confirm a majority and elect a primary, keep a majority of the set’s members in one location

#### Target operations with tag sets
- Use replica set tag sets to ensure that operations replicate to specific data centers
- Tag sets also allow the routing of read operations to specific machines
- See: https://docs.mongodb.org/manual/core/replica-set-architectures/#target-operations-with-tag-sets

#### Use journaling to protect against power failures
- Enable journaling to protect data against service interruptions
- Without journaling MongoDB cannot recover data after unexpected shutdowns, including power failures and unexpected reboots
- All 64-bit versions of MongoDB after version 2.0 have journaling enabled by default.

### Common architectures
- The standard deployment for production system is a three-member replica set: https://docs.mongodb.org/manual/core/replica-set-architecture-three-members/
- It provides redundancy and fault tolerance
- It avoids complexity when possible, but let your application requirements dictate the architecture
- More deployment patterns: https://docs.mongodb.org/manual/core/replica-set-architectures/#deployment-patterns

### High availability
- Failover allows a secondary member to become primary if the current primary becomes unavailable
- To support effective failover, ensure that one facility can elect a primary if needed
- Choose the facility that hosts the core application systems to host the majority of the replica set
- Place a majority of voting members and all the members that can become primary in this facility
- Otherwise, network partitions could prevent the set from being able to form a majority
