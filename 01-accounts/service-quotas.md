# AWS Service Quotas

- Defines how much of a "thing" we can use inside of an AWS account
- Example:
    - Number of EC2 instances at a certain times per region
    - Number of IAM users per AWS accounts
- Services usually have a default per region quota
- Global services may have a per account quota instead per region
- Most services quotas can be increased as needed
- Some service quotes can not be changed, example: number of IAM users per account (5000)
- The higher the increase, the more time is needed to get the change-request approved
- Service endpoint and quotas: [https://docs.aws.amazon.com/general/latest/gr/aws-service-information.html](https://docs.aws.amazon.com/general/latest/gr/aws-service-information.html)
- **Service Quotas**: 
    - From the console we can go to *Service Quotas* page, where we can create dashboards for quotas we want to monitor
    - We can request quota changes from this service for certain services
    - *Quote request template*: we can predefine quota value request for new accounts in an AWS organization
    - We can create a CloudWatch Alarm based on a particular service quota
- Legacy method to increase quotas: create a support ticket selecting service quota increase
- We can request service quota increase from the CLI as well. Reference API: [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/service-quotas/request-service-quota-increase.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/service-quotas/request-service-quota-increase.html)
