 # CloudTrail

- Is a product which logs API actions which affects AWS accounts (example: stop EC2 instance, delete S3 bucket)
- It logs API calls/activities as a CloudTrail Event, actions taken by user, role or service
- CloudTrail by default logs the last 90 days in Event History. It is enabled by default at no cost
- Trails: used to customize CloudTrail history
- Trails can be of 2 types:
    - **Management Events**: provide information about management operations performed on resources in AWS accounts (control plane operation). Example: create an EC2 instance, terminate an EC2 instance
    - **Data Events**: contain information about resource operations performed on or in a resource, example: objects uploaded to S3, Lambda function being invoked
- By default AWS logs only management events
- CloudTrail is a regional service, but when we create a Trail, it can operate in one region or in all regions
- All region trail: collection of log events in all regions
- Most services log events in the region where the event occurred
- A small number of services (IAM, STS, CloudFront) log events globally (us-east-1). For a trail to accept global events, it has to be all region trail (has to be enabled for the trail)
- The default event history is limited to 90 days, with a trail we can be much more flexible
- A trail can store the events in a defined S3 buckets indefinitely, and it also can be parsed by other tooling
- CloudTrail can be integrated with CloudWatch Logs, where we can use Metric Filters for example
- Organizational Trail: a trail created in the master account storing all events across every account in the organization
- CloudTrail does not offer real time logging!