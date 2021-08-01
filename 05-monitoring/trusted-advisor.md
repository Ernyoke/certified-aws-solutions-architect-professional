# AWS Trusted Advisor

- Provides real time guidance to provision resources against AWS best practices
- It is an account level product, requires no agent to be installed
- Provides a number of checks and recommendations in 5 major areas:
    - Cost Optimization & Recommendations
    - Performance
    - Security
    - Fault Tolerance
    - Service Limit
- Trusted Advisor is not a free service, at least if we want to get out the most of it
- The free version is available if the account has basic or developer support plans
- The free version provides 7 basic core checks:
    - S3 bucket permissions (open access permissions)
    - Security Groups - specific ports unrestricted
    - IAM use
    - MFA on Root Account
    - EBS Public Snapshots
    - RDS Public Snapshots
    - 50 service limit checks
- Anything beyond these basic checks requires business or enterprise support plans
- With business and enterprise support we get further 115 checks
- We also get access to the AWS Support API
- AWS Support API allows for programmatic access for AWS support functions:
    - We can get the names and identifiers for the checks AWS offers
    - We can request a Trusted Adviser check runs against accounts and resources
    - Allows to get summaries and detailed information programmatically
    - Allows request for Truster Advisor refresh
- AWS Support API allows to programmatically open support ticket, and manage them
- With business and enterprise support we get CloudWatch Integration

## AWS Support Plans

- Basic Support:
    - Is included for AWS customers and it is free
    - For Trusted Advisor with this support plan we get 7 core checks (see them above)
- Developer:
    - For Trusted Advisor we get the same 7 base core checks (see them above)
- Business:
    - We ge the full set of checks and recommendation
    - We get programmatic access to Trusted Advisor
- Enterprise:
    - Same as business

## Good to Know

- We can check if an S3 bucket is made public, but we cannot check if objects are public in a bucket. Monitoring this we might use CloudWatch Events/S3 Events instead
- Service Limits:
    - Limits can only be monitored in Trusted Advisor
    - Cases have to be created manually in AWS Support Centre to increase limits