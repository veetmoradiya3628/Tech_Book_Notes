
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

##### Amazon Redshift
- Amazon redshift is a fast and powerful, fully managed, petabyte-scale data warehouse service in the cloud. This service is highly scalable to a petabyte or more for $1000 per terabyte per year.
- Single node (160 GB)
- Multi-node
	- Leader and compute node
- Massively parallel processing (MPP)
- Maximum retention period is 35 days


#### End User Computing

##### Amazon Workspaces
 - TODO


##### Amazon GuardDuty
- Managed Thread detection service offered by AWS that continuously monitors and analyzes activity within AWS account to identify potential security threats and vulnerabilities.
- Region specific Integrate with CloudTrail, EventBridge & Lambda services

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
- Life cycle
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

##### Amazon Elastic Transcoder
- Amazon Elastic Transcoder delivers a cloud-based service for media transcoding, offering developers and businesses a scalable, intuitive and cost-effective method to convert media files into formats suitable for diverse devices, including smartphones, tablets and computers.
- Transcoding pipelines, jobs, system transcoding presets, custom transcoding presets

##### Amazon Managed Blockchain (AMB)
- A service by AWS built on Blockchain with reliable APIs and without specialized infrastructure that powers your application with actionable, real-time blockchain data, allowing you to focus on innovation and speed to market with fully managed blockchain infrastructure.

##### AWS Device Farm
- AWS Device Farm is a cloud-based application testing service that lets developers test iOS, Android and Fire OS apps on real, physical mobile devices and desktop browsers hosted by Amazon Web Services.