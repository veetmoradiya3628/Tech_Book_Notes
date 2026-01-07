
#### Analytics
##### Amazon Athena
- Amazon Athena is an interactive serverless service used to analyze data directly in Amazon Simple Storage Service using Standard SQL ad-hoc queries
- charged based on amount of data scanned by each query
- Analyze different kinds of data
- executes multiple queries in parallel
- supports various standard data formats, such as CSV, JSON, ORC, Avro and Parquet

##### Amazon EMR
- Amazon EMR (Elastic Map Reduce) is a service used to process and analyze large amounts of data in the cloud using Apache Hive, Hadoop, Apache Flink, Spark etc.
- EMR cluster having EC2 instances as nodes
- decouples compute and storage layer

##### AWS Glue
- AWS Glue is a serverless ETL (Extract, transform and load) service used to categorize data and move them between various data stores and streams.
- AWS Glue Data Catalog - central repository
- managed ETL flow kind of.
- Better for batch processing.

##### Amazon Managed Service for Apache Flink
- The Amazon Managed Service for Apache Flink stands as a holistic data streaming platform, adept at managing the intricacies involved in configuring and linking data origins and destinations, all while requiring minimal coding efforts.
- Use cases
	- Rapidly deliver streaming data to destinations
	- Create real-time analytics apps
- Pricing based on Kinesis processing units (KPUs)
- Stateful computation over Data Streams
##### Amazon Data Firehose
- Amazon Data Firehose is a serverless service used to capture, transform and load streaming data into data stores and analytics services.
- Kinesis Data Firehose delivery streams

##### Amazon Kinesis Data Streams
- Amazon kinesis is a service used to collect, process and analyze real-time streaming data. It can be an alternative to Apache Kafka.
- Kinesis family consists
	- Kinesis Data Streams
	- Kinesis Data Analytics
	- Kinesis Data Firehose
	- Kinesis Video Streams
- Kinesis worker count etc
- Amazon Kinesis data analytics can be used with API Gateway to ingest and process huge amount of streaming data.

##### AWS Lake Formation
- AWS Lake Formation is a cloud service that is used to create, manage and secure data lakes. It automates the complex manual steps required to create data lakes.
- There is centralized access management for data in data lakes.
##### Amazon Managed Streaming for Apache Kafka
- Amazon MSK is a managed cluster service used to build and execute Apache Kafka applications for processing streaming data.
- Apache Kafka Access Control Lists (ACLs)

##### Amazon OpenSearch Service
- Amazon OpenSearch service is a managed service that allows users to deploy, manage and scale ElasticSearch clusters in the AWS Cloud.

##### Amazon Quick Sight
- A scalable cloud-based BI service providing clear insights to collaborators worldwide.
- Connects to various data sources, consolidating them into single data dashboards.

#### Application Integration

##### AWS AppSync
- AWS AppSync is a serverless service used to build GraphQL API with real-time data synchronization and offline programming features.
- AWS provides an Amplify Framework that helps build mobile and web applications using GraphQL APIs

##### Amazon EventBridge
- serverless event bus service for Software-as-a-Service (SaaS) and AWS services.
- It is fully managed service that takes care of event ingestion, delivery, security, authorization, error handling, and required infrastructure management tasks to setup and run a highly scalable serverless event bus. EventBridge was formerly called Amazon CloudWatch Events, and it uses the same CloudWatch Event API.
- Key concepts
	- Event buses
	- Events
	- Schema registry
	- Rules
	- Targets

##### Amazon SNS (Simple Notification Service)
- Amazon Simple Notification Service (Amazon SNS) is a web service that makes it easy to set up, operate, and send notifications from the cloud.
- topics and subscribers
- Standard Topics
	- When incoming message are not in order. In other words, messages can be delivered as they are received.
- FIFO topics
	- designed to maintain order of the messages between the applications, especially when the events are critical. Duplication will be avoided in this case.

##### Amazon SQS (Simple Queue Service)
- Amazon SQS is a serverless service used to decouple (loose couple) serverless applications and components.
- queue represents a temporary repository between the producer and consumer of messages.
- messages have fixed size of 256 KB
- Standard Queue
	- unlimited number of transactions per second
	- messages gets delivered in any order
	- messages can be sent twice or multiple times
- FIFO Queue
	- 300 messages per second
	- Support batches of 10 messages per operation, results in 3000 messages per second.
- Delay Queue
	- Delay queue is a queue that allows users to postpone/delay the delivery of messages to a queue for a specific number of seconds.
	- Messages can be delayed for 0 seconds - 15 minutes
- Dead-Letter Queue
	- Dead letter queue is a queue for those messages that are not consumed successfully. It is used to handle message failure. Visibility Timeout is the amount of time during which SQS prevents other consumers from receiving (poll) and processing the messages.

##### AWS Step Functions
- Step functions allow developers to offload application orchestration into fully managed AWS services. This means you can just modularize your code to steps and let AWS worry about handling partial failure cases, retries, or error handling scenarios.
- Types of step functions
	- Standard Workflow
		- Standard workflow can be used for long-running, durable and auditable workflows
	- Express Workflow
		- Express workflow is designed for high volume, and event processing workloads.
- based on task and state machines
	- Tasks can be defined by using an activity or an AWS Lambda function
	- State machines can express an algorithm that contains relations, input/output
- execution modes
	- best effort execution
	- at least once
	- at most once
	- exactly once execution
- Step functions built-in retry and catch fields provide exponential backoff retries and error handling, allowing you to skip non-critical steps while logging failures.
- Sync express workflow vs. Async express workflow

#### Cloud Financial Management

##### AWS budgets
- AWS Budgets enables the customer to set custom budgets to track cost and usage from the simplest to the complex use cases.
- set reservation utilization or coverage targets allowing you to get alerts by email or SNS notification when metrics reach the threshold.
- AWS budgets can now be created monthly, quarterly, or annually budgets for the AWS resource usage or the AWS costs.

##### AWS Cost and Usage Report
- AWS Cost & Usage report is a service that allows users to access the detailed set of AWS cost and usage data available including metadata about AWS resources, pricing, Reserved instances, and Savings Plans.
- updates reports up to three times a day.
- AWS Organizations set-up a Cost and Usage report, service control policy (SCP) can be used.

##### AWS Cost Explorer
- AWS Cost Explorer is a UI-tool that enables users to analyze the costs and usage with the help of a graph, the Cost Explorer cost and usage reports, and/or the Cost Explorer RI report. It can be accessed from the Billing and Cost Management console.
- Reserved Instance Reports
	- RI Utilization reports
	- RI Coverage reports
- Access the data programmatically using the Cost Explorer API

#### Compute

##### AWS Auto Scaling
- AWS Auto Scaling keeps on monitoring your application and automatically adjusts the capacity required for steady and predictable performance.
- Allows creating scaling plans for EC2 Instance, EC2 tasks, DynamoDB, Aurora Read replicas
- Launch configuration vs. Launch Template
- Health check & CloudWatch Events for monitoring
- It can monitor SQS queue size and ASG can be configured to scale up and down based on SQS queue size (no. of messages in queue).
- No cost for configuration but cost for resources that we will use

##### AWS Batch
- AWS Batch allows developers, scientists, and engineers to run thousands of computing jobs in the AWS platform.
- Executes workloads on EC2 and AWS Fargate
- Components
	- Jobs
	- Job Definitions
	- Job Queues
	- Compute environments
		- managed vs. unmanaged
	- Schedular

##### AWS EC2
- Elastic Compute Cloud
- Virtual machine in cloud environment
- Can scale up & down automatically based on the traffic
- Instance types
- EBS Volume
	- Elastic Block Storage
	- block-level storage that is assigned to your single EC2 instance
	- Types
		- General purpose (SSD)
		- Provisioned IOPS (SSD)
		- Throughput optimized Hard disk drive
		- Cold Hard Disk Drive
		- Magnetic
- Instance Store
	- Instance store is the ephemeral block-level storage for the EC2 instance
- AMI
	- Amazon Machine Image
	- AMI decides OS, installs dependencies, libraries, data of your EC2 instances
	- Multiple instances can be launched with single AMI
- Security Groups
	- Virtual firewall to EC2 instance
	- defines the type of port and kind of traffic to allow
	- stateful
	- by default outbound traffic is allowed and needs to define the inbound rules
- Key pair for login to instance (public and private key)
- Tag for resource tagging
- Pricing On-demand, Savings plan, Reserved Instances, and Spot instances
- Placement groups
	- Spread placement
	- Partition placement
- Reserved Instances for baseline traffic and spot instances for unpredictable spikes optimizes both cost and performance.
##### EC2 Auto Scaling
- region-specific service
- ASG is a collection of the minimum number of EC2 used for high availability
- provides features as fault tolerance, health check, scaling policies, and cost management

##### AWS Elastic Beanstalk
- Beanstalk is a compute service for deploying and scaling applications developed in many popular programming languages.
- Platform as a Service
- AWS Elastic Beanstalk is the best way to deploy your application in the fastest and simplest way.
- provides the user interface / dashboard to monitor your application.
- Two types of environments
	- Web Tier environment
	- Worker environment
- Deployment models
	- All at once
	- Rolling
	- Rolling with additional batch
	- Immutable
	- Traffic splitting
- We can swap the environment in backend at runtime.

##### AWS Fargate
- AWS Fargate is a serverless compute service that is used for containers by Amazon Elastic Container service (ECS) and Amazon Elastic Kubernetes Service (EKS).
- Amazon EFS volumes for persistent storage
- Ephemeral storage for non persistent storage
- Auto scale in built
- Built in integration with Amazon CloudWatch, Container Insights

##### AWS Lambda
- AWS Lambda is a serverless compute service through which you can run your code without provisioning any servers.
- It only runs your code when needed and also scales automatically when the request count increases.
- Pay per use principle
- Lambda can be triggered in response to the events
- Lambda Functions
- Lambda Layers
	- A lambda layer is a container/archive which contains additional code such as libraries, dependencies, or custom runtimes.
	- Allows Up to 5 Layers
- Lambda event
- Lambda @ Edge
	- It is the feature of Amazon CloudFront which allows you to run your code closer to the location of Users of your application.
	- It improves performance and reduces latency.
	- Lambda @ Edge runs your code in response to the event created by the CDN.

##### AWS Outposts
- AWS Outposts enables running AWS services locally and accessing a variety of services within the local AWS region.
- Hybrid Cloud Deployment
- Edge computing
- Data processing at remote locations
- Application modernization

##### AWS Wavelength
- Amazon Wavelength is used to create mobile applications with exceptionally low latencies.
- Integrates storage and computing resources directly into the 5G edge networks of communications service providers (CSPs)
- Wavelength zones
- Wavelength services

#### Containers
##### Amazon Elastic Container Registry
- Amazon Elastic Container Registry is a managed service that allows users to store, manage, share and deploy container images and artifacts. It is mainly integrated with Amazon Elastic Container Service (ECS) for simplifying the production workflow.
- Lifecycle policies
- AWS marketplace
- IAM with each registry access level control
- Public gallery

##### Amazon Elastic Container Service (ECS)
- Amazon ECS is a regional container orchestration service like Docker that allows to execute, stop and manager containers on a cluster.
- Task definition with JSON format
- Service as a configuration object to run and maintain several tasks simultaneously in a cluster.
- Task schedular responsible for attaching tasks within your cluster based on the task definitions.
- Use cases includes Microservices, Batch Jobs 
- Fargate launch type vs. EC2 Launch type

##### Amazon Elastic Kubernetes Service (EKS)
- Amazon EKS is a service that enables users to manage Kubernetes applications in the AWS cloud or on-premises. Any standard Kubernetes application can be migrated to EKS without altering the code.
- EKS cluster has two components :-
	- Amazon EKS control plane
	- Amazon EKS nodes
- Control planes includes etcd, Kubernetes API server
- Two methods to create a cluster
	- eksctl
	- AWS management console and AWS CLI
- For pod scheduling
	- Self-managed nodes
	- Amazon EKS managed node groups
	- AWS Fargate
- Easy integration with other AWS services for networking, storage etc

#### Database

##### Amazon Aurora
- Fully managed RDS services offered by AWS. It's only compatible with PostgreSQL / MySQL, as per AWS Aurora provides 5 times throughput then MySQL and 3 times throughput then PostgreSQL.
- supported by region which have minimum 3 AZs
- up to 15 read replicas
- 128 TB per database instance scale
- two instances
	- Primary DB instance
	- Aurora Replica
- Aurora Global database
- Aurora Multi master
- Aurora Serverless
- Fault tolerance and Self-healing feature
- Aurora replicas in the same region and enabling automation failover for high availability provides low latency and performance
- Aurora replicas in different region which is Aurora Global Database adds cross region latency
- Aurora serverless ideal for workloads with variable demand but not ideal for large-scale read-heavy workloads.
- Switchover vs. Failover

##### Amazon DocumentDB
- DocumentDB is a fully managed document database service by AWS which supports MongoDB workloads.
- highly recommended for storing, querying, and indexing JSON data.
- maximum up to 64 TB
- provides up to 15 read replicas with single-digit millisecond latency.
- supports RBAC

##### Amazon ElastiCache
- ElastiCache is a fully managed in-memory data store. It works with both Redis and Memcached protocol based engines.
- data is in key-value format
- Shard having collection of primary nodes and read-replicas
- Multi AZ possible by placing a read replicas in another AZ
- Use cases
	- To store web sessions
	- Caching database results
	- Live polling and gaming dashboards
- Redis cluster mode allows us to scale cluster horizontally by re-sharing data across multiple shards.
- ElastiCache does not support auto scaling in redis mode.

##### Amazon Keyspaces (for Apache Cassandra)
- Keyspaces is an Apache Cassandra compatible database in AWS. It is fully managed, high availability and scalable.
- Compatible with CQL (Cassandra Query Language) 
- Two modes
	- On-demand capacity mode
	- Provisioned capacity mode
- PITR - Point in time recovery
- Replicated across 3 AZs for high availability

##### Amazon Neptune
- Amazon Neptune is a graph database service used as a web service to build and run applications that require connected datasets.
- Partitioning the data into subgraphs based on userId or similar identifier reduces the complexity of traversal queries by limiting the data that needs to be searched, leading to better performance.
- Does not support materialized view in the same way RDBMS does.

##### Amazon RDS
- RDS (Relational database system) in AWS makes it easy to operate, manage and scale in the cloud.
- Engines supported by RDS:
	- MySQL
	- SQL
	- MariaDB
	- PostgreSQL
	- Oracle
	- Amazon Aurora
- DB instances
	- Standard
	- Burstable Performance
	- Memory optimized
- Multi AZ deployment
- Storage types
	- General purpose (SSD)
	- Provisioned IOPS (SSD)
- Monitoring, backup & restore
- Replication is asynchronous

##### Amazon Redshift
- Amazon redshift is a fast and powerful, fully managed, petabyte-scale data warehouse service in the cloud. This service is highly scalable to a petabyte or more for $1000 per terabyte per year.
- Single node (160 GB)
- Multi-node
	- Leader and compute node
- Massively parallel processing (MPP)
- Maximum retention period is 35 days
- PARALLEL option allows for efficient handling of large files by loading data in parallel.
- MANIFEST option ensures that all files in the dataset are properly accounted for during the ingestion process, making this the most appropriate configuration.
- GZIP for compressing data
- UNLOAD command for export data from redshift to S3
- Distribution style "KEY" allows us to distribute rows based on the values of a specific column.
- https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-best-dist-key.html

#### End User Computing

##### Amazon Workspaces
 - Amazon Workspaces is a managed service used to provision virtual Windows or Linux desktops for users across the globe.

#### Frontend Web and Mobile

##### Amazon App Stream 2.0
- Amazon App Stream 2.0 is a managed AWS service that streams desktop and SaaS applications to users via a browser, removing the need for local installs or complex infrastructure.
- Cost effective, Reliable, Remote Workforce support

##### AWS Amplify
- AWS Amplify is a set of tools and services provided by Amazon Web Services (AWS) that enables developers to build scalable and secure web and mobile applications quickly. 
- It supports popular frontend frameworks like React, Angular and Vue.js, making it easier for developers to integrate with their existing workflows.
- Use Version control, Modularize app, IaC, Environment management, CI/CD, Data validation and sanitization
- To build personal blog website, e-commerce mobile app, event scheduling apps etc

##### Amazon API Gateway
- Amazon API Gateway is a service which creates, publishes, maintains, monitors and secures APIs at any scale.
- It handles the tasks involved in processing concurrent API calls.
- HTTP based
- Enables stateless and client-server communication
- Edge-optimized endpoint
- To enforce CORS we can use Access-Control-Allow-Origin header for configuration
#### Internet of Things (IoT)

##### AWS IoT Analytics
- AWS IoT Analytics streamlines the intricate process of analyzing extensive amounts of IoT data, eliminating the need for constructing a complex and costly IoT analytics platform.
- Features
	- Collect
	- Process
	- Store
	- Analyze
	- Hosted Notebooks
	- Automated execution
	- Incremental Data Capture
	- Visualization
- Use cases
	- Contextual Data Enrichment
	- Predictive Maintenance
	- Proactive Supply Replenishment
	- Process Efficiency Monitoring
- IoT Analytics provides an automated way of analyzing data from IoT devices. Channel is concept for data analytics.

##### AWS IoT Core
- AWS IoT core is a cloud service that enables users to connect IoT devices (wireless devices, sensors, and smart appliances) to the AWS cloud without managing servers.
- supported protocols
	- MQTT
	- MQTT over WSS
	- HTTPS
- supports bi-directional communication and LoRaWAN
- It integrates with Amazon services well

##### AWS IoT Events
- AWS IoT Events is a monitoring service that allows users to monitor and respond to device fleets events in IoT applications.
- It helps to create event logic using conditional statements and trigger alerts when  an event occurs.
- AWS IoT Events accepts data from many IoT sources like sensor devices, AWS IoT Core and AWS IoT Analytics.

##### AWS IoT Greengrass
- AWS IoT Greengrass is a cloud service that groups, deploys and manages software for all devices at once and enables edge devices to communicate securely.
- Greengrass core is a device that enables the communication between AWS IoT core and the AWS IoT Greengrass
- provides AWS Lambda functions and Docker containers as an environment for code execution.

##### AWS IoT SiteWise
- Industrial data collection, organization and monitoring
- Factories, plants and industrial equipment monitoring
- focus on industrial machines and processes
- AWS IoT SiteWise plug-in  for amazon managed Grafana can be used to visualize equipment data in near-real time using visualization options in Grafana dashboards.

##### AWS IoT FleetWise
- Vehicle data collection and normalization
- Automotive manufacturers and connected vehicle fleets
- Focused on vehicle and automotive data

##### AWS IoT TwinMaker
- Build digital twins (virtual, real-time visual models) of systems
- Visualizing and simulating complex physical environments
- AWS IoT Twin Maker to create a digital twin from the collected data
- AWS IoT Twin Maker plug-in for amazon managed Grafana used for building dashboards embedding 3D scenes and display data insights about the physical systems.
#### Machine Learning

##### Amazon Polly
- Text to speech conversion
- pay for the text converted
- different language supports, Neural-text-to-speech (NTTS)

##### Amazon SageMaker
- Amazon Sage Maker is a cloud service that allows developers to prepare, build, train, deploy and manage machine learning models.
- Sage Maker Canvas supports importing datasets from Amazon S3 directly.

##### Amazon Comprehend
- Document processing
- Amazon Comprehend employs natural language processing (NLP) to extract insights from document content. 
- Document processing workflow enhancement
- PII from documents
- Built in feature for sentiment analysis, language detection, key phase extraction

##### Amazon Rekognition
- Amazon Rekognition is a cloud based service that employs advanced computer vision technology to analyze images and videos without requiring expertise in machine learning.
- Object and text detection
- Object, Sense and Concept detection
- Text detection
- People tracking
- Facial analysis

##### Amazon Lex
- Amazon Lex, an AWS service, enables developers to build chatbots with natural conversion capabilities, leveraging the technology behind Alexa. 
- Lex simplifies speech recognition and facilitates the creation of engaging chatbots for intuitive user interfaces.
- Amazon Lex is not good with natural language processing task such as sentiment analysis, language detection, or custom entity recognition.

##### Amazon Transcribe
- Amazon Transcribe is a service used to convert audio (speech) to text using a Deep Learning process known as automatic speech recognition (ASR).

##### Amazon Kendra
- Amazon Kendra is a cutting-edge search solution powered by AI, utilizing natural language processing (NLP) and machine learning to provide highly accurate and context-aware search results.
- Contextual search
- Machine Learning
- Easy Integration
- Security
- Simplicity
- Provides intelligent search capabilities for indexed content.
##### Amazon Translate
- Neural Machine Translation - Uses neural networks for accurate and natural text translations
- Language pairs
- Source-target conversion

##### Amazon Personalize
- Provides personalized product recommendation

##### Amazon Fraud Detection
- Detects Fraudulent activities in transactions.

#### Management and Governance

##### AWS CloudFormation
- AWS CloudFormation is a service that collects AWS and third-party resources and manages them throughout their life cycles, by launching them together as a Stack.
- Template is used to create, update and delete an entire stack as a single unit without managing resources individually
- Templates - JSON or YAML
- Stack
- Nested Stack
- Change sets
- Stack updates
- AWS CloudFormation Registry

##### AWS CloudTrail
- Audit service in AWS account
- global service that permits users to enable operational and risk auditing of the AWS account
- allows users to view, search, download, archive, analyze and respond to account activity across the AWS Infrastructure

##### Amazon CloudWatch
- Amazon CloudWatch is a service that helps to monitor and manage services by providing data and actionable insights for AWS applications and infrastructure resources.
- It collects monitoring data in form of logs, metrics and events from AWS resources.
- Alarms
- statsd & collectd protocol for log observability
- Synthetic monitoring to simulate user traffic

##### Amazon CloudWatch Logs
- Amazon CloudWatch logs is a service provided by AWS that enables you to monitor, store and access log data from various AWS resources and applications.
- Log collection
- Log storage
- Real-time monitoring
- Log queries
- Log retention
- Log streams

##### AWS Compute Optimizer
- AWS Compute optimizer assists in optimizing the provisioning of various AWS resources, including Amazon EC2 instances, Amazon EBS volumes, ECS services on AWS Fargate, and AWS Lambda functions.
- It analyzes utilization data to prevent both overprovisioning and under provisioning ensuring that resources are efficiently allocated to meet workload demands.
- Tailored Rightsizing Recommendations
- Utilize external metrics
- Facilitate migration to AWS Graviton CPUs
- Licensing optimizations

##### AWS Config
- AWS Config is a service that continuously monitors and evaluates the configurations of the AWS resources (services).
- Config rules
- Aggregator
- Conformance pack
- Custom rules and its compliance requirements

##### AWS Health Dashboard
- AWS Health dashboard keeps you informed about service events, scheduled modifications, and account notifications, enabling effective management and action-taking.
- Swift resolution and proactive management
- Receive Proactive notifications
- Prepare for Lifecycle Events
- Efficient Event monitoring
- Incident troubleshooting

##### AWS Control Tower
- AWS Control Tower is an extension to AWS Organizations providing additional controls.
- Creates a Landing zone which is a well-architected multi-account baseline based on AWS best practices.
- OU - Organizational Units
- Integrates with AWS Identity center
- SCP at OUs
- Guardrails
	- Preventive guardrails
	- Detective guardrails
- policies
- Account Factory
- Pre-approved account configuration

##### AWS License Manager
- AWS License manager is a service that manages software licenses in AWS and on-premises environments from vendors such as Microsoft, SAP, Oracle and IBM
- Supports BYOL - Bring your own license

##### AWS Management Console
- AWS Management console is a web application that consists of many service consoles for managing Amazon Web Services.

##### AWS Organizations
- AWS Organizations is a global service that enables users to consolidate and manage multiple AWS accounts into an organization.
- The main account is the management account
- Other accounts are member account and can be part of single organization.
- Service Control Policies (SCPs) can be created to provide governance boundaries for the OUs.
- For migrating from one org to other org account first make it independent and remove from existing organization.

##### AWS Systems Manager
- AWS Systems manager is a service which helps users to manage EC2 and on-premises systems at scale. It not only detects the insights about the state of the infrastructure but also easily detects problems as well.
- Systems manager documents for defining service actions

##### AWS Trusted Advisor
- Trusted advisor itself provides checks based on Best Practices in the Cost Optimization, Security, Fault tolerance and performance improvement categories.
- Optimization of cost & efficiency
- Address security gaps
- performance improvement 

#### Migration & Transfer

- Mapping out application dependencies and resource requirements is crucial before initiating the migration. This assessment helps identify interdependencies, configurations, and specific needs of your applications, which is essential for ensuring that all components work correctly after migration and that nothing critical is missed during the transition.
##### AWS Application Discovery Service
- AWS Application Discovery service helps you plan application migration projects. It automatically identifies servers, virtual machines (VMs) and network dependencies in your on-premises data centers.
- Agentless discovery
- Agent-based discovery
- AWS partner network (APN)
- Migration Hub
- Application dependencies, utilization and configurations.

##### AWS Database Migration Service
- AWS Database migration service is a cloud-service used to migrate relational databases from on-premises, Amazon EC2 or Amazon RDS to AWS Securely.
- MySQL - MySQL - homogeneous migration
- MySQL - Amazon Aurora - heterogeneous migration
- Task endpoints
- AWS SCT - Schema Conversion Tool helps to perform heterogeneous migration.
- Full load migration with SCT and CDC for minimal downtime and to handle complex database migration.
- its used for data migration, not schema optimization. while DMS can apply changes to the target database, it does not perform schema optimization, ignoring manual intervention can lead to performance issues or incompatibilities in the target MySQL environment.

##### AWS DataSync
- AWS DataSync is a secure, reliable, managed migration service that automates the movement of data online between storage systems.
- AWS DataSync helps you simplify your migration planning and reduce costs associated with the data transfer.
- AWS Storage Gateway
- DataSync with task retries allows the transfer to resume automatically after network interruptions, ensuring data integrity without manual intervention.

##### AWS Migration Hub
- AWS Migration Hub (Migration Hub) offers a centralized platform for discovering current servers, planning migrations, and monitoring application migration progress.
- Migration hub allows visualization of connections and status regardless of the migration tool used.
- Streamlined process
- Guided expertise
- Effective resources
- No cost

##### AWS Transfer Family
- AWS Transfer Family is a fully managed & secure service that enables transfer of files using SFTP, FTPS & FTP.
- The destination storage services to which files are transferred are S3 and EFS.

##### AWS Snow Family
- The AWS Snow Family includes devices for transferring large datasets to and from AWS and enabling data processing at the edge, even in rugged or remote locations.
- Snow Devices
	- AWS Snowcone
		- Lightweight, portable, ideal for remove field
		- SSD & HDD
		- 4.5 pound / 2.1 kg
		- Designed for smaller data transfers (up to 8 TB)
	- AWS Snowball
		- Compute optimized and storage optimized 
		- Tamper-resistant and designed for extreme environments 
		- on device computing
		- Snowball Edge storage optimized designed for large data transfer (up to petabytes)
		- Edge Compute optimized is more suitable for scenarios requiring edge computing capabilities.
	- AWS Snowblade
		- high performance and compact device, designed specifically for tactical edge scenarios
	- AWS Snow Edge
		- Enhanced edge computing device capable of supporting even larger and more complex workloads.
		- Ideal for intermittent connectivity environment.
	- AWS Snowmobile
		- Typically used for exabyte-scale data migrations and for petabytes scale
##### Seven Common migration strategies (7Rs)
- 7Rs
	- Retire
	- Retain
	- Rehost
	- Relocate
	- Repurchase
	- Replatform
	- Refactor or re-architect
- Retain
	- "lift and shift" applications are moved to cloud without any modifications
- Relocate
	- "drop and shop" strategy involves moving instances or objects within an AWS environment such as to a different VPC, Region or AWS account
- Repurchase
	- exiting application is replaced with a new version or product offering 
- Replatform
	- "lift, tinker and shift" or "lift and reshape"
	- migrating to cloud with additional optimization and cost reduction
- Refactor or re-architect
	- modify the applications architecture to fully utilize cloud-native features.

##### AWS Transform
- modern replacement for Application Discovery Service and provides agentless, on-premises environment analysis.

#### Network & Content Delivery

##### AWS Application Migration Service
- The AWS Application Migration Service streamlines and automates the conversion of your source servers to operate seamlessly on AWS, reducing the need for labor-intensive and error-prone manual tasks.

##### Amazon CloudFront
- Amazon CloudFront is a content delivery network (CDN) service that securely delivers any kind of data to customers worldwide with low latency, low network and high transfer speeds.
- Uses edge locations to cache copies of the data for the lowest latency.
- Lambda @ Edge
- Access controls
	- Signed URLs
	- Singed Cookies
	- Geo Restriction
	- Origin Access Identity (OAI)

##### AWS Direct Connect
- AWS Direct Connect is a cloud service that helps to establish a dedicated connection from an on-premises network to one or more VPCs in the same region.
- Private VIF
- Methods to connect
	- AWS Managed VPN
	- AWS Direct Connect
	- AWS Direct Connect plus VPN
	- AWS VPN CloudHub
	- Transit VPC
	- VPC Peering
	- AWS PrivateLink
	- VPC Endpoints
- Direct connect gateway - Globally available service used to connect multiple Amazon VPCs across different regions or AWS accounts
	- Transit gateway
	- Virtual private gateway
- scale by 1 Gbps and 10 Gbps connections based on the capacity needed.
- Ideal for applications that requires consistent, high-bandwidth connectivity (dedicated connection to AWS).
- VPN is suitable for applications that need same connection that can be quickly established, as it can be set up faster and with more flexibility than direct connect.

##### AWS Elastic Load Balancer
- It distributes the incoming traffic to multiple targets such as Instances, Containers, Lambda Functions, IP addresses etc
- spans single or multiple AZs
- provides high availability, scaling and security for the application.
- Types of ELB
	- Application Load Balancer
	- Network Load Balancer
	- Gateway Load Balancer
	- Class Load balancer - old generation
- Listeners
- Target groups
- Health check

##### AWS PrivateLink
- AWS PrivateLink is a network service used to connect to AWS services hosted by other AWS accounts (referred to as endpoint services) or AWS Marketplace
- Interface endpoint

##### Amazon Route 53
- Route53 is a managed DNS (Domain Name System) service where DNS is a collection of rules and records intended to help clients / users understand how to reach any server by its domain name.
- Route 53 hosted zone
	- public host zone
	- private host zone
- Route53 TTL
- CNAME vs. Alias
- A, AAAA, CNAME, Alias
- CAA, MX, NAPTR, NS, SOA, SPF, SRV, TXT
- Routing policies
	- Simple
	- Failover
	- Geo-location
	- Latency based
- Domain registrar != DNS

##### AWS Transit Gateway
- AWS Transit gateway is a network hub used to interconnect multiple VPCs. It can be used to attach all hybrid connectivity by controlling your organizations entire AWS routing configuration in one place.
- IPSec VPN
- Transit Gateway Network manager
- Transit Gateway vs. VPC Peering

##### AWS VPC
- Amazon VPC is a service that allows users to create a virtual dedicated network for resources.
- Security group
	- Default
	- Custom
- NACL
	- Default NACL
	- Custom NACL
- Components
	- Subnets
	- Route tables
	- NAT Devices
	- NAT Instances
	- NAT Gateway
	- DHCP Options set
	- PrivateLink
- Types of endpoints
	- Interface endpoints
	- Gateway Load balancer endpoints
	- Gateway endpoints
	- Egress only internet gateway
- VPN
	- AWS Site-to-site VPN
	- AWS Client VPN

##### AWS Global Accelerator
- AWS Global accelerator is a networking service designated to improve application performance, availability and security by routing user traffic through the AWS Global network.
- Enhanced Network performance
- High availability
- Deterministic Routing
- DDoS Protection
- Use cases
	- Global traffic manager
	- API acceleration
	- Global static IP
	- Low-latency gaming & media workloads

#### Security, Identity & Compliance
##### AWS Certificate Manager (ACM)
- AWS Certificate manager is a service that allows a user to provide, manage, renew and deploy public and private secure sockets layers / TLS x.509 certificates.
- direct issue new certificate vs. importing existing third-party certificate.

##### AWS Nitro Enclaves
- AWS Nitro enclaves are isolated, hardened virtual environments with in an Amazon EC2 instance.

##### Amazon Cognito
- Amazon Cognito is a service used for authentication, authorization and user management for web and mobile applications.
- User pools
- Identity pools
- Federated Identity

##### Amazon Detective
- Regional service
- GuardDuty is pre requisites
- Triage security findings / alerts, incident investigation, Thread hunting

##### AWS Directory Service
- AWS Directory service also known as AWS Managed Microsoft Active Directory (AD), enables multiple ways to use Microsoft AD with other AWS services.
- AD Connector
##### Amazon GuardDuty
- Managed Thread detection service offered by AWS that continuously monitors and analyzes activity within AWS account to identify potential security threats and vulnerabilities.
- Region specific Integrate with CloudTrail, EventBridge & Lambda services
- Does not automatically remediate non-compliant configurations.
##### AWS IAM
- Identity and access management
- Control access to AWS resources securely
- AWS Principal
	- Users, Groups, Roles
- Root account is the first principal
- Root User
- IAM User
- IAM Group
	- collection of Users
- IAM Role
	- policies attached to user
- Assume roles
- IAM Policies
	- what level of access an identity or AWS resource will posses.
	- JSON based documents
	- Resource based policy
	- Identity based policy
	- Managed policy
		- AWS managed
		- Customer managed policies
	- Inline policies
- IAM Access Analyzer to analyze IAM access details across the account / organization & it can be integrated with security hub to send findings directly in Security Hub.

##### Amazon Inspector
- Amazon Inspector is a vulnerability management service which continuously scans AWS resources for software vulnerabilities and network accessibility.
- Features
	- Automation of vulnerability management
	- multiple account with Amazon Organizations
	- CVE findings etc

##### AWS Key Management Service
- AWS Key management service (AWS KMS) is a secured service to create and control the encryption keys.
- Keys in KMS are regional
- Customer master keys
- Customer managed CMKs
- AWS managed CMKs
- AWS each request on Key Usage gets recorded in a log file in CloudTrail

##### AWS Resource Access Manager
- AWS Resource Access Manager (RAM) is a service that permits users to share their resources across AWS accounts or within their AWS organizations
- Access control policy in AWS IAM and Service control policies in AWS Organizations provides security and governance controls to AWS Resource Access Manager (RAM)

##### AWS Secrets Manager
- AWS secrets manager is a service that replaces secret credentials in a code like passwords, with an API call to retrieve the secret programmatically.
- feature rotation, management, and retrieve database passwords, OAuth tokens, API keys and other secret credentials.
- secret can be secured with encryption keys managed by KMS
- Integrates with AWS CloudTrail and AWS CloudWatch to log and monitor services for centralized auditing.

##### AWS Security Hub
- AWS Security Hub is a service that provides an extensive view of the security aspects of AWS and helps to protect the environment against security industry standards and best practices.
- It collects data using a standard findings format and reduces the need for time consuming data conversion efforts.
- Integrated dashboards are provided to show the current security and compliance status.
- Works well with Organizational level complexity

##### AWS STS
- Use AWS STS when you need to enhance security, delegate permissions, to provide temporary controlled access to AWS resources for users, applications or services in a flexible and granular manner.
- It helps follow security best practices and reduce the reliance on long-lived credentials, improving overall security posture in your AWS environment.
- STS API operations
	- AssumeRole
	- AssumeRoleWithSAML
	- AssumeRoleWithWebIdentity
	- GetSessionToken
	- DecodeAuthorizationMessage
	- GetCallerIdentity

##### AWS WAF
- WAF stands for Web Application Firewall
- Managed service provided by AWS that helps protect web applications from common web exploits that could affect application availability, compromise security or consume excessive resources.
- Protect against SQL Injection, cross-site scripting, distributed denial of service (DDoS) attack

#### Storage

##### AWS Backup
- AWS Backup is a secure service that automates and governs data backup (protection) in the AWS cloud and on-premises.
- incremental backup, automated backup etc
- Cost per monthly

##### AWS EBS - Elastic Block Storage
- Amazon Elastic Block Storage is a persistent block-level storage (volume) service designed to be used with Amazon EC2 instances. EBS is AZ specific & automatically replicated within its AZ to protect from component failure, offering high availability and durability.
- Types of EBS
	- Types of SSD
		- General purpose - gp2
		- Provisioned IOPS SSD - io1
	- Types of HDD
		- Throughput Optimized HDD (st1)
		- Cold HDD (sc1)
- Backup/Migration
- Provisioned capacity
- Instance Store (ephemeral storage)
	- Massive IOPS

##### AWS EFS - Elastic File Storage
- Amazon Elastic File Storage (Amazon EFS) provides a scalable, fully managed elastic distributed file system based on EFS.
- persistent file storage
- scales to petabytes
- Types
	- Standard
	- Infrequent access storage (EFS-IA)
- Access modes
	- Performance modes
		- General purpose 
		- Max I/O
	- Throughput modes
		- Bursting
		- Provisioned

##### AWS FSx for Windows File Server
- Amazon FSx for windows file server is an FSx solution that offers a scalable and shared file storage system on the Microsoft Windows server.

##### AWS FSx for Lustre
- Amazon FSx for Lustre is an FSx solution that offers scalable storage for the Lustre system (parallel and high-performance file storage system)

##### AWS S3
- S3 stands for Simple storage service. Amazon S3 is object storage that allows us to store any kind of data in the bucket.
- Object storage
- Files are stored in bucket
- 0 to 5 TB
- bucket names must be unique gloabally
- Versioning
- Static website hosting
- Encryption
- Objects lock
- Transfer Acceleration
- Access Control List
- Bucket policy
- CORS
- Access Point
- Life cycle policy
- Replication
- S3 classes
	- S3 Standard
	- S3 Standard-IA
	- S3 Express One Zone
	- S3 Intelligent Tiering
	- S3 One Zone-IA
	- S3 Glacier Instance Retrieval
	- S3 Glacier Flexible Retrieval
	- S3 Glacier Deep Archive
		- for storing data for long term

##### S3 Glacier
- Amazon S3 Glacier is a web service with vaults that offers long-term data archiving and data backup.
- It is the cheapest S3 storage class and offers 99.999999999% of data durability.
- S3 Glacier data retrieval options
	- Expedited retrievals
	- Standard retrievals
	- Bulk retrievals
- Peta bytes of data scale

##### AWS Storage Gateway
- AWS Storage gateway is a hybrid cloud storage service that allows your on-premises storage & IT infrastructure to seamlessly integrate with AWS cloud storage services. It can be AWS provided hardware or compatible virtual machine.
- Volume gateway (iSCSI)
- File gateway (NFSv4 / SMB)
- Tape gateway (VTL)

##### AWS Elastic Disaster Recovery
- AWS Elastic Disaster Recovery (AWS DRS) ensures fast and reliable recovery of both on-premises and cloud-based applications.


#### Developer Tools

##### AWS CodeBuild
- AWS CodeBuild is a continues integration service in the cloud used to compile source code, run tests, and build packages for deployment.
- Code services includes
	- AWS CodeBuild
	- AWS CodeCommit
	- AWS CodeDeploy
	- AWS CodePipeline

##### AWS CodeDeploy
- AWS CodeDeploy is a service that helps to automate application deployments to a variety of compute services such as Amazon EC2, AWS Fargets, AWS ECS and on-premises instances.
- Use cases
	- In-place deployment
	- Blue/green deployment
- It does support Canary deployment with gradual switch over of traffic by % to new version.

##### AWS CodeArtifact
- AWS CodeArtifact is a fully managed comprehensive software artifact repository service.
- Centralized Artifact Repository
- Support multiple package formats
- Security and Access Control
- Dependency resolution
- Integration with popular tools

##### AWS X-Ray
- AWS X-Ray is a service that allows visual analysis or allows to trace microservices based applications.
- Tracing service

##### AWS CodeGuru
- CodeGuru is a developer tool designed to enhance code quality and optimize application performance by offering intelligent recommandations.

#### Media Services & Blockchain

##### Amazon Elastic Transcoder
- Amazon Elastic Transcoder delivers a cloud-based service for media transcoding, offering developers and businesses a scalable, intuitive and cost-effective method to convert media files into formats suitable for diverse devices, including smartphones, tablets and computers.
- Transcoding pipelines, jobs, system transcoding presets, custom transcoding presets

##### Amazon Managed Blockchain (AMB)
- A service by AWS built on Blockchain with reliable APIs and without specialized infrastructure that powers your application with actionable, real-time blockchain data, allowing you to focus on innovation and speed to market with fully managed blockchain infrastructure.

##### AWS Device Farm
- AWS Device Farm is a cloud-based application testing service that lets developers test iOS, Android and Fire OS apps on real, physical mobile devices and desktop browsers hosted by Amazon Web Services.

##### AWS Service Catalog
- Constraints 
	- Template constraint
	- Launch constraint

##### AWS Data Exchange
- AWS Data exchange is a service for securely sharing and using third-party data on AWS.
- With AWS Data Exchange data is stored in an Amazon S3 bucket and is securely shared with the subscribers as a product, subscribers can browse and subscribe to any data set from the AWS Data exchange catalog in AWS Marketplace.

##### Amazon Timestream
- it is a fast, scalable, serverless, time-series database service from AWS.
- purpose-built for IoT, operational monitoring (DevOps), and application telemetry, efficiently storing and analyzing trillions of time-stamped data points to provide real-time analytics and insights at a fraction of the cost of traditional databases.
- support for scheduled queries

##### DynamoDB
- DAX for read performance optimization

##### Points for Exam


