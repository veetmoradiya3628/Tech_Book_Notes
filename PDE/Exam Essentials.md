
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
	- Fully managed cron job service for running scheduled tasks to trigger jobs or automate infrastructure operations.
	- at least once delivery
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





---------------
1. Google Cloud Dataflow 
	- fully managed service for both batch and stream processing. It supports windowing and handles late-arriving data, making it suitable for processing streaming data with delayed events.
	- Windowing functions for efficiently handling late-arriving data
	- Sliding window for irregular arriving data
	- GCP Pub/Sub with Dataflow for exactly once processing in real-time
2. Google Cloud Dataproc 
	- designed for processing batch and interactive big data jobs using Apache Spark and Apache Hadoop.
	- batch mode
3. Google Cloud Pub/Sub 
	- messaging service for real-time event-driven systems.
4. Google Cloud Bigtable 
	- NoSQL database
5. Google Cloud Build
	- fully managed CI/CD platform that automates the testing, building and deployment of applications, including data pipelines.
6. Google Cloud Dataprep
	- Visual data preparation tool
7. Google Cloud Composer
	- Workflow orchestration service
8. Google Cloud Datafusion
	- Designed for data integration
9. Google Cloud Database Migration Service
	- Designed specifically for database migrations
10. Data Transfer Appliance
	- Suitable for physical transfer