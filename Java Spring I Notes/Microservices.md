
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
