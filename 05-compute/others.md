# Other Compute Related Products

## AWS Outposts

- AWS Outposts is a fully managed service that extends AWS infrastructure, services, APIs, and tools to customer premises
- An Outpost is a pool of AWS compute and storage capacity deployed at a customer site
- AWS operates, monitors, and manages this capacity as part of an AWS Region
- We can create subnets on our Outpost and specify them when we create AWS resources such as EC2 instances, EBS volumes, ECS clusters, and RDS instances
- Instances in Outpost subnets communicate with other instances in the AWS Region using private IP addresses, all within the same VPC
- Not every AWS service is supported within an Outpost, for a list see: https://docs.aws.amazon.com/outposts/latest/userguide/what-is-outposts.html#services

## AWS Wavelength

- AWS Wavelength is just like having an Availability Zone in a phone carrier's 'edge' network
- Wavelength deploys standard AWS compute and storage services to the edge of telecommunication carriers' 5G networks
- We can extend a virtual private cloud (VPC) to one or more Wavelength Zones
- We can then use AWS resources such as Amazon Elastic Compute Cloud (Amazon EC2) instances to run the applications that require low latency or edge resiliency within the Wavelength Zone
- AWS resources on Wavelength:
    - EC2 Auto Scaling
    - EKS clusters
    - ECS clusters
    - EC2 System Manager
    - CloudWatch, CloudTrail
    - CloudFormation
    - Application Load Balancers
- The services in Wavelength are part of a VPC that is connected over a reliable connection to an AWS region