# AWS Elastic Disaster Recovery (DRS)

- Minimizes downtime and data loss with fast, reliable recovery of on-premises and cloud-based applications using affordable storage, minimal compute, and point-in-time recovery
- We can increase IT resilience when you use AWS Elastic Disaster Recovery to replicate on-premises or cloud-based applications running on supported operating systems
- We set up a DRS agent on our source servers to initiate secure data replication
- Our data is then replicated in a staging area subnet in our AWS account
- The staging area is designed to reduce costs by using affordable storage and minimal compute
- AWS Elastic Disaster Recovery automatically converts our servers to boot and run natively on AWS when we launch instances for drills or recovery
- If we need to recover applications, you can launch recovery instances on AWS within minutes