
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

