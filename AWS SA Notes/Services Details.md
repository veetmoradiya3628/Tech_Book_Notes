
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