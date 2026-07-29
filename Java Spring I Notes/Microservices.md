
- Communication between 2 services
	- Synchronous
	- Asynchronous
- Synchronous communication
	- client waits for the response from server before continuing
	- blocking in nature, means thread waits till response is not received
	- Sync communication types in spring boot
		- RestTemplate
		- RestClient
		- FeignClient

#### RestTemplate
- Abstract low level code like creating HttpURLConnection object etc
- Traditional / legacy way to call REST APIs in spring application
- GET
	- getForObject
	- getForEntity
- POST
	- postForObject
	- postForEntity
- PUT
	- put
- DELETE
	- delete
- GENERAL PURPOSE
	- exchange
	- execute
- Not used any more like its lagecy & this is in maintenance mode as of now
- WebClient
	- Async / non-blocking in nature
	- do not wait for response from server
	- introduced in spring webflux for reactive programming
#### RestClient
- Introduction of Fluent, builder-style API (more readable and user friendly way of configuring the endpoint)
- RestClient supports easy integration with interceptors, filters etc
- Fluent API means chaining of method calls
- Each method call returns an object (of next step) that exposes next set of operations (methods)

```
RestClient restClient = RestClient.create();

String response = restClient
	.get()
	.uri("http://localhost:8082/products/" + id)
	.accept(MediaType.APPLICATOIN_JSON)
	.header("X-Custom-Header", "xyz")
	.retrive()
	.body(String.class)
```
- Exception handling supported pretty well
- Custom interceptor can be added

#### FeignClient
- Feign is a Declarative HTTP client developed by Netflix
- Declarative means we tell what to do, not how to do
- In spring boot feign client available via Spring Cloud OpenFeign library
- Dependencies to be added
- @EnableFeignClients annotation is required to tell spring to scan for interfaces annotated with @FeignClient
- Encoder and Decoder in Feignclient
	- Encoder - Converts a Java Object into a request body 
	- Decoder - Converts the HTTP response body into Java Object
- ErrorDecoder
	- its used to handle non 2xx status codes like 4xx and 5xx
- Retry configuration
- Feignclient naming configuration

#### Service Discovery
- In microservices, services or component instances are created and deleted dynamically
- we can not hardcode the URL of a particular instance, its not scalable and feasible
- The problems with hardcoding
	- Single point of failure
	- No Load balancing
	- Tight coupling
	- Difficulty in testing
- Solution for this 
	- Service Discovery (like Eureka)
		- Eureka Server
			- Act like a phonebook
			- Has all the instances info for all the registered clients like
				- Service name
				- Instance id
				- IP
				- port number
				- Health Status
		- Eureka Client
			- Register itself with the server
			- Discovers an instance of other service via Eureka Server
- Dependency `spring-cloud-starter-netflix-eureka-server` for server
- `@EnableEurekaServer` - Tells spring boot to create necessary beans, which is required for Eureka Server like
	- EurekaController
	- Dashboard etc
- Dependency `spring-cloud-starter-netflix-eureka-client` for client
	- client register to target eureka server based on config in application.properties
- Client heart beat to eureka server
- Eureka server only stores the data in memory - Map\<String, Lease\<InstanceInfo>>
- Eureka server is single point of failure so we should have replicas for the same and it should be multi node cluster configured
- Local cache and its tradeoff in client side 

#### Load balancer
- Helps in distributing traffic to multiple instances of a servers
- Helps in preventing single server from being overloaded with huge traffic
- Load balancer types
	- Server side
		- Centralized load balancer like Nginx, ELB etc
		- Like a separate microservice
	- Client side
		- Load balancing capability present inside the client (the caller) or it uses a library to take the decision
		- Like spring cloud LoadBalancer, Netflix Ribbon, Istio with sidecar etc
- Client side load balancing with RestTemplate/FeignClient
- Spring Cloud Load Balancer dependency 
- Two algorithms
	- Round Robin
	- Random
- Other algorithms like
	- Weighted
	- Least connection
- Istio with Side car

#### Fault-Tolerance Microservices
- Fault tolerance microservice is a service, which continues to work even when downstream system fails
- Instead of crashing or cascading failures, it handles the failure gracefully
- Resilience4j
	- Retry
	- Circuit breaker
	- Rate limiter
	- Bulk head
	- Time Limiter
- Rate Limiter
	- It controls the number of requests allowed to a microservice in a given time window
	- So in other words, it protects our system from sudden traffic spike (like in DDoS attack)
	- RateLimiter
		- Fixed window counter
			- Counts how many request happens in a fixed window
			- If the count crossed the Limit, then reject the request
		- Sliding log
		- Sliding window counter
		- Token Bucket
		- Leaky Bucket 
- dependency `resilience4j-spring-boot3`
- Internally uses AOP functionality
- `@RateLimiter` annotation can be used to implement this Rate limiting which default set and use Token bucket algorithm

- Bulkhead
	- It helps to control how many concurrent requests can go to downstream service
	- Semaphore bulkhead
	- It also protects our application from our downstream services by limiting how many threads we allocate to them
	- Bulkhead
		- Semaphore bulkhead
			- Limits the number of concurrent calls using counter
			- If the limit is reached, further calls are rejected immediately or blocked for specific wait time
		- Thread pool bulkhead
- Time Limiter
	- Time limiter is used to prevent async call from hanging indefinitely
	- Time limiter is non blocking in Resilience4j
	- Means it is mainly designed for asynchronous operations, that returns a reactive type like Mono, Flux etc
- Retry
	- In distributed system, call to downstream service might fail because of Transient issues
	- Transient issues means short temporary issues like network issue, timeouts etc.
	- And retry of same request after a short delay can get succeed
	- Idempotency
	- Types of retry
		- Fixed Interval
		- Exponential backoff
		- Exponential backoff + Jitter
		- Custom Interval
	- AOP based

- Circuit Breaker
	- This pattern prevents an application to make repeated calls to a downstream service that is likely to fail
	- It prevents application to make repeated calls to a downstream service that is likely to fail
	- State of Circuit Breaker
		- Closed
			- All calls are allowed to downstream service
		- Open
			- No calls allowed to downstream service and fail the call immediately
		- Half Open
			- Allows only limited number of test calls to downstream and keep track of their success rates

![[Pasted image 20260729195303.png]]


- API Gateway
	- It provides a Single entry point to access all microservices
	- It provides lot of benefits like
		- Routing - forward requests to right microservice
		- Load balancing
		- Authentication like JWT
		- Rate limiting
		- Resilience features (Circuit breaker, retry etc)
		- Request/Response transformation
		- Monitoring and logging
	- dependency `spring-cloud-starter-gateway`
	- client side load balancing with API gateway

- Centralized Configuration
	- If config are kept within microservice resource (application.properties) then any change means
		- Edit application.property file
		- Rebuild the JAR
		- Redeploy service
	- Inconsistent config across services
	- No runtime update
	- Time consuming rollback
- Centralized configuration with spring cloud config
	- Git repository to store config files for multiple microservices
	- global common properties
		- application.properties
		- application-{profile}.properties
	- dependency `spring-cloud-config-server`
	- @EnableConfigServer
	- using this concept we no need to restart server when config is updated it will refresh runtime with annotations like @RefreshScope etc

- Actuator
	- Provides production-ready endpoints to monitor and manage the spring boot application
	- /health
	- /metrics
	- /metrics/\<metricname>
	- There are lot of metrics and we can override the behaviors based on config 
	- Few important metrics
		- JVM Memory metrics
			- jvm.memory.used
			- jvm.memory.max
		- Garbage collection metrics
			- jvm.gc.pause
		- Threads
			- jvm.threads.live
			- jvm.threads.peak
		- System metrics
			- system.cpu.usage
		- HTTP Server / Requests
			- http.server.requests
		- Database / JDBC metrics
			- jdbc.connections.active
			- jdbc.connections.idle
			- jdbc.connections.max
- GET /threaddump
	- Helps to diagnose deadlock or thread leaks
	- which threads are active, blocked or waiting
- Additional endpoints
	- /heapdump
	- /mappings
	- /beans
	- /configprops
	- /loggers
	- /shutdown
	- /env
	- /actuator/env/{property}
- Custom actuator endpoint
	- class annotated with @Endpoint(id = "custom endpoint name")
	- @ReadOperation
	- @WriteOperation
	- @DeleteOperation
- These metrics data can be pushed to datadog (monitoring platform)
- we can also push to different other platforms like: Prometheus, CloudWatch etc

