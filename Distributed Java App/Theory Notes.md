
##### Overview
- Where can we find Distributed Systems ?
	- Distributed systems are everywhere!
	- Every time we:
		- watch a movie on demand
		- shop online
		- order a ride share service through our mobile phone
		- search for something online
- Why Distributed Systems ?
	- Those companies are running highly scalable distributed systems in order to:
		- handle millions of users
		- Petabytes of data
		- Provide consistent user experience
- The Cloud and Distributed Systems
	- Even the simplest web site is running on a distributed system
	- Cloud (AWS, Azure, GCP,...) is a distributed system designed for companies and SW developers
- The virtues of a Distributed System
	- You are not aware of the complexities of the system
	- It just works!
	- Feels like a single machine
	- Dedicated specifically for you
- The problem with centralized systems
	- Vertical scaling limited
	- Single point of failure
	- High latency - poor user experience
	- Security and privacy
	- Solution :- Distributed system
- Definition :- 
	- *A Distributed system is a system of several processes running on different computers, communicating with each other through the network, and are sharing a state or are working together to achieve a common goal*
- No common goal, not a distributed systems

##### Cluster Coordination Service and Distributed Algorithms
- Node
	- A process running on a dedicated machine
- Cluster
	- Collection of computers / nodes connected to each other.
- Challenges of Master-Workers architecture
	- Automatic and system leader election is not a trivial task to solve, even among people.
	- Arriving to an agreement on a leader in a large cluster of nodes is even harder.
	- By default each node knows only about itself - service registry and discovery is required
	- Failure detection mechanism is necessary to trigger automatic leader reelection in a cluster
- Apache Zookeeper
	- high performance coordination service designed specifically for distributed systems.
- Zookeeper abstraction and data model
	- Znodes
		- Persistent 
		- Ephemeral
- Leader election algorithm

- Install, setup and use zookeeper command line client tool for visualization and debugging
- Zookeeper threading model for client library
	- Two threads gets created
		- Event thread
		- IO thread

- Watchers and triggers
	- The watcher allows us to get a notification when a change happens.
- The Herd Effect
	- Try to avoid having this in our algorithm
- Failure detection with Zookeeper
- Leader Re-election Algorithm
- Use subscription, event driven architecture to detect failures in the cluster
- Fault tolerance
	- Re elect a new leader automatically
- Should be horizontal scalable
	- Grow on demand


##### Hands on
- Election and re election algorithm in distributed systems
	- Watch predecessor znode algorithm
- Auto-header using Zookeeper

