# AWS Config

- Has 2 main jobs:
    - Primary: record configuration changes over time on AWS resources. Every time a configuration is changed on a resource a configuration item is created which stores the change at that specific point in time
    - Secondary: auditing of changes, compliance with standards
- Config does not prevent changes from happening! Even if we define standards for resources, Config can check the compliance against those standards, but it does not prevent resources braking those standards
- Config is a regional service, supports cross-region and cross-account aggregations
- Changes can generate SNS notifications and near-realtime events via EventBridge and Lambda
- Config stores changes historically in a consistent format in an S3 product bucket
- Config recording has to be manually enabled!
- Config Rules:
    - Can be AWS managed ones or user defined using Lambda
    - Resources are evaluated against these rules determining if there are compliant or non-compliant
    - Custom Rules use Lambda, the function does the evaluation and returns the information back to Config
- Config can be integration to EventBridge which can be used to invoke Lambda functions for automatic remediation
- Config can also have integration with SSM to remediate issues