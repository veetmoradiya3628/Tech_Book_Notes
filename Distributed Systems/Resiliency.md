- As we scale out our application, any failure that can happen will eventually happen. Hardware failure, software crashes, memory leaks etc.

###### Common Failure causes
- To protect systems against failures, we first need to have idea of what can go wrong. The most common failures you will encounter are caused by single points of failure, the network being unreliable, slow processes and unexpected load.

- **Single Point Of Failure**
	- A single point of failure is the most glaring cause of failure in a distributed system.
		- Ex. centralized configuration service, gateway service with https and TLS.
	- Single points of failure should be identified when the system is architected before they can cause any harm.
	- The best way to detect them is to examine every component of the system and ask what would happen if that component were to fail.

- **Unreliable network**
	- When client makes a remote call it sends a request to a server and expects to receive a response from it a while later. In the best case the client receives a response shortly after sending the request, but what if the client waits and waits and still doesn't get a response ?
	- there can be any network or system failure for the failure or there the other server might be slow to process the request.

- **Slow processes**
	- From observer's POV, a very slow process isn't very different from one that isn't running at all - neither can perform useful work.
	- Resource leaks are one of the most common causes of slow processes.
	- Memory is the most well-known source of leaks.
	- The constant paging and the garbage collector eating up CPU cycles make the process slower.
	- The process can leaks the following resource if not utilized well, memory, threads and sockets.

- **Unexpected load**
	- Every system has a limit to how much load it can withstand without scaling.
	- One thing is an organic increase in load that gives you the time to scale out your service accordingly, but another is a sudden and unexpected spike.
	- traffic due to seasonable access etc. we need to have some prevention or detection mechanism to take a proper action if some incident happens.

- **Cascading failures**
	- cascading failure, which occurs when a portion of an overall system fails, increasing the probability that other portions fail.
	- let say there are two replicas of DB sitting behind LB and serving 50 TPS traffic each one of them, now suddenly replica B becomes unavailable due to network failure or something then LB will start transferring all traffic of B to replica A which doubles the traffic on replica A but now replica A is performing out of its capacity and then after some time replica A will crash since its on load beyond its capacity, now by that time if replica B becomes available then all the sudden traffic comes to B which will make replica B crash, this loop will continues for some iterations.
	- there are some patterns to handle cascading failures.

- **Risk Management**
	- out of all possible failures, we can not handle all at the time since we have very less time in a day and engineering efforts to be put in proper direction so at that time risk management will come in picture to prioritize which one to handle at the time.
	
		![[Pasted image 20250114135509.png]]
	- To address a failure, you can either find a way to reduce the probability of it happening, or reduce its impact.

###### Downstream resiliency
- patterns to shield service against failures in its downstream dependencies

- **Timeout**
	- When you make a network call, you can configure a timeout to fail the request if there is no response within a certain amount of time.
	- you also have to have good monitoring in place to measure the entire lifecycle of your network calls, like the duration of the call, the status code received and if a timeout was triggered.

 - **Retry**
	 - When request timeouts at that time the client has two options at that point: it can either fail fast or retry the request at a later time.
	 - Exponential backoff
		 - To set the delay between retries, you can use a capped exponential function, where the delay is derived by multiplying the initial back-off duration by a constant after each attempt, up to some maximum value the cap.
		 ![[Pasted image 20250114142237.png]]
	- you should not retry a network call that isn't idempotent or not short leaved.
	- Retry amplification, jitter.
		 **![[Pasted image 20250114142603.png]]**

- **Circuit breaker**
	- A circuit breaker's goal is to allow a sub-system to fail without bringing down the whole system with it.
	- Unlike retries, circuit breakers prevent network calls entirely, which makes the pattern particularly useful for long-term degradations. In other words, retries are helpful when the expectation is that the next call will succeed, while circuit breakers are helpful when the expectation is that the next call will fail.
	
	- State machine
	
		![[Pasted image 20250114142927.png]]

###### Upstream resiliency
- upstream resiliency discuss on protest against upstream pressure from external systems

- **Load shedding**
	- Server has very little control over how many requests it receives at any given time, which can deeply impact its performance.
	- When a server operates at capacity, there is no good reason for it to keep accepting new requests since that will only end up degrading it. in that case process can start rejecting new service with 503 response and it can focus on already processing ones.
	- so this is Load shedding

- **Load leveling**
	- Load leveling is an alternative to load shedding, which can be used when clients don't expect a response within a short time frame.
	- The idea is to introduce a messaging channel between the clients and the service.
	![[Pasted image 20250115233206.png]]
	- Load-shedding and load leveling don't address an increase in load directly, but rather protect a service from getting overloaded.
	- This is why these protection mechanism are typically combined with auto scaling, which detects that the service is running hot and automatically increases its scale to handle the additional load.

- **Rate-limiting**
	- Rate-limiting or throttling is a mechanism that rejects a request when a specific quota is exceeded.
	- Quotas are typically applied to specific users, API keys or IP addresses.
	- most common response for HTTP client is 429 (Too many requests) with header as Retry-After: seconds so that will indicate a client that its limit is reached and it should retry after given time.
	-  Rate-limiting is also used to enforce pricing tiers.

- **Single-process implementation**
	- implementing rate limiting is interesting in its own pace.
	- one way is that we can store doubly linked list with all different API keys for particular time duration and increase a size of list by 1 node as request arrives with timestamp, if new request comes and the no. of nodes belongs to the range timestamp exceeds then it has a problem in it, so this approach works but its memory consuming since we are storing doubly linked list with all different API keys.
	- other way is to implement with bucket solution (https://www.geeksforgeeks.org/token-bucket-algorithm/)

- **Distributed Implementation**
	- if we have more than one instance of service in that case local state will not work and there should be some way to store it externally and shared across all the instances of application.
	- every request updating a state of value in database is expensive operation so we are kind of doing it with batch processing and using single atomic get-and-increment and other one is compare-and-swap.
	
		![[Pasted image 20250115235329.png]]

- **Bulkhead**
	- The goal of the bulkhead pattern is to isolate a fault in one part of a service from taking the entire service down with it. this comes from ship's hull concept.
	- When everything else fails, the bulkhead pattern provides guaranteed fault isolation by design. The idea is to partition a shared resources, like pool of service instances behind a load balancer, and assign each user of the service to a specific partition so that its requests can only utilize resources belonging to the partition its assigned to.
		
		![[Pasted image 20250116000038.png]]
	- scaling is much harder when instances are partitioned so be careful when using bulk head patten. (https://www.geeksforgeeks.org/bulkhead-pattern/)

- **Health endpoint**
	- some mechanism to indicate that server is all good and will be able to process the requests. if health check fails then it will be removed from pool of server and no more requests will come to the server.
		- liveness health test
		- local health test
		- dependency health check

- **Watchdog**
	- background threaded implementation to periodic check on system's memory and other resources which will be available when there is any crashes, that will help to identify the issue.
	- point to consider
		- there are other processes that are identical to the one that crashed that can handle incoming requests.
		- requests are stateless and can be served by any process.
		- any non-volatile state is stored on a separate and dedicated.
		- data store so that when the process crashes its state isn’t lost.
		- all shared resources are leased so that when the process crashes, the leases expire and the resources can be accessed by other processes.
		- the service is always running slightly over-scaled to with stand the occasional individual process failures.

