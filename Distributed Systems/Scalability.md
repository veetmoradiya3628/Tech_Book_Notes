- A scalable application can increase its capacity as its load increases.
- There are mainly two types of scalability possible
	- scale up - single node capacity increase
	- scale out - load to be divided on multiple nodes

- 3 categories of scalability patterns
	- functional decomposition
	- partitioning
	- duplication

- Functional decomposition is the process of taking as application and breaking it down into multiple individual parts.
- Partitioning is when a dataset no longer fits on a single node, it needs to be partitioned across multiple nodes.
- Duplication is easiest way to add more capacity to a service is to create more instances of it and have way of routing, or balancing, requests to them.
	- Stateless is easy to duplicate while stateful service duplication requires some sort of coordination.

###### Functional Decomposition

- Functional decomposition is that split monolithic backend to set of independently deployable services that communicates via APIs.
	![[Pasted image 20250106222423.png]]
- Benefits
	- Small services can be independently managed by small teams and requires less communication with each other which speeds up the development.
	- smaller codebase is easy to maintain and provides less overhead on IDEs.
	- independent service scale is possible
	- each service having its own independent data model and data store that benefit and allows developer to change its schema without affecting other services.
- Costs
	- Development experience
	- Resource provisioning
	- Communication
	- Continues delivery, integration and deployment
	- Operations
	- Eventual consistency
- Practical consideration is to start with monolith first approach and once demand / scale goes up then we could think of microservice boundaries at a time.

###### API Gateway
- once you have split an application into a set of services, each with its own API, you need to rethink how clients communicate with the application. A client might need to perform multiple requests to different services to fetch all the information it needs to complete a specific operation.
- we can solve this problem by adding a layer of indirection. the internal APIs can be hidden by a public one that acts as a facade, or proxy for the internal services.

	![[Pasted image 20250106224012.png]]
- The API Gateway provides multiple features, like routing, composition and translation
	- Routing
		- The API Gateway can route the requests it receives to the appropriate backend services. It does so with the help of a routing map, which maps the external APIs to the internal once.
	- Composition
		- some use cases might require stitching data back together frommultiple sources. The API gateway can offer a higher-level API that queries multiple services and composes their responses within a single one that is then returned to the client. This relieves the client from knowing which services to query and reduces the number of requests it needs to perform to get the data it needs.
	- Translation
		- The API gateway can translate from one IPC mechanism to another. For example, it can translate a RESTful HTTP request into an internal gRPC call.
- Cross-cutting concerns
	- Cache of frequently data at Gateway level
	- Authentication and Authorization at Gateway level
		- OpenID connect, OAuth, JWT etc
- API Gateway can be bottleneck since its an single point of failure.
- NGINX, Azure API Management etc tools for API Gateway.

###### CQRS
- API Gateway is in efficient if the composition requires large in-memory joins.
- CQRS - Command Query Responsibility Segregation
	- decoupling the read path from the write path.
	![[Pasted image 20250107235250.png]]

- **Messaging**
	- A form of indirect communication
	- point-to-point or publish-subscribe channel

- One Way messaging
	**![[Pasted image 20250107235606.png]]
- Request-response messaging
    ![[Pasted image 20250107235712.png]]

- Broadcast messaging
	![[Pasted image 20250107235816.png]]
- Exactly once processing is hard to achieve so try to keep messages idempotent.
- Failures
	- dead letter channel - a channel that acts as a buffer for messages that have been retried too many times.
- Backlogs
	- if more produces come online and consumer can't match their rate.
	- consumers became slow due to processing time increase of individual message.
- Fault Isolation
	- if there is a specific producer that emits poisonous messages that repeatedly fail to be processed can degrade the whole system and can potentially cause backlogs.
- Queue plus blob method for large BLOB object transmission.

###### Partitioning
- scale out or sharding, when dataset no longer fits on a single node, it needs to be partitioned across multiple nodes.
- shared KV-store.

- **Sharding strategies**
	- When a client sends a request to a partitioned data store to read or write a key, the request needs to be routed to the node responsible for the partition the key belongs to.
	- The mapping between key and partitions and other metadata is typically maintained in a strongly-consistent configuration store like etcd or Zookepeer.
	- partitioning methods
		- Range partition
		- hash partition

	- **Range based partition**
		- The data is split into partitions by key range in lexiographical order, and each partition holds a continues range of keys.
		- The data can be stored in sorted order on disk within each partition, making range scan fast.
		
		![[Pasted image 20250108224904.png]]
		- distribution of key may not be uniform.
	- **Hash Partitioning**
		- The idea behind hash partitioning is to use a hash function to assign keys to partitions. which shuffles - or uniformly distributes keys across partitions.
		
		![[Pasted image 20250108225345.png]]
		- modular hashing is problematic when a new partition is added, as all keys have to be reshuffled across partitions.
		- shuffling data is extremely expensive as it consumes network bandwidth and other resources from nodes hosting partitions.
		- ideally if new partitioning added then only K/N keys should be shuffled around, where K is the number of keys and N the number of partitions, A strategy that guarantees this property is called stable hashing.
		
	- **Ring hashing**
		- With ring hashing, a function maps a key to a point on a circle. The circle is then split into partitions that can be evenly or pseudo-randomly spaced, depending on the speciﬁc algorithm.
		- example, with consistent hashing, both the partition identiﬁers and keys are randomly distributed on a circle, and each key is assigned to the next partition that appears on the circle in clockwise order.
		
		![[Pasted image 20250108230058.png]]
	
	- **Rebalancing**
		- When no. of  requests to the data store becomes too large, or the dataset's size becomes too large, the no. of nodes serving partitions to be increased. similarly if the dataset's size keeps shrinking, the number of nodes can be decreased to reduce costs. The process of adding and removing nodes to balance the system's load is called rebalancing.
		- the rebalancing to be implemented in such as way that the amount of data transferred during the rebalancing act needs to be minimized.
		- Static partitioning
			- The idea is to create way more partitions than necessary when the data store is first initialized and assign multiple partitions per node.
			- this approach is more kind of tightly coupled and having trouble with elastic behavior.
		- Dynamic partitioning
			- The idea is to creating partitions upfront is to create them on demand.

###### Duplication

- Network load balancing
	- adding more services instances with shared data store case the data store can become a bottleneck.
	- The routing, or balancing of requests across a pool of servers is implemented by a network load balancer.
	- A load balancer (LB) has one or more physical network interface cards (NIC) mapped to one or more virtual IP (VIP) addresses.
	- distributing requests across servers has many benefits. because client can decoupled from servers and don't need to know their individual addresses, the number of servers behind the LB can be increased or reduced transparently.
	- A LB supports several core features beyond load balancing, like service discovery and health-checks.

- Load Balancing
	- The algorithms used for routing requests can vary from simple round-robin to more complex ones that take into account the server's load and health.
	- ping-pong effect , where the server alternates between very busy and not busy at all.
	- The ideal idea for routing is to consider combination of load metrics and power of randomness.

- Service discovery
	- SD is the mechanism used by the LB to discover the available servers in the pool it can route requests to.
	- there is benefit of using a dynamic service discovery mechanism compare to static config file.

- Health checks
	- Health checks are used by the LB to detect when a server can no longer serve requests and needs to be temporarily removed from the pool.
		- Two categories for this:
			- passive
			- active
	- A passive health check is performed by the LB as it routes incoming requests to the servers downstream. if server isn't reachable, the request timeout or the server returns a non-retriable status code then LB can decide to take that server out of the pool.
	- Instead an active health check requires support from the downstream servers which needs to expose a health check endpoint signaling the server's health check.

- DNS load balancing
	- The most basic form of load balancing can be implemented with DNS.
	- Transport layer load balancing
		- LB that works at TCP layer
		- Sits between client and server and work as an intermediator between them. (F5 Virtual pool & members kind of)
		
			![[Pasted image 20250110233620.png]]
		- As the data going out of the servers usually has a greater volume than the data coming in, there is a way for servers to bypass the LB and respond directly to the clients using a mechanism called direct server return.
		- L4 LBs generally don't support features that require higher-level network protocols, like terminating TLS connections or balancing HTTP sessions based on cookies.
	- Application Layer Load Balancer
		- HTTP reverse proxy
		- The LB receives http request from client, inspects it, and sends it to a backend server.
		- L7 LB supports the feature those are not supported by L4 LB.
			![[Pasted image 20250110234502.png]]
		- load balancing / proxy can be separated out as an internal separate process.
		- Popular sidecar proxy load balancers are NGINX, HA Proxy and Envoy.
	- Geo Load Balancing
		- When client and server are located in different area's of world at that time no matter how fast our server is, there will always be latency added due to physical location difference in the server and client, that time we need to setup a client to connect over a geo graphically nearer server which is called a Geo Load Balancing.
		
		![[Pasted image 20250112143101.png]]

- **Replication**
	- If server behind LB is stateless in that case its easy to add / remove servers but when its stateful in that case some sort of coordination is required for scale out / in.
	- Replication is the process of storing a copy of the same data in multiple nodes.
	- Replication and sharding are techniques that are often combined but are orthogonal to each other.
		
		![[Pasted image 20250112144134.png]]
	
	- Single leader replication
		- The most common approach to replicate data is the single leader, multiple followers / replicas approach.
			- In this approach the clients sends writes exclusively to the leader, which updates its local state and replicates the changes to the followers.
			- we can implement this using Raft replication algorithm easily.
		
		 ![[Pasted image 20250112144040.png]] 
		- Asynchronous replication
			- where data replication happens async way between leader and followers but the catch in this approach is that if leader crashes just after write and before replication then it will be eventual consistency and other one is that if client just after writing to one of the node and reading from older version of follower leads to the inconsistent data problem there.
		- Synchronous replication
			 - Synchronous replication waits for a write to be replicated to all followers before returning response to the client, which comes with a performance penalty.
			 - this approach can be bottleneck and performance can be down if replication is slow or if follower node is not available / not responding.
	 - Async and Sync both has its advantages and its disadvantages so most of the commercial database comes with combination of both approach for the replication.
	
	- Multi-leader replication
		- In multi-leader replication there is more than one node that can accept writes.
		- This approach is used when the write throughput is too high for a single node to handle, or when a leader needs to be available in multiple data centers to be geographically closer to its clients.
		- This strategy is mostly avoided due to its complexity and it requires high level of conflict resolution since due to multiple writers we need to implement a conflict resolution strategy.
		![[Pasted image 20250112170313.png]]
	- There are multiple ways to handle conflict resolutions.
	- we can leverage data structures that provides automatic conflict resolution like conflict-free replicated data type (CRDT).
	
	- Leaderless replication.
		- In this approach any replica could accept writes from clients, but in this case there will not be any leader for replicating and conflict resolution but that responsibility would be offloaded to the client.

- **Caching**
	- A cache is a high-speed storage layer that temporarily buffers responses from downstream dependencies so that future requests can be served directly from it. it's the form of best effort replication.
	- Policies
		- When a cache miss occurs, the missing data item has to be requested from the remote dependency and the cache has to be up-dated with it. 
			- side cache
			- inline cache
	- Since cache has max capacity there policy is needed to remove the entry from cache
		- LRU - least recently used entry
		- TTL - Time to leave (based on time)
	
	- **In-process cache**
		-  The simplest possible cache you can build is an in-memory dictionary located within the clients such as a hash table with a limited size and bounded to the available memory that the node offers.
		- it has its disadvantages and advantages.
	- **Out-of-process cache**
		- An external cache, shared across all service instances, addresses some of the drawbacks of using an in-process cache at the expense of greater complexity and cost.
	- cache introduces bi-modal behavior in the system.