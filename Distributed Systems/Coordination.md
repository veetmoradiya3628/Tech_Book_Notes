- The end goal has always been to use multiple processes and services to build a Distributed application.
- some degree of coordination is needed to build a distributed application.

##### System models
###### Communication Links
- fair-loss link
	- it assumes that messages may be lost and duplicated.
- reliable link
	- it assumes that a message is delivered exactly once, without loss or duplication.
- authenticated reliable link
	- works same as reliable link but additionally assumes that the receiver can authenticate the message's sender.

###### Node failure
- arbitrary fault
	- Byzantine model
- crash recovery
	- model assumes that a node doesn’t deviate from its algorithm, but can crash and restart at any time, losing its in-memory state.
- crash stop
	- model assumes that a node doesn’t deviate from its algorithm, but if it crashes it never comes back online.

###### Timing Assumption
- synchronous model
	- model assumes that sending a message or executing an operation never takes over a certain amount of time.
- asynchronous model
	- model assumes that sending a message or executing an operation on a node can take an unbounded amount of time.
- partially synchronous
	- model assumes that the system behaves synchronously most of the time, but occasionally it can regress to an asynchronous mode.

##### Failure Detection
- **Ping** 
	- A ping is a periodic request that a process sends to another to check weather it's still available.
- **heartbeat**
	- A heartbeat is a message that a process periodically sends to another to inform it that it's still up and running.
- pings & heartbeats are typically used when specific processes frequently interact with each other, and an action needs to be taken as soon as one of them is no longer reachable. if that's not the case, detecting failures just at communication time is good enough.

###### Time
- The flow of execution of a single-threaded application is easy to grasp since every operation executes sequentially in time, one after the other.
- But in a distributed systems, there is no shared global clock that all processes agree on and can be used to order their operations. And to make matters worse, processes can run concurrently.

- **Physical clock**
	- clock drift, clock skew concept
	- **Atomic clock** based on quantum mechanical property.
	- **NTP** - Network Time Protocol is used to synchronize clocks.
	- **Monotonic clock**
		- A monotonic clock measures the number of seconds elapsed since an arbitrary point, like when the node started up, and can only move forward in time.

- **Logical clock**
- A clock that isn't tied to the physical concept of time, but captures the casual relationship between operations: a logical clock
- A logical clock measures the passing of time in terms of logical operations, not wall-clock time.
- simplest logical clock is a counter
- when one process sends a message to another a so called synchronization point.

![[Pasted image 20241228141033.png]]
- Lamport clock simulation
- The problem here is that operation P2,C happened before P3,E but still P2,C's timestamp is 3 > P3,E timestamp as 1, to solve this we have another logical clock called Vector clock.

- **Vector clock**
- A vector clock is implemented with an array of  counters, one for each process in the system. And similarly to how Lamport clocks are used, each process has its own local copy of the clock.
- vector clock simulation

![[Pasted image 20241228141817.png]]

##### Leader Election
https://www.geeksforgeeks.org/leader-election-in-system-design/

- Sometimes a single process in the system needs to have special powers, like being the only one that can access a shared resource or assign work to others.
- A leader election algorithm needs to guarantee that there is at most one leader at any given time and that an election eventually completes.

- **Raft Election Algorithm**
	- Three stages of process
		1. follower state - process recognizes another one as the leader
		2. candidate state - process starts a new election proposing itself as a leader
		3. leader state - process is the leader
	- Three things can happens
		- process win's the election
		- another process win's the election
		- some time goes by with no winner.
- RAFT's algorithm logic represented as a state machine.
![[Pasted image 20241229203650.png]]

- fencing token
	- https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html
- optimistic locking
- Leader election algorithms
	- Bully algorithm
	- Ring algorithm
	- Paxos
	- Raft

##### Replication

- Data replication is a fundamental building block of distributed systems.
- Raft's replication algorithm
- State machine replication

![[Pasted image 20250101220105.png]]
- Leader's log replicated to followers
- if there are 2f+1 followers, the system can tolerate up to f failures. 
- If leader fails then for new election, other processes logs needs to be up to date in-order to participate in election and to be a leader.
- To determine which two processes logs is more up-to-date, the index and term of their entries are compared.
	- if the logs ends with different terms, the log with the later term is more up-to-date.
	- If the logs ends with same term then whichever log is longer is more up-to-date.
- What if follower fails ?
	- the leader will retry sending it indefinitely.
	- What happens when a follower that was temporarily unavailable comes back online ?
		- The resurrected follower will eventually receive an AppendEntries message with a log entry from the leader. The AppendEntries message includes the index and term number of the entry in the log that immediately precedes the one to be appended. If the follower can’t ﬁnd a log entry with the same index and term number, it rejects the message, ensuring that an append to its log can’t create a hole. It’s as if the leader is sending a puzzle piece that the follower can’t ﬁt in its version of the puzzle.
		- When the AppendEntries request is rejected, the leader retries sending the message, this time including the last two log entries — this is why we referred to the request as AppendEntries, and not as AppendEntry. This dance continues until the follower ﬁnally accepts a list of log entries that can be appended to its log without creating a hole. Although the number of messages exchanged can be optimized, the idea behind it is the same: the follower waits for a list of puzzle pieces that perfectly ﬁt its version of the puzzle.

- **Consensus**
	- its a process of set of processes to agree on a value in a fault-tolerant ways.
	- Any problem that requires consensus can be solved with state machine replication too.
	- etcd and ZooKeeper implements state machine replication and exposes APIs on top of it.

- **Consistency models**
	![[Pasted image 20250101222038.png]]
- Only write has to go through leader in distributed env. but read can be done from leader, follower or combination of both.
- In this environment there might be a change that client reading from follower whose legging behind leader might see a different view of a system, which can cause a huge consistency problem.

- Strong consistency
	- Read only from leader, not so good idea
	![[Pasted image 20250101222652.png]]
	- linearizability or strong consistency model

- Sequential Consistency
	- Read from leader or follower, but at the cost of different view of a system.
	![[Pasted image 20250101222942.png]]

- Eventual Consistency
	- The only guarantee the client has is that eventually, all followers will converge to the ﬁnal state if the writes to the system stop. This consistency model is called eventual consistency.
	- An eventually consistent store is perfectly ﬁne if you want to keep track of the number of users visiting your website, as it doesn’t really matter if a read returns a number that is slightly out of date. But for a payment processor, you deﬁnitely want strong consistency.

- **CAT Theorem**
	- This concept is expressed by the CAP theorem, which can be summarized as: “strong consistency, availability and partition tolerance: pick two out of three.” In reality, the choice really is only between strong consistency and availability, as network faults are a given and can’t be avoided.

- **Practical Considerations**
	- To provide high availability and performance, off-the-shelf distributed data stores — sometimes referred to as NoSQL stores come with counter-intuitive consistency guarantees. Others have knobs that allow you to choose whether you want better performance or stronger consistency guarantees, like Azure’s Cosmos DB and Cassandra. Because of that, you need to know what the trade-offs are.
	- https://docs.microsoft.com/en-us/azure/cosmos-db/consistency-levels
	- https://docs.datastax.com/en/cassandra-oss/3.0/cassandra/dml/dmlConfigConsistency.html

##### Transactions
- 