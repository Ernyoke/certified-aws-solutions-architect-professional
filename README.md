# AWS Certified Solutions Architect – Professional (SAP-C01) Notes

## Table of Contents

1. Permissions, Identity and Federation
    - [MFA](01-identity/mfa.md)
    - [IAM](01-identity/iam.md)
    - [STS](01-identity/sts.md)
    - [AWS Resource Access Manager - RAM](01-identity/ram.md)
    - [AWS Service Quotas](01-identity/service-quotas.md)
    - [SAML 2.0 Identity Federation](01-identity/saml.md)
    - [AWS Single Sing-On - SSO](01-identity/sso.md)
    - [Amazon Cognito](01-identity/cognito.md)
    - [Amazon Workspaces](01-identity/workspaces.md)
2. Networking and Hybrid
    - [VPC - Virtual Private Cloud](02-networking/vpc.md)
    - [BGP - Border Gateway Protocol](02-networking/bgp.md)
    - [AWS Global Accelerator](02-networking/global-accelerator.md)
    - [DX - Direct Connect](02-networking/direct-connect.md)
    - [Route53](02-networking/route53.md)
3. Storage Services
    - [FSx](03-storage/fsx.md)
    - [EFS - Elastic File System](03-storage/efs.md)
    - [S3](03-storage/s3.md)
    - [EBS and Instance Store](03-storage/ebs.md)
4. Compute, Scaling and Load Balancing
    - [Regional and Global AWS Architecture](04-compute/aws-architecture.md)
    - [EC2](04-compute/ec2.md)
    - [ELB - Elastic Load Balancer](04-compute/elb.md)
    - [ASG - Auto Scaling Groups](04-compute/asg.md)
5. Monitoring, Logging and Cost Management
    - [CloudWatch](05-monitoring/cloudwatch.md)
    - [CloudTrail](05-monitoring/cloudtrail.md)
    - [AWS X-Ray](05-monitoring/xray.md)
    - [Cost Allocation Tags](05-monitoring/cost-allocation-tags.md)
    - [AWS Trusted Advisor](05-monitoring/trusted-advisor.md)
    - [AWS Billing and Cost Management](05-monitoring/billing.md)
6. Databases
    - [RDS - Relational Database Service](06-databases/rds.md)
    - [DynamoDB](06-databases/dynamodb.md)
    - [AWS ElasticSearch - ES](06-databases/elasticsearch.md)
    - [Amazon Athena](06-databases/athena.md)
    - [Amazon Neptune](06-databases/neptune.md)
    - [Amazon Quantum Ledger Database - QLDB](06-databases/quantum-ledger.md)
7. Data Analytics
    - [Kinesis](07-data-analytics/kinesis.md)
    - [EMR - Elastic Map Reduce](07-data-analytics/emr.md)
    - [Amazon Redshift](07-data-analytics/redshift.md)
    - [AWS Batch](07-data-analytics/aws-batch.md)
    - [AWS Quicksight](07-data-analytics/quicksight.md)
8. App Services, Containers and Serverless
    - [ECS - Elastic Container Service](08-containers-and-serverless/ecs.md)
    - [SNS - Simple Notification Service](08-containers-and-serverless/sns.md)
    - [SQS - Simple Queue Service](08-containers-and-serverless/sqs.md)
    - [Amazon MQ](08-containers-and-serverless/mq.md)
    - [AWS Lambda](08-containers-and-serverless/lambda.md)
    - [CloudWatch Events and EventBridge](08-containers-and-serverless/eventbridge.md)
    - [API Gateway](08-containers-and-serverless/api-gateway.md)
    - [AWS Step Functions](08-containers-and-serverless/step-functions.md)
    - [SWF - Simple Workflow Service](08-containers-and-serverless/swf.md)
    - [Amazon Mechanical Turk](08-containers-and-serverless/mechanical-turk.md)
    - [Elastic Transcoder and AWS Elemental MediaConvert](08-containers-and-serverless/mediaconvert.md)
    - [AWS IOT](08-containers-and-serverless/iot.md)
9. Caching, Delivery and Edge
    - [CloudFront](09-caching/cloudfront.md)
    - [ElastiCache](09-caching/elasticache.md)
10. Migrations and Extensions
    - [The 6R's of Cloud Migrations](10-migrations/6r.md)
    - [DMS - Database Migration Service](10-migrations/dms.md)
    - [VM Migrations AWS <=> On-Premises](10-migrations/vm-migration.md)
    - [Storage Gateway](10-migrations/storage-gateway.md)
    - [Snowball and Snowmobile](10-migrations/snow.md)
    - [AWS DataSync](10-migrations/datasync.md)
11. Security and Configuration Management
    - [AWS GuardDuty](11-security-and-config/guard-duty.md)
    - [AWS Config](11-security-and-config/config.md)
    - [AWS Inspector](11-security-and-config/inspector.md)
    - [Encryption and KMS](11-security-and-config/kms.md)
    - [CloudHSM](11-security-and-config/cloudhsm.md)
    - [AWS Certificate Manager - ACM](11-security-and-config/acm.md)
    - [AWS Systems Manager Parameter Store](11-security-and-config/parameter-store.md)
    - [AWS Secrets Manager](11-security-and-config/secrets-manager.md)
    - [VPC Flow Logs](11-security-and-config/vpc-flow-logs.md)
    - [AWS Shield and Web Application Firewall (WAF)](11-security-and-config/waf-shield.md)
12. Disaster Recovery and Business Continuity in AWS
    - [DR/BC Architecture](12-disaster-recovery/dr.md)
13. Infrastructure as Code
    - [CloudFormation](13-iac/cloudformation.md)
14. Deployment and Management
    - [Service Catalog](14-deployment/service-catalog.md)
    - [CI/CD](14-deployment/cicd.md)
    - [Elastic Beanstalk - EB](14-deployment/eb.md)
    - [AWS OpsWorks](14-deployment/opsworks.md)
    - [AWS Systems Manager (SSM)](14-deployment/ssm.md)
15. Everything Else
    - [Amazon Lex and Amazon Connect](15-other/lex.md)
    - [AWS Rekognition](15-other/rekognition.md)
    - [Kinesis Video Streams](15-other/kinesis-video-streams.md)
    - [AWS Glue](15-other/glue.md)
    - [AWS Device Farm](15-other/device-farm.md)

## Exam Description

The AWS Certified Solutions Architect - Professional (SAP-C01) examination is intended for individuals who perform a solutions architect professional role. This exam validates advanced technical skills and experience in designing distributed applications and systems on the AWS platform.
It validates an examinee's ability to:
- Design and deploy dynamically scalable, highly available, fault-tolerant, and reliable applications on AWS.
- Select appropriate AWS services to design and deploy an application based on given requirements.
- Migrate complex, multi-tier applications on AWS.
- Design and deploy enterprise-wide scalable operations on AWS.
- Implement cost-control strategies.

## Preparation

- Official Exam Guide: [https://d1.awsstatic.com/training-and-certification/docs-sa-pro/AWS-Certified-Solutions-Architect-Professional_Exam-Guide.pdf](https://d1.awsstatic.com/training-and-certification/docs-sa-pro/AWS-Certified-Solutions-Architect-Professional_Exam-Guide.pdf)

- The exam may contain two types of questions:
    - Multiple choice
    - Multiple response

- Minimum passing score: 750/1000
- Number of questions: 75
- Time: 190 minutes to complete the exam (with a possibility to request 30 minutes extra for non-native english speakers)

## Exam Content

| **Domain**                                              | **% of Examination** |
|---------------------------------------------------------|----------------------|
| Domain 1: Design for Organizational Complexity          | 12.5%                |
| Domain 2: Design for New Solutions                      | 31%                  |
| Domain 3: Migration Planning                            | 15%                  |
| Domain 4: Cost Control                                  | 12.5%                |
| Domain 5: Continuous Improvement for Existing Solutions | 29%                  |
| **Total**                                               | **100%**             |

## Credits

- These notes are based on the [AWS Certified Solutions Architect - Professional course](https://learn.cantrill.io/p/aws-certified-solutions-architect-professional) by Adrian Cantrill and on [Ultimate AWS Certified Solutions Architect Professional 2021](https://www.udemy.com/course/aws-solutions-architect-professional/) by Stephane Maarek
- All images present in the notes are taken from Adrian Cantrill's GitHub [repository](https://github.com/Ernyoke/aws-sa-pro) for his course

## My other AWS Certification Notes

- [AWS Certified Developer – Associate (DVA-C01)](https://github.com/Ernyoke/certified-aws-developer-associate-notes)
- [AWS Certified Solutions Architect – Associate (SAA-C02)](https://github.com/Ernyoke/certified-aws-solutions-architect-associate)
- [AWS Certified SysOps – Associate (SOA-C01)](https://github.com/Ernyoke/certified-aws-sysops-associate)
- [AWS Certified DevOps Engineer – Professional (DOP-C01)](https://github.com/Ernyoke/certified-aws-devops-professional)
