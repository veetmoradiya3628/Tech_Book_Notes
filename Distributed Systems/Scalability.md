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

