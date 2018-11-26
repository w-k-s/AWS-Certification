# Services that will be examined for Certified Solution Architect - Associate

1. AWS Global Infrastructure

- Regions
- Availability Zones
- Edge Locations

2. Compute

- **EC2**: Virtual Machines inside the AWS platform
- **EC2 Container Services**: Manage Docker containers at scale
- **Elastic Beanstalk**: handles the deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring based on the code you upload to it
- **Lambda**: Serverless Computing
- **LightSail**: Virtual Private Server service. 
- **Batch**: Batch Computing in the cloud.

3. Storage

- **S3 (Simple Storage Service)**:  offers durable, available storage for flat files
- **Glacier**: for long term data archival

4. Databases

- **RDS**: Relational Database Service (SQL, MySQL, MariaDB, PostgreSQL, Aurora, and Oracle)
- **DynamoDB**: Non-relational databases
- **Elasticache**: Caches results of common queries (e.g. top products)
- **RedShift**: Data warehousing

5. Migration

- **AWS Migration Hub**: Track application as you migrate to AWS
- **Application Discovery Service**: Track dependencies for your applications
- **Database Migration Service**: migrate your database to AWS
- **Server Migration Service**: Migrate your physical or virtual server to AWS
- **Snowball**: Migrating large amount of data to the cloud.

6. Networking & Content Delivery

- **[!]VPC (Virtual Private Cloud)**: Virtual Data Center. Manually configure firewalls, availability zones, route tables
- **CloudFront** : Content Delivery Network
- **Route53**: DNS Server 
- **API Gateway**: handles all the tasks involved in accepting and processing up to hundreds of thousands of concurrent API calls, including traffic management, authorization and access control, monitoring, and API version management
- **[!]Direct Connect**:  Directly connect your headoffice or datacenter to AWS

7. Management Tools

- **CloudWatch**: Monitoring tool
- **CloudFormation**: an automated provisioning engine designed to deploy entire cloud environments via a JSON script
- **CloudTrail**: Logs changes to AWS environment
- **Config**: AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources
- **OpsWorks**: configuration management service that enables your system administrators to configure and operate your web applications
- **ServiceCatalog**: Manage approved list of it services (not in the exam)
- **SystemManager**: Manage EC2 systems (not in the exam) 
- **[!]Trusted Advisor**: Offers securit advuce (e.g. open ports, how to use aws affectively)
- **Managed Servicess**: (not in the exam)

8. Analytics

- **Athena**: SQL queries on S3 buckets
- **[!]EMR (Elastic MapReduce)**: Processing large amounts of data
- **CloudSearch**: 
- **ElasticSearch service**: 
- **[!]Kinesis**: used for collating large amounts of data streamed from multiple sources
- **Kinesis Video Streams**: 
- **QuickSight**: Business Intelligence
- **[!]Data pipeline**: Migrate data between AWS services

9. Security & Identity and compliance

- **[!IAM]**:  add users to your AWS account and set password rotation policies for these new users
- **Cognito**: Authenticate services for temporary access to AWS for mobile devices  
- **Guard Duty**: Monitors for malicious activity
- **Inspector**: install on EC2 to check for security vulnerabilities
- **Macie**: Iterates over S3 documents to look for PII (Personal Identifiable Information)
- **Cerificate Manager**: SSL cerificate
- **[!]CloudHSM**: Store secure keys
- **Directory Service**: Integrate Microsoft Active Directory service with AWS
- **[!]WAF (Web Application Firewall)**: Prevents SQL injects, XSS, 
- **[!]Shield**: Prevents DDoS
- **Artifact**: Download AWS Compliance Reports

10. Applicaion Integration

- **Step Functions**: Managing lambda functions (not in exam)
- **AmazonMQ**: Message Queues
- **SNS**: Notification service (e.g. billing)
- **SQS**: Queueing service, decoupling infrastructure
- **SWF**: Workflow

11. Desktop & App Streaming.

- **WorkSpaces**: VDI system, stream OS to device (not in exam)
- **AppStream 2**: Stream application to device (not in exam)