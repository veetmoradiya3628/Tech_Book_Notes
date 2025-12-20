
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
