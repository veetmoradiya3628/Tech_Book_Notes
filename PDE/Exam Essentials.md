
- Cloud Architecture Framework
	- Shared responsibility & shared fate
	- Security principles
	- Risk & Asset management
	- Identity & Access management
	- Compute & Container security
	- Network security
	- Data security
	- Secure Application Deployment
	- Compliance & Sovereignty
	- Privacy & Threat Monitoring
- Database Migration Service
	- serverless, managed tool that enables secure, low-downtime migration to Google cloud.
	- Homogeneous migration - similar database engine
	- Heterogeneous migration - CDC based replication, migration across different engines
- Migration strategies
	- Rehost - Life & shift with minimal changes
	- Replatform - Optimize after migration
	- Refactor - Re-engineer for cloud-native use
	- Re-architect - Modernize for scalability
	- Rebuild - Replace existing with new cloud-native apps
	- Repurchase - Move to SaaS based solutions
- Google Cloud Adoption Framework
	- Key pillers
		- system design
		- operational excellence
		- security
		- reliability
		- cost optimization
		- performance optimization
	- Migration phases
		- Assess, Plan, Deploy, Optimize
- Reliability
	- Measures
		- SLI - Service Level Indicator - Measures user satisfaction
		- SLO - Service Level objective - The target value for SLI
		- SLA - Service Level Agreement - A formal contract with users that outlines what happens if SLOs are missed
		- Error budget - how much downtime is allowed
	- Scale & Availability
		- Redundancy
		- Multi-zone & Multi-Region Architecture
		- Disaster Recovery
		- Degrade Gracefully
	- Operational Rollouts
		- Progressive rollouts
		- Automation
		- Testing & Recovery
	- Reliability for Data Engineering
		- Durability
		- High availability
		- Data consistency
		- Recovery
		- Disaster planning
		- Autoscaling
		- Graceful decommissioning
	- Best Practices
		- Backup & recovery
		- Least privilege access control
		- Materialized views in Big Query
		- Monitor SLIs & SLOs
		- Cross-Region storage
		- Autoscaling
		- CI/CD
		  Security
- Migration phases
	- Define starting point
	- Define workload type
	- Choose migration strategy
	- Assess cloud readiness
	- Define migration path
		- Assessment phase
		- Planning & foundation
		- Deployment approaches
		- Optimization
- Data governance in BigQuery
	- Access Control
		- IAM Roles
		- Column & Row level access
		- VPC Service Controls
	- Audit logging
	- Data stewardship
	- Encryption
	- Metadata management
		- Data Catalog
	- Data Quality
		- Powered by Dataplex

- Streaming data pipeline
	- Three stages
		- Ingestion
		- Processing
		- Storage
	- Near real-time insights
	- Key components
		- Ingestion via Pub/Sub
		- Processing via Cloud Run / Dataflow
			- Cloud Run - Lightweight processing using stateless containers
			- Dataflow
				- Apache beam based, exactly-once processing
				- Ideal for windowing, filtering, joins and aggregations
				- PCollections & PTransforms
		- Storage - BigQuery, Cloud Storage or BigTable
	- Define SLOs
	- Windowing & triggers
	- Dataflow template for reusability
	- Drain Job
- Cloud Deploy
	- Fully managed CI/CD service
	- Delivery pipeline
	- Targets
	- Releases
	- Automation
	- Pipeline management
	- Canary Deployment
	- Scoped IAM roles
	- Manifest
	- Rendering
	- YAML & Skaffold configs
- Cloud Scheduler
	- Fully managed Cron job service for running scheduled tasks to trigger jobs or automate infrastructure operations.
	- At least once delivery
	- retry policies
	- Integrates with HTTP/S, Pub/Sub and App Engine
	- IAM roles for job-level access control
	```
	* * * * *
	  1. Minute (0 - 59)
	  2. Hour (0 - 23)
	  3. Day of the month (1 - 31)
	  4. Month (1 - 12; or JAN to DEC)
	  5. Day of the week (0 - 6; or SUN to SAT; or 7 for S)   
	```
- Orchestration in Google Cloud
	- Orchestration connects multiple tasks / services into a unified workflow to streamline execution, automate processes, and optimize performance.
	- Cloud Scheduler vs. Workflows vs. Cloud Composer
	- Cloud Scheduler
		- Scheduling standalone jobs
	- Workflows
		- Low-latency, real-time workflows involving multiple services
	- Cloud Composer
		- Apache Airflow based
		- Data pipelines with complex dependencies
- Workload management using Reservations
	- Billing models
		- On-demand
			- Pay per byte processed at query time
		- Flat-rate
			- Reserve slots monthly or annually for predictable workloads
		- Flex slots
			- Short-term (60-sec min) slots reservations, ideal for testing or burst workloads
	- Slots
	- Commitment
	- Reservations
	- Assignment
	- Idle slots
	- Regional Restriction
- Cloud Composer
	- Managed Apache Airflow for orchestrating data pipelines across hybrid and multi-cloud.
	- Ideal for batch workloads
	- Fully managed Airflow
	- Python based DAGs
	- Environment components
	- Monitoring
	- Auto-scaling
	- GCP service integrations
	- CI/CD pipelines for DAG deployment
- Cloud Storage
	- Scalable, durable and secure object storage for unstructured data
	- 11 9's availability
	- Choose right storage class (Standard, Nearline etc)
	- Use lifecycle policies for cost efficiency
	- Enable Cloud CDN for edge caching
	- Track object changes with Cloud Functions
	- Access control
	- Signed URLs
	- Storage class
		- Access frequency
		- Standard - frequently accessed data
		- Nearline - data accessed once a month
		- Coldline - data accessed once a quarter
		- Archive - rarely accessed data (once / year)
	- Multi-region or Dual regional offer higher availability
	- Retrieval fees
	- Outbound data (egress) costs money
	- Best practices
		- Object Lifecycle management
		- Monitoring & Budgets
		- Avoid Object versioning
		- Data compression & delta transfers
		- Tag resources
	- Use cases
		- Hosting static assets
		- Long-term
		- Storage for analytics datasets, ML model inputs / outputs
-  Data Lake
	- A data lake is a centralized repository designed to store, process and secure large volumes of data.
	- It can store any format (structured, semi-structured, unstructured)
	- Components
		- GCS
		  BigQuery
		- Dataflow / Dataproc
		- Data Catalog
	- Data Catalog for metadata, lineage and search
	- Dataplex for automatic discovery and classification
	- Encryption
		- Default encryption - AES 256
		- CMEK - Customer managed keys
		- CSEK - Customer supplied keys
	- IAM access levels (Uniform vs. Fine-grained)
- Memorystore for Redis cluster
	- It is a fully managed service powered by Redis in-memory data store to build a highly available and scalable application cache that offers sub-millisecond data access
	- Redis vs. Redis cluster vs. Memcached
	- Tiers
		- Basic
		- Standard
	- Caching, Gaming and Streaming are the usecases
- BigQuery
	- BigQuery is a fully managed, serverless, petabyte scaled low-cost data warehouse service for analytics.
	- Supports SQL & ML
	- Separate compute & ML
- Datalake modernization
	- Data Lake modernization is the process of enhancing and strengthening existing data lake infrastructure to make it more secure, scalable, fast and accessible through leveraging GCP services.

- BigQuery "Automatically detect" schema is for scenarios where the schema of files occasionally changes.
- Partitioning by date is a good practice for improving query performance and reducing costs for frequently queried recent data
- Federated query with cloud storage
- Bigtable is for high-throughput, low-latency access to key-value data
- BigQuery nested and repeated fields allows for flexibility in handling varying schema structures
- BigQuery does not have built-in triggers to handle deduplication, built in message deduplication feature
- Bigtable raw design matters for effective separation and performance in range queries
- Permanent tables in BigQuery allows for efficient querying of aggregate values

- BigQuery
	- BigQuery is a serverless, AI-ready, fully managed, scalable, multi-engine, multi-format, multi-cloud data analytics platform that helps you extract the most value from your data.
	- Features
		- Serverless Architecture
		- High performance
		- GoogleSQL
		- ML with BigQuery ML
		- Vertex AI Integration
		- Gemini AI
		- BI Engine
		- Analytics Hub
	- ML models in BigQuery
		- CREATE MODEL
		- ML.EVALUATE
		- ML.PREDICT
	- Views
		- Views are virtual tables defined by SQL queries
		- they are read-only and do not store any data.
		- Types of views
			- Logical View
				- No data stored, Reflects live changes, Lower cost, slower query
			- Materialized View
				- Stores precomputed results
				- Periodic refresh (default: 30 minutes)
				- Faster, costlier
		- INFORMATION_SCHEMA contains view metadata
- Looker studio is a no-cost self-service data visualization tool that allows you to create and consume dashboards and customizable reports from various data sources.
- Query modes
	- Interactive mode
		- Triggered by user interaction
		- Real-time, but more costly
		- Limited concurrency
	- Batch queries
		- Scheduled, cheaper
		- Ideal for recurring reports
- Cloud IAM
	- Cloud Identity and Access Management (IAM) allows users to manage access control for specific Google Cloud resources and helps prevent access to other resources.
	- Core components
		- Principal - User account, Service account
		- Role - Basic, Predefined, Custom
		- Policies - Allow, Deny
		- Resources - Organization, Folder, Project, Resources
- GCP Observability
	- Cloud logging - Managed log monitoring and analytics
	- Cloud Monitoring - Metrics, events, dashboards and alerts
	- Cloud Trace - Distributed tracing for latency and performance insights
	- Cloud Profiler - CPU and memory profiling
	- Error Reporting - Centralized error management and analytics
- Cloud Key Management
	- KMS 
	- Symmetric / asymmetric
	- Hardware Security Module (HSM)
	- Customer-managed encryption keys (CMEK)
	- External Key Manager (EKM) support
- Google Cloud Encryption
	- Simple text to ciphertext conversion
	- Encryption at rest and encryption in transit
	- CMEK
	- Encryption in transit - TLS, ALTS, AES GCM
	- DEK, KEKs
- Monitoring and troubleshooting process in GCP
	- Dataflow - Metrics, Job monitoring, failure logs
	- BigQuery - Query metrics, data scans, execution time
	- DataProc - Cluster and job monitoring
	- Pub/Sub - Message throughput, delivery latency monitoring
	- Cloud Storage - Metrics for errors, data transfer and requests
- DR - Disaster recovery
	- Zones / Regions
	- Replication strategies
		- Sync / Async
	- Automated failover - Hot / warm recovery based on RTO
- Data Analytics
	- Connect to BI Tools
	- Materialized views & pre calculated fields
	- Define time granularity
	- Troubleshoot query performance
	- IAM & Cloud DLP

- BigQuery BI Engine with precomputed materialized views allows for quick access to aggregated data.
- BigQuery as a data warehouse solution offers scalability, low maintenance and efficient handling of large datasets for data analytics.
- Analytics Hub allows centralized management of data access, ensuring security and control while providing third-party companies with access to the dataset.
- Cloud Natural Language API for quick implementation of generated Entity analytics or subject labels.
- Centralized log management
- Authorized views in BigQuery allows the organization to share aggregated data summaries while controlling access to underlying user-level data.
- Logistic regression for binary classification
- Linear regression for prediction of value
- k-means clustering is unsupervised algorithm for clustering
- Cloud AutoML for batch prediction task
- Vertex AI Online Prediction is the most suitable option for low latency, scalability and seamless model updates for real-time predictions

- Intermittent Workloads - Job based clusters are designed to be created and terminated on-demand, making them ideal for tasks that don't require a continuous cluster. This flexibility allows the company to pay only for the resources used during the processing, reducing costs.
- Predictable, Ongoing tasks - Persistent clusters offer a more stable and predictable environment, making them suitable for long-running processes. These clusters are always running, ensuring consistent performance and availability.
- As DAGs in cloud composer allow organizations to define workflow dependencies and schedule job execution in a repeatable and reliable manner, ensuring timely execution of critical tasks.
- Pricing model
	- Flat-rate slot pricing - Flat-rate slot pricing offers fixed capacity for predictable workloads. It allows users to pay a flat rate for a set number of slots, regardless of usage, providing stability in pricing for steady workloads
	- Flex slot pricing - Flex slot pricing offers flexibility and dynamic allocates slots based on demand, making it suitable for fluctuating workloads.
	- On-demand pricing - On-demand pricing charges users based on usage, without any fixed commitments, making it suitable for sporadic or unpredictable workloads.
	- Pay as you go is similar to on-demand pricing 
- Auto scaling policies allows for dynamic adjusting resource allocation based on workload demands, ensuring optimal resource utilization and cost-efficiency for both critical and non-critical data processes.
- Exporting relevant information to Cloud monitoring and configuring an alerting policy aligns with the requirement to perform health checks, monitor behavior, and notify the team promptly in case of pipeline failures. This approach utilizes managed products and features for effective monitoring across multiple projects.
- Failover replica vs. Read replica
- AutoML to quickly build models. ML always split training data into 70-30% where 70% for training and 30% after that for testing the model
- ML types
	- Regression - output variable is a continuous value, supervised
	- Classification - output variable is a category, supervised
	- Clustering - An unsupervised learning method to find references between input data without labeled output
	- Reinforcement - its learning technique where a machine takes actions without training sets to reach the highest rewards possible.
- For ML model overfitting prevention increase the training set, decrease features parameters, increase regularization
- GCP Cloud NLP - service is to derive insights from unstructured text revealing meaning of the documents and categorize articles.
- Speech to text - caption on video etc
- Auto ML Vision API - service to recognize and derive insights from images by either using pre-trained models or training a custom model based on set of photos
- ML Engine - managed service for custom model develop, design and deploy in prod
- Ephemeral dataproc cluster is recommended best practice to save the cost, spin a cluster with Preemptible VM
- Storage transfer service allows you to quickly import ONLINE data into cloud storage.
- Transfer Appliance is an OFFLINE secure, high capacity storage server that is used for huge data transfer
- BigQuery sql - to refer table its always \`\<table\>\`
	- TABLE_SUFFIX - for wild card table scan with particular suffix

- Dataflow pipeline to replace an existing pipeline, a new pipeline needs to be created with the same job name.
- flag -update to be passed as well also `-transformNameMapping` needs to be supplied for updating transformation name mapping
- BigQuery connector can be used as input and output source for Dataproc cluster by using BigQuery Connector. There are 3 ways BigQuery connectors can be used
	- Installing BigQuery connector using initialization action.
	- specifying BigQuery connctor in the jars parameter when submitting a job. the jar could be placed on cloud storage and a path to the jar is provided.
	- BigQuery connector classes can be included as dependencies in your code.
- BigQuery with permanent table with "automatically detect" schema changes detect and source can be in cloud storage
- BigQuery we can export data in several formats, it has native support to export data in CSV, JSON or AVRO format 
- Cloud Composer is recommended over Apache Airflow and also composer support multi cloud pipeline orchestration
- Ingestion table in BigQuery will have two pseudo column called `_PARTITIONTIME` and `_PARTITIONDATE`.
- Preemptible workers can not store data, it only functions as a processing nodes.
- Cloud Vision API product search we can easily train by providing reference images. The API will allow the user to query the product catalog based on the new image as input and will fetch the best-matching products
- Possible ways to load data into BigQuery
	- Batch Ingestion
	- Stream ingestion
	- Data Transfer Service (DTS) - to load data from other SaaS products
	- Partner Integrations
	- Query Materialization - This is the best way to simplify extract, transform, and load patterns in BigQuery. Using federated queries in BigQuery, one can persist their analysis results in BigQuery to derive any insights.
- Cloud Pub/Sub limits
	- retention period 7 days
	- min 10 minutes to 31 days retention period
- Cloud BigTable is designed for high-throughput and low-latency workloads, making it ideal for real-time applications like inventory tracking.
- ParDo for filtering a dataset.
- Transfer Application service for large / huge dataset
- Data Fusion is similar to CDAP (Cask Data Application Platform)
- Cloud Vision API - limit 20 MB single image
- Avro is recommended data format for BigQuery
- Storage transfer service can be used when transferring more than 1 TB from another cloud storage service

- Pub/Sub stream events can directly configured to ingest data into BigQuery using BigQuery subscription
- Ephemeral Dataproc cluster
- Preemptible VMs
- Object Lifecycle management
- Online prediction vs. Batch Prediction in Vertex AI
- If there are multiple BigQuery projects and users you can manage costs by requesting a user-level custom quota that specifies  a limit on the amount of query data processed per day.
- BigTable cluster - there is no option to change the cluster configuration once created, only way to change if by deleting and recreating new one
- Scheduled run by Cloud Scheduler
- For timeseries data on Big Table you should generally use tall and narrow tables
- Performance test on BigTable
	- Use production instance
- Precision
	- Out of all the examples the model predicted as positive, how many were actually positive ?
- Recall
	- Out of all the actual positive examples in the data, how many did the model manage to find ?
- Hyperparameter tuning effcts
	- Number of nodes in hidden layers
	- Number of hidden layers
- STS - Storage transfer service - online transfer
- Transfer Appliance is an offline secure, high capacity storage server that we setup in our datacenter. 

- Sliding window vs. Tumbling window vs. Hopping window vs. Session window vs. Global window
- Dataflow job stopping two options 
	- Cancel
	- Drain
- BigQuery to GCS below options are supported
	- CSV
	- JSON
	- Avro
	- Parquet
- GZIP, DEFLATE, SNAPPY are only supported compression types while exporting data to GCS

---------------
1. Google Cloud Dataflow 
	- fully managed service for both batch and stream processing. It supports windowing and handles late-arriving data, making it suitable for processing streaming data with delayed events.
	- Windowing functions for efficiently handling late-arriving data
	- Sliding window for irregular arriving data
	- GCP Pub/Sub with Dataflow for exactly once processing in real-time
2. Google Cloud Dataproc 
	- Designed for processing batch and interactive big data jobs using Apache Spark and Apache Hadoop.
	- batch mode
3. Google Cloud Pub/Sub 
	- Messaging service for real-time event-driven systems.
4. Google Cloud Bigtable 
	- NoSQL database
5. Google Cloud Build
	- Fully managed CI/CD platform that automates the testing, building and deployment of applications, including data pipelines.
6. Google Cloud Dataprep
	- Visual data preparation tool
7. Google Cloud Composer
	- Workflow orchestration service
	- Supports custom sensor to take action and re-run or trigger pipeline etc
8. Google Cloud Datafusion
	- Designed for data integration
9. Google Cloud Database Migration Service
	- Designed specifically for database migrations
10. Data Transfer Appliance
	- Suitable for physical transfer
11. Cloud Spanner
	- globally distributed, horizontally scalable transactions.
	- Secondary indexes in cloud spanner allows for efficient range queries on non-key columns
	- supports horizontal scaling across continents
12. Cloud Monitoring
	- Centralized logging and monitoring capabilities for Google cloud services, including data processes such as Dataproc and Dataflow.
13. Cloud Logging
	- Google Cloud Logging offers centralized logging capabilities
14. Cloud Trace
	- Cloud Trace is primarily focused on distributed application tracing, providing insights into application performance, rather than centralized logging and monitoring for data processes.
15. Cloud Audit Logging
	- Cloud Audit logging is specifically designed for tracking and logging user access and system activity within Google Cloud Platform services, not for monitoring data processes.
16. Cloud Scheduler
	- Allows automated and repeatable task scheduling, ensuring timely execution based on predefined schedules.

- Dataform (ELT SQL) 
- Dataflow (Stream Beam ETL)
- DataPrep (Data wrangling) 
- DataFusion (ETL UI easy way to manage - 3rd party trifacta)
- Analytics Hub (Data sharing across the org)
- Knowledge catalog


Big Table
- Row Key, Column Family, Column, Timestamp, Value
- Row key design should have hash(col)#timestamp or user#timestamp
- Tablets & tablet server - divide the row key range in multiple machines
- Use cases
	- Time series
	- Financial tick data
	- User Profile
	- Recommendations engine
	- Adtech
	- Fraud detection
	- IoT

BigQuery
- OLTP vs. OLAP
- OLAP engine
- It's
	- Distributed SQL Engine
	- Columnar Storage
	- Massively Parallel Processing
	- Serverless
- Separation of Compute and Storage
- Based on Google's Dremel engine
- Query execution tree
- Query Planner
- Slots
	- Virtual CPU
	- Memory
	- Execution resource
- Shuffle
- Partitioning and clustering
	- Partitioning
		- Time partition
		- Integer range partition
		- Ingestion time
	- Clustering
		- Choose blocks
- Materialized view
- Result cache
- Security
	- IAM
	- Column-level security
	- Row-level security
	- Authorized views
	- Encryption at rest / transit

Dataflow
- Based on Apache Beam
- Beam - Programming model
- Dataflow - Execution Engine
- Batch and Stream both supported
- PCollection - Collection of records
- Transform - Transforms modify data
- Windows
	- Fixed window
	- Sliding window
	- Session window
- Event time vs. Processing time
- Watermark
- Trigger - immediate internal result
- ETL
- Exactly-once processing semantics

Dataproc
- Manages infra structure
- Spark does a computation
- Ephemeral clusters
- Broadcast Join
- Managed Spark / Hadoop 

- Datawarehouse
	- Structured, cleaned, transformed, business-ready data optimized for analytics
	- BigQuery
	- Schema-on-write
- Data Lake
	- Raw data in its original format
	- Cloud storage
	- Schema-on-read 
- Data swamp
	- Poorely managed data lake
- Data Lakehouse
	- Modern architecture
	- cheap object storage from a Data Lake
	- reliability and governance of a Data Warehouse
	- Apache Iceburg & Open table format
- ETL
	- Extract, transform, load
- ELT
	- Extract, load, transform
- Reverse ETL
	- Warehouse data back to operational systems

```
                Data Sources
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
   Data Preparation       Data Movement
          │                     │
    Dataprep/Data Fusion   Dataflow
          │                     │
          └──────────┬──────────┘
                     ▼
          Data Lake / Warehouse
        (Cloud Storage, BigQuery)
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
     Governance           Batch Compute
      Dataplex             Dataproc
```

- Cloud Dataprep
	- Visual data cleaning tool (no-code/low-code)
	- Data cleaning
- Cloud Data Fusion
	- Visual ELT/ETL pipeline builder
	- Managed ETL pipeline
- Cloud Dataproc
	- Managed Spark/Hadoop cluster
	- Managed cluster
- Cloud Dataflow
	- Serverless distributed data processing
	- Serverless ETL
- Cloud Dataplex
	- Unified data governance and management
	- Governance
- Cloud Dataform
	- ELT
	- SQLx
- Datamesh
	- decentralized data ownership
- BigLake
	- Unified access layer for data lakes.
	- Open standards with Apache Iceburg
	- Allows BigQuery to analyze data stored directly in Cloud Storage while providing centralized governance.
- Datastream
	- CDC - Change data capture
- Analytics Hub
	- Secure data sharing across org
- Cloud Composer
	- Workflow orchestration (Managed Apache Airflow)

```
              Need to move data
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
   Database      Files/Objects   Huge Offline Data
   Migration      Transfer          Migration
        │            │                │
        ▼            ▼                ▼
Database        Storage         Transfer
Migration       Transfer        Appliance
Service         Service
        
   Continuous Database Changes
        │
        ▼
     Datastream
```

- Cloud DMS - Data Migration Service
	- Managed service for migrating databases into Google Cloud
	- Database Migration
- Datastream
	- Captures ongoing changes
	- Serverless CDC
- Storage Transfer Service
	- This moves Objects/Files not databases
	- Managed service for transferring object data
	- Online Object Transfer
- Transfer Appliance
	- Offline migration
	- High data migration with limited bandwidth
- BigQuery Data Transfer Service
	- Automatically loads data into BigQuery from supported Google Services and SaaS applications
	- Managed Scheduled Import

```
                Trigger
                   │
      ┌────────────┼─────────────┐
      ▼            ▼             ▼
   Time-based   Event-based   API/Workflow
      │            │             │
Scheduler      Eventarc     Workflows
      │            │             │
      └──────┬─────┴─────────────┘
             ▼
     Run Function / Cloud Run
             │
             ▼
        Dataflow
             │
             ▼
        BigQuery
```

- Cloud Composer
	- Apache Airflow
	- Pipeline Orchestration
- Cloud Scheduler
	- Cron in the cloud
- Cloud Workflows
	- Coordinates service-to-service calls
	- Service orchestration
- Cloud Run
	- Serverless containers
- Cloud Functions
	- Run a small function when an event occurs
	- Event-driven function
- Eventarc
	- Universal event router
- Cloud Tasks
	- Asynchronous task queue
- Batch
	- Managed batch job execution

- Three pillars to any production system
	- Logs
	- Metrics
	- Alerts
- Cloud Logging
	- Everything that happens
- Cloud Monitoring
	- Current health
- Dashboards
- Alerting
- Dead Letter Queue (DLQ)
	- A separate location where failed records are stored for later investigation and reprocessing.
	- Pubsub topic, cloud storage or Bigquery error table
- Best practices
	- Idempotency
	- checkpointing
	- Watermarks
	- Exactly-once processing

- BigQuery Schema Design
	- Denormalize instead of normalize
	- Use nestead and repeated fields
	- Partition tables
	- Cluster table with in partitions
	- Don't over-partition
	- Avoid SELECT *
	- Use Materialized views
	- Use BI engine cache
	- Choose correct file format (avoid CSV and prefer Avro, Parquet, ORC etc)
- BigTable schema design
	- Design Row key carefully
	- Avoid sequential keys
	- Store related data together

- Principle of least privilege
- Separation of duties
- Defense in depth
- Zero trust
- BigQuery IAM
	- Dataset level
	- Column level security
	- Row level security
- Data Encryption
- CMEK
- CSEK
- Cloud KMS

- PCollection
	- Immutable, distributed, processed in parallel, it takes a PCollection and produces another PCollection
- ParDo
	- Parallel do
	- for each element do something
- DoFn
	- ParDo executes a DoFn

DoFn Lifecycle

```
Setup()

↓

StartBundle()

↓

ProcessElement()

↓

FinishBundle()

↓

Teardown()
```

- MapElements
	- One input to One output
- FlatMapElements
	- One input to Many outputs
- Filter
	- Keeps only matching elements
- WithKeys
	- Adds key
- Keys
	- Extract keys only
- Values
	- Extract values only
- GroupByKey
- CoGroupByKey
- Combine
- Flatten
- Partition


- Classic template vs. Flex Template GCP Dataflow
	- **Classic (Standard) Templates serialize the execution graph at compilation time**, while **Flex Templates package the code into a Docker container and generate the graph dynamically at runtime**
- Vertex AI
- BigQuery ML
- DLP - Data Layer protection - FPE-FFX