**S3**

- s3 bucket policy
- s3 enable static website hosting
- policy to secure the hosting bucket
- ARN - amazon resource name
- bucket policy & user policy
- AWS s3 encrypts data at the object level as it writes it to disks in the AWS data centers.
- SSE-S3 managed keys
- virtual hosted style urls vs. path-style urls. - aws supports both

**EC2**

- AWS EBS - Elastic block store - block storage service to be used with EC2
- AMI - Amazon Machine Image
- VPC - Subnets
- Security groups
- firewall rules (inbound, outbound)

**VPC**

- public subnet, private subnet
- route table
- Internet gateway
- Security group concept
- Elastic IP concept

**RDS**

- AWS managed relational database service
- support for read replica, master slave configuration and , multiple AZs availability
- fully managed backup and patching configuration
- AWS Data migration service used for migration of database.

**DynamoDB**

- Key value and Document database
- multi active and multi region scale
- flexible schema
- micro second latency for read
- encryption at rest
- ACID support
- PITR (Point in time recovery)
- On demand backup and restore
- SLA for availability
- No SQL Database models 
	- KeyValue - Amazon DynamoDB, 
	- Document - Amazon DynamoDB,
	- Graph DB - Amazon Neptune,
	- In Memory - Amazon ElastiCache,
	- Seach - Amazon ElasticSeach service
- Partition Key vs. Sort Key

**Elastic File System**
- shared file system over network
- serverless solution
- fully managed service
- elastic in nature (grows and shrinks based on requirements)
- servers can access shared data in Amazon EFS by using Mount targets in each AZs
- Multiple EC2 servers in different AZs can access same EFS by mounting it.
- ENI - elastic network interface
- Commands to attach EFS as file system directory in EC2
```
sudo -i

sudo yum install -y amazon-efs-utils

sudo mount -t efs -o tls fs-00eef3db270c65997:/ data
```

**VPC Peering**

- VPC peering allows route traffic between the VPCs by using private IP addresses.
- VPC peering does not support transitive peering relationships.
- VPC Peering steps includes
	- creating vpc peering instance
	- modification on router table of instance subnets
	- modification on security groups inbound rules


**Auto Healing & Scaling Applications**
- Auto scaling group is used to provide dynamic feature on EC2 scale.
- Amazon EC2 Auto scaling helps ensure that you have the correct number of EC2 instances available to handle the load on your application.
- You can specify min and max no. of instances configuration for ASG so that it can't go beyond the limits.
- Schedule scaling helps you setup  your own timings for scaling in and out.
- Amazon cloud watch alarm is configured to monitor CPU utilization greater than 70 percent.
- dynamic scaling policy can be configured to for scale configuration.
- Instance templates
- AMIs (Amazon Machine Image) is used to configure machine image for instance creation.
- Auto Scaling group
	- Cron 
	- Action configurations etc


**High Availability Web Applications**

![[Pasted image 20250126221958.png]]
- Amazon Route 53 provides DNS services to streamline domain management.
- Amazon Cloud Front is used to deliver static and dynamic content. CloudFront can cache frequently accessed content to decrease latency.
- Amazon Simple storage service (S3) is used to store static assets such  as images and video.
- Elastic load balancing is used to distribute traffic across multiple availability zones. Amazon EC2 Auto scaling group are deployed for redundancy.

**AWS IAM**
- AWS IAM - Identity and Access Management provides support to create policies that manage AWS users and groups, and you can use permissions to allow and deny access to AWS resources.
- Predefined policy and custom policy.
- Create and attach a user to the group membership.
- IAM User, Group and permissions.

Troubleshoot an AWS Cloud Architecture

![[Pasted image 20250126230243.png]]




- AWS Pricing calculator to estimate a pricing and budget for architecture.
- https://calculator.aws/#/
---
https://mail.google.com/mail/u/0/#starred/FMfcgzQZSZJxdRDzJLbLCQpSGSGLjQrQ
https://cloudquest.skillbuilder.aws/?role=d8f9243e-3348-4785-9cb3-27741ff1a719
