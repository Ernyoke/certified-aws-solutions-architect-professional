# AWS Certified Solutions Architect – Professional (SAP-C02) Notes

## Table of Contents

1. Advanced Permissions and Accounts
    - [MFA](01-accounts/mfa.md)
    - [IAM](01-accounts/iam.md)
    - [AWS Organizations](01-accounts/organizations.md)
    - [STS](01-accounts/sts.md)
    - [Policies](01-accounts/policies.md)
    - [AWS Resource Access Manager - RAM](01-accounts/ram.md)
    - [AWS Service Quotas](01-accounts/service-quotas.md)
2. Advanced Identities and Federation
    - [SAML 2.0 Identity Federation](02-identity/saml.md)
    - [IAM Identity Center (Successor to AWS Single Sing-On - SSO)](02-identity/identity-center.md)
    - [Amazon Cognito](02-identity/cognito.md)
    - [Amazon Workspaces](02-identity/workspaces.md)
    - [AWS Control Tower](02-identity/control-tower.md)
3. Networking and Hybrid
    - [VPC - Virtual Private Cloud](03-networking/vpc.md)
    - [VPNs](03-networking/vpn.md)
    - [AWS Transit Gateway](03-networking/transit-gateway.md)
    - [BGP - Border Gateway Protocol](03-networking/bgp.md)
    - [AWS Global Accelerator](03-networking/global-accelerator.md)
    - [DX - Direct Connect](03-networking/direct-connect.md)
    - [Route53](03-networking/route53.md)
    - [AWS PrivateLink](03-networking/privatelink.md)
4. Storage Services
    - [FSx](04-storage/fsx.md)
    - [EFS - Elastic File System](04-storage/efs.md)
    - [S3](04-storage/s3.md)
    - [Amazon Macie](04-storage/macie.md)
    - [EBS and Instance Store](04-storage/ebs.md)
    - [AWS Transfer Family](04-storage/transfer-family.md)
5. Compute, Scaling and Load Balancing
    - [Regional and Global AWS Architecture](05-compute/aws-architecture.md)
    - [EC2](05-compute/ec2.md)
    - [ELB - Elastic Load Balancer](05-compute/elb.md)
    - [ASG - Auto Scaling Groups](05-compute/asg.md)
    - [Gateway Load Balancers (GWLB)](05-compute/gwlb.md)
    - [Other Compute Related Products](05-compute/others.md)
6. Monitoring, Logging and Cost Management
    - [CloudWatch](06-monitoring/cloudwatch.md)
    - [CloudTrail](06-monitoring/cloudtrail.md)
    - [AWS X-Ray](06-monitoring/xray.md)
    - [Cost Allocation Tags](06-monitoring/cost-allocation-tags.md)
    - [AWS Trusted Advisor](06-monitoring/trusted-advisor.md)
    - [AWS Billing and Cost Management](06-monitoring/billing.md)
    - [AWS Compute Optimizer](06-monitoring/compute-optimizer.md)
7. Databases
    - [RDS - Relational Database Service](07-databases/rds.md)
    - [Amazon Aurora](07-databases/aurora.md)
    - [DynamoDB](07-databases/dynamodb.md)
    - [AWS OpenSearch (Formerly known as ElasticSearch)](07-databases/opensearch.md)
    - [Amazon Athena](07-databases/athena.md)
    - [Amazon Neptune](07-databases/neptune.md)
    - [Amazon Quantum Ledger Database - QLDB](07-databases/quantum-ledger.md)
8. Data Analytics
    - [Kinesis](08-data-analytics/kinesis.md)
    - [EMR - Elastic Map Reduce](08-data-analytics/emr.md)
    - [Amazon Redshift](08-data-analytics/redshift.md)
    - [AWS Batch](08-data-analytics/aws-batch.md)
    - [AWS Quicksight](08-data-analytics/quicksight.md)
    - [Other Data Analytics Related Products](08-data-analytics/others.md)
9. App Services, Containers and Serverless
    - [ECS - Elastic Container Service](09-containers-and-serverless/ecs.md)
    - [EKS - Elastic Kubernetes Service](09-containers-and-serverless/eks.md)
    - [SNS - Simple Notification Service](09-containers-and-serverless/sns.md)
    - [SQS - Simple Queue Service](09-containers-and-serverless/sqs.md)
    - [Amazon MQ](09-containers-and-serverless/mq.md)
    - [AWS Lambda](09-containers-and-serverless/lambda.md)
    - [CloudWatch Events and EventBridge](09-containers-and-serverless/eventbridge.md)
    - [API Gateway](09-containers-and-serverless/api-gateway.md)
    - [AWS Step Functions](09-containers-and-serverless/step-functions.md)
    - [SWF - Simple Workflow Service](09-containers-and-serverless/swf.md)
    - [Amazon Mechanical Turk](09-containers-and-serverless/mechanical-turk.md)
    - [Elastic Transcoder and AWS Elemental MediaConvert](09-containers-and-serverless/mediaconvert.md)
    - [AWS IOT](09-containers-and-serverless/iot.md)
    - [AWS SAM - Serverless Application Model](09-containers-and-serverless/sam.md)
    - [AWS App Runner](09-containers-and-serverless/app-runner.md)
10. Caching, Delivery and Edge
    - [CloudFront](10-caching/cloudfront.md)
    - [ElastiCache](10-caching/elasticache.md)
11. Migrations and Extensions
    - [The 6R's of Cloud Migrations](11-migrations/6r.md)
    - [DMS - Database Migration Service](11-migrations/dms.md)
    - [VM Migrations AWS <=> On-Premises](11-migrations/vm-migration.md)
    - [Storage Gateway](11-migrations/storage-gateway.md)
    - [Snowball and Snowmobile](11-migrations/snow.md)
    - [AWS DataSync](11-migrations/datasync.md)
12. Security and Configuration Management
    - [AWS GuardDuty](12-security-and-config/guard-duty.md)
    - [AWS Config](12-security-and-config/config.md)
    - [AWS Inspector](12-security-and-config/inspector.md)
    - [Encryption and KMS](12-security-and-config/kms.md)
    - [CloudHSM](12-security-and-config/cloudhsm.md)
    - [AWS Certificate Manager - ACM](12-security-and-config/acm.md)
    - [AWS Systems Manager Parameter Store](12-security-and-config/parameter-store.md)
    - [AWS Secrets Manager](12-security-and-config/secrets-manager.md)
    - [VPC Flow Logs](12-security-and-config/vpc-flow-logs.md)
    - [Application (Layer 7) Firewalls](12-security-and-config/application-firewalls.md)
    - [AWS Shield](12-security-and-config/shield.md)
    - [AWS Network and DNS Firewalls](12-security-and-config/network-and-dns-firewall.md)
    - [AWS Detective](12-security-and-config/detective.md)
    - [Security Hub](12-security-and-config/security-hub.md)
    - [AWS Audit Manager](12-security-and-config/audit-manager.md)
13. Disaster Recovery and Business Continuity in AWS
    - [DR/BC Architecture](13-disaster-recovery/dr.md)
    - [AWS Elastic Disaster Recovery (DRS)](13-disaster-recovery/drs.md)
14. Infrastructure as Code
    - [CloudFormation](14-iac/cloudformation.md)
15. Deployment and Management
    - [Service Catalog](15-deployment/service-catalog.md)
    - [CI/CD](15-deployment/cicd.md)
    - [Elastic Beanstalk - EB](15-deployment/eb.md)
    - [AWS OpsWorks](15-deployment/opsworks.md)
    - [AWS Systems Manager (SSM)](15-deployment/ssm.md)
    - [AWS Proton](15-deployment/proton.md)
16. Everything Else
    - [Amazon Lex and Amazon Connect](16-other/lex.md)
    - [Amazon Comprehend](16-other/comprehend.md)
    - [Amazon Kendra](16-other/kendra.md)
    - [Amazon Polly](16-other/polly.md)
    - [AWS Rekognition](16-other/rekognition.md)
    - [Kinesis Video Streams](16-other/kinesis-video-streams.md)
    - [AWS Glue](16-other/glue.md)
    - [AWS Device Farm](16-other/device-farm.md)
    - [Amazon Textract](16-other/textract.md)
    - [Amazon Transcribe](16-other/transcribe.md)
    - [Amazon Forecast](16-other/forecast.md)
    - [Amazon Fraud Detector](16-other/fraud-detector.md)

## Exam Description

AWS Certified Solutions Architect - Professional is intended for individuals with two or more years of hands-on experience designing and deploying cloud architecture on AWS. Before you take this exam, we recommend you have:
- Familiarity with AWS CLI, AWS APIs, AWS CloudFormation templates, the AWS Billing Console, the AWS Management Console, a scripting language, and Windows and Linux environments
- Ability to provide best practice guidance on the architectural design across multiple applications and projects of the enterprise as well as an ability to map business objectives to application/architecture requirements
- Ability to evaluate cloud application requirements and make architectural recommendations for implementation, deployment, and provisioning applications on AWS
- Ability to design a hybrid architecture using key AWS technologies (e.g., VPN, AWS Direct Connect) as well as a continuous integration and deployment process

## Preparation

- Official Exam Guide: [https://d1.awsstatic.com/training-and-certification/docs-sa-pro/AWS-Certified-Solutions-Architect-Professional_Exam-Guide.pdf](https://d1.awsstatic.com/training-and-certification/docs-sa-pro/AWS-Certified-Solutions-Architect-Professional_Exam-Guide.pdf)

- The exam may contain two types of questions:
    - Multiple choice
    - Multiple response

- Minimum passing score: 750/1000
- Number of questions: 75
- Time: 190 minutes to complete the exam (with a possibility to request 30 minutes extra for non-native english speakers)

## Credits

- These notes are based on the [AWS Certified Solutions Architect - Professional course](https://learn.cantrill.io/p/aws-certified-solutions-architect-professional) by Adrian Cantrill and on [Ultimate AWS Certified Solutions Architect Professional 2021](https://www.udemy.com/course/aws-solutions-architect-professional/) by Stephane Maarek
- All images present in the notes are taken from Adrian Cantrill's GitHub [repository](https://github.com/Ernyoke/aws-sa-pro) for his course
