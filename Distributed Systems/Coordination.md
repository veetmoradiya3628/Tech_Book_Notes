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
- Sometimes a single process in the system needs to have special powers, like being the only one that can access a shared resource or assign work to others.
- A leader election algorithm needs to guarantee that there is at most one leader at any given time and that an election eventually completes.

- **Raft Election Algorithm**
	- 