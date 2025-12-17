
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