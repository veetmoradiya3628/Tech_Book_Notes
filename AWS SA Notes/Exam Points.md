
- EC2 build image - managed service for AMI management and RAM can be used to share this images across other AWS accounts / Organizations.

- OpenSearch - open-source distributed search and analytical service, kinesis data firehose can deliver logs to the OpenSearch for real-time analytics, also open search supports Dashboards

- If we want our lambda function with custom docker image then implement Lambda Runtime API in the container image

- Blue/Green deployment is production use case in EBS environment

- EBS provides option for managed update configuration where they can control the update level, The weekly update period.

- VPC endpoints and Private Link can be used with EBS for private connectivity

- There are two methods to add accounts to the AWS Organizations
	- Creating new account
	- By creating invitations

- Organization activity console provides an interface to see and identify services that are never used.

- AWS Migration Hub is a central place where the migration process can be visualized including the discovery phase. 
- Supported methods
	- Agentless collector
	- Discovery agent
	- Migration hub template
	- RVTools export

- SAML with IDP - 
	- Get SAML metadata doc
	- create SAML IDP
	- SAML IDP with a relying party trust
	- SAML assertions

- Amazon Cognito Identity pools provide temporary AWS credentials for users who are guests (unauthenticated) and for users who have been authenticated and received a token. An identity pool is a store of user identifiers linked to your external identity providers.
	- Identity pools authentication flow (https://docs.aws.amazon.com/cognito/latest/developerguide/authentication-flow.html)
	- Guest users can request temporary tokens by using the GetId API

- Amazon Cognito User pool, provides an way to create groups and assign IAM roles to them to assign the required permissions based on groups.

- API Gateway REST API cannot exceed 29 seconds due to the limits and quotas.

- CloudWatch logs for CPU and memory details of usage in AWS Lambda execution

- NAT Gateway is for IPv4 while for IPv6 support we need to use egress-only internet gateway

- VPC supports to add secondary IPv4 (CIDR) in existing VPC (VPC FQAs : https://aws.amazon.com/vpc/faqs/)

- EMR cluster with Hadoop is best suited for Batch processing

- AWS License manager is a central place to manage licenses in AWS EC2 and on-premises instances. It contains 3 parts to use:
	- Define licensing rules
	- Enforce licensing rules
	- Track usage

