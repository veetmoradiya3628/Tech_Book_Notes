
### Chap 1. Introduction to Cloud Native

- A distributed system is a system in which individual computers are connected through a network and appear as a single computer.
	- Network is not so reliable every time and network failure needs to be consider.
	- Latency needs to be considered in case of distributed systems world.
	- There is limited network bandwidth and its not infinite.
	- network is not secure at all.
	- The topology is not static at all in case of distributed computing.
	- There is no any single administrator in case of cloud native distributed systems.

- CAT Theorem
	- Consistency, Availability vs. Isolation

- The Twelve-Factor App
	- The Twelve-Factor app methodology can be considered the foundation for cloud native applications and was first introduced by engineers at Heroku.
	1. Codebase
		- One codebase tracked in revision control; many deploys.
	2. Dependencies
		- Explicitly declare and isolate dependencies.
	3. Configuration
		- Store configuration in the environment.
	4. Backing Services
		- Treat backing services as attached resources.
	5. Build, Release, Run
		- Strictly separate build and run stages.
	6. Processes
		- Execute the app in one or more stateless processes.
	7. Data Isolation
		- Each service manages its own data.
	8. Concurrency
		- Scale out via the process model.
	9. Disposability
		- Maximize robustness with fast startup and graceful shutdown.
	10. Dev/Prod Parity
		- Keep development, staging and production as similar as possible.
	11. Logs
		- Treat logs as event streams.
	12. Admin processes
		- Run admin and management tasks as one-off processes.

- SLAs are important and defined in terms of 9s availability.

### Chap 2. Fundamentals

- Containers
	- The initial idea of containers was to slice up an OS so that you can securely run multiple applications without them interfering with one another.
	- The required isolation is accomplished through namespaces and control groups, which are linux kernal features.
	- Containers are encapsulated, individually deployable components running as isolated instances on the same kernal with virtualization happening on the OS level.
	- Container uses copy-on-write file system to share data across multiple containers.
	- VM isolation softwares and available famous isolation software provided by famous public cloud providers.

- Container Orchestration
	- Container orchestration is needed on large scale container architecture and in those case, management of those many no. of containers become very difficult that's when container orchestrator comes to save our life and by far the most famous tool for that is K8s.

- Kubernetes Overview
	- Kubernetes master node components :
		- kube-apiserver
		- etcd
		- kube-scheduler
		- kube-controller-manager
		- cloud-controller-manager
	- Kubernetes node components :
		- kubelet
		- kube-proxy
		- container runtime
	- Fundamental concepts of k8s : 
		- Pods
		- Services
		- Replica Sets
		- Deployments

- Kubernetes and Containers
	- CRI - Container Runtime interface, deals with the runtime support for container life cycle in k8s
	- OCI - The OCI is a Linux foundation project that aims to design open standards for container images and runtimes.

- Serverless Computing
	- Serverless computing means that scale and the underlying infrastructure is managed by the cloud provider; that is your application automatically drives the allocation and deallocation of resources and you do not need to worry about managing the underlying infrastructure at all.
	- it works based on event driven programming model & pay only per execution model.

- Functions
	- When talking about functions, people typically talk about FaaS offerings such as AWS Lambda, Azure Functions, and Google cloud functions, which are implemented on serverless infrastructure.
	- From a development perspective, a function is the unit of work, which means that your code has a start and a finish. Functions are usually triggered by events that are emitted by either other functions or platform services.
	
- Faas Vs. Containerized Services

| FaaS                              | Containerized Services                     |
| --------------------------------- | ------------------------------------------ |
| Does one thing                    | Does more than one thing                   |
| Can't deploy dependencies         | Can deploy dependencies                    |
| Must respond to one kind of event | Can respond to more than one kind of event |
- There are multiple open source FaaS platforms available for use which can be deployed as container or in containerized environment and use it.
- IaaS to PaaS modernization of applications in the cloud native environment
- Containers are the great way for IT software evaluation over a time and its game changer.
- There are 2 methods to move from monolithic to microservices :
	1. Strangler pattern
		- With this pattern you strangle the monolithic application. New services or existing components are implemented as microservices. A facade or gateway routes user requests to the correct application. Over time, more and more features are moved to the new architecture until the monolithic application has been entirely transformed into a microservices application.
	2. Anticorruption layer pattern
		- This is similar to the strangler pattern but is used when new services need to access the legacy application. the layer then translates the concepts from existing app to new, and vice versa.
- k8s and service meshes became really popular and supports out of the box features.
- CaaS - Container as a service is also became more popular in cloud native environment.
- Benefits of a Microservices Architecture
	- Agility
	- Continuous Innovation
	- Evolutionary design
	- Small, focused teams
	- Fault isolation
	- Improved scale and resource usage
	- Improved observability
- Challenges with a Microservices Architecture
	- Complexity
	- Data Integrity and consistency
	- Performance
	- Development and testing
	- Versioning and integration
	- Monitoring and logging
	- Service dependency management
	- Availability
- Every application, whether cloud native or traditional, needs infrastructure on which to be hosted, technology that addresses pain points with development and deployment, and an architectural style that helps with achieving the business objectives, such as time to market. The goal of this chapter was to provide the basic knowledge for cloud native applications. By now you should understand that there are various container technologies with different isolation levels, how functions relate to containers, and that serverless infrastructure does not always need to be FaaS. Further, you should have a basic understanding of microservices architectures and of how you can migrate and modernize an existing application to be a cloud native application.

### Chap 3. Designing Cloud Native Applications

- A good way to designing cloud native applications is to consider five key areas when starting with the initial design: operational excellence, security, reliability, scalability and cost.
- Operational Excellence
	- Operational excellence means tbat you need to factor in how to run your application. monitor it and improve it over time when you are starting to design.
	- Automate Everything
		- Cloud automation goes hand in hand with infrastructure as Code (IaC). This enables you to minimize errors during environment provisioning and application deployment because the entire environment management is being defined using code artifacts.
		- Azure Resource Manager and AWS cloud formation with Hashicorp's terraform are few good IAC providers.
		- Besides automating how to provision the environment, you also need to automate the entire deployment process of your application.
	- Monitor Everything
		- Monitoring allows you to learn not only about your application and environment behaviour, but also how your application is being used.
	- Document everything
		- It is very common that cloud native applications are being built by many teams and in cloud native distributed environment its always needed to have a well documented APIs using swagger and other automated tools.
	- Make incremental changes
		- When making changes to both the environment as well as the application, you need to ensure that those changes are increamental and reversible.
	- Design for failure
		- Failures in the cloud will happen - period. you need to think not only about how to design your application to survive failures, but also about the processes that need to kick in when something goes wrong.
- Security
	- cloud native environment needs to be super safe and should follow defense in depth principle for security at each and every level.
	- Source code repository should be super secure and you should have CI step for vulnerability checks.
	- Container image scan for ports and RBAC needs to be enabled for private container repository.
	- K8s cluster needs to be RBAC enabled.
	- Protect the data and secure the communication between services.
- Reliability and Availability
	- Reliability means that the application will still work in an acceptable way even in the presence of failure, whereas availability means that your application is available for a certain amount of time.
	- In summary, to design for reliability and availability you should have testing in place that informs you of how your system is behaving and how your recovery mechanisms work. And, of course, the application needs to recover automatically by taking advantage of the scaling capabilities.
- Scalability and Cost
	- Scalability and cost go hand in hand. When designing a cloud native application, you need to think about not only how to scale the application, but also how to do it in a very cost-efficient way.

- Cloud Native versus Traditional Architectures
	- Traditional applications are often stateful in nature, which means that the application state was commonly stored with the compute instance.
	- Cloud native applications, on the other hand are stateless by nature. Stateless does not mean that they do not deal with data, but it means that they need to be designed in a way that the number of compute instances is highly dynamic without affecting user experiences that rely on data.
       	- Finally, there is a big difference in how cloud native architectures deal with failure as opposed to how traditional applications cope with them. cloud native expects failures and implement mechanism to deal with them whereas traditional architectures try to minimize failures.
  
- Functions versus Services
  - Function scenarios
    - Simple parallel execution scenarios in which functions do not need to communicate with one another.
  - Considerations for using functions
    - challenges when moving from a monolith to microservices
    - Limited lifetime of a function
    - No usage of specialized hardware
    - Functions are stateless and not directly network addressable
    - local development and debugging
    - Economics
- Most of the time a combination of functions and services is a great solution allowing you to take advantage of the simplicity of FaaS while benifitting from the flexibility of containerized services.

- API Design and Versioning
	- Three strategies for API versioning : 
		1. The knot - Consumers of your API are tied to a single version of the API. When the API changes, all consumers need to change as well.
		2. Point to Point - All API versions are kept running and each consumer uses the version they need to.
		3. Compitable Versioning - All consumers talk to the same API version. Old versions are deprecated and no longer exist because the latest version is backward compitable.
	
	- REST has three approaches that deal with versioning : global versioning, resource versioning and mime-based approach.
	- Regardless of the approach you take, it is important that you are able to monitor the API and versions of the API used by the consumers. Having good monitoring in place helps you decide how and when to deprecate the APIs.

- API backward and forward Compatibility

- Gateways
	- Gateways are of two types 
	    1. API Gateways
	    2. Application Gateways
    - Their responsibility can vary from routing incoming requests or SSL offloading or routing to specific backend service.

- Gateway can work as aggregator as well where it takes one request from the client and makes multiple requests to the underlying services.
- One of the most common uses for gateways is to offload different functionality from individual services and do them at the gateway level.
    - Examples of functionalities that can be offloaded from the individual services
        - Authentication and authorization
        - Rate limiting, retry policies, circuit breaking
        - Caching
        - Compression
        - SSL offloading
        - Logging and monitoring
    - its essential to monitor gateways for performance.

- There are multiple technologies available for implementing gateways. The most popular proxies that are used for gateways are NGINX, HAProxy and Envoy.
- Egress gateway running inside your private network can help direct and control all traffic exiting the private network.
- You should consider using an egress gateway if you need to monitor or control access to external services.
- Service Mesh with control plane and data plane configuration, Istio is most famous service mesh tool.
- Service Mesh features
    - Traffic Management
        - Request headers, URI and Sources are some common criteria for traffic management.
    - Failure Handing
    - Security
    - Tracing and monitoring

- As mentioned at the beginning of this chapter, each architecture is different, and there is no one-size-fits-all architecture. Nonetheless, there are specific components and building blocks in a cloud native application architecture that, if designed the wrong way, can cause many problems down the road. By understanding the technologies and patterns described in this chapter, you should be well prepared for designing a cloud native application from the compute side.

### Chap 4. Working with Data
- Cloud native applications take advantage of managed and serverless data storage and processing services.
- By using a managed database, the team can focus on building applications that use the database instead of spending time provisioning and managing the underlying data systems.
- Applications will often take advantages of multiple data storage systems, storing files in an object store, writing data to a relational database, and caching with an in-memory key/value store.
- Object storage service
	- manage data as objects
	- Determining where to store files such as images, documents, content and genomics data files will largely depend on the systems that access them.
	- Object/blob storage
		- Use it with files when the applications accessing the data support the cloud provider API.
	- File storage
		- Use it with applications designed to support Network Attached Storage (NAS).
	- Disk (block) storage
		- Use it for applications that assume persistent local storage disks, like mongodb or a MySQL database.
- distributed file system like HDFS is popular for big data analytics.

- Databases
	- Databases are generally used for storing more structured data with well-defined formats.
	- key/value
		- Ofter, application data needs to be retrieved using only the primary key, or may be even part of the key. A key/value store can be viewed as simply a very large hash table that stores some value under a unique key.
		- KV stores are very scalable datastores. selecting a key is important when using these datastores because it will have a significant impact on the scale and the performance of data storage reads and writes.
	- Document
		- A document database is similar to a key/value database in that it stores a document (value) by a primary key. Unlike a key/value database, which can store just about any value, the documents in a document database need to conform to some defined structure.
	- Relational
		- Relational databases organize data into two-dimentional structures called tables, consisting of columns and rows. Data in one table can have a relationship to data in another table, which the database system can enforce.
		- If the application data has a lot of relationships, especially those that require transactions, these databases might be a good fit.
	- Graph
		- A graph database stores two types of information: edges and nodes.
		- Graph database work well at analyzing the relationships between entities.
	- Column family
		- Column family database organizes data into rows and columns, and can initially appear very similar to relational database. You can think of a column-family database as holding tabular data with rows and columns, but the columns are divided into groups known as column families.
	- Time-series
		- Time-series data is a database that's optimized for time, storing values based on time. These databases generally need to support a very high number of writes.
		- Time-series databases are good for storing telemetry data. 
	- Search
		- Search engine databases are often used to search for information held in other datastores and services. A search engine database can index large volumes of data with near-real-time access to the indexes.
		- Some databases have full-text indexing features, but search databases are also capable of reducing words to their root forms through stemming and normalization.

- Streams and Queues
