# AWS Compute Optimizer

- AWS Compute Optimizer is a service that analyzes our AWS resources' configuration and utilization metrics to provide us with rightsizing recommendations
- Generates optimization recommendations to reduce the cost and improve the performance of your workloads
- Supported resources and requirements:
    - EC2 instances
    - Auto Scaling Groups
    - EBS volumes
    - Lambda functions
    - ECS and ECS Fargate
    - Commercial Software Licenses
    - RDS DB instances and storage
- Compute Optimizer is opt-in
- Analyzing metrics: after opt in, Compute Optimizer begins analyzing the specifications and the utilization metrics of resources from Amazon CloudWatch for the last 14 days
- Enhancing recommendations: we can enhance recommendations by activating recommendation preferences, such as the enhanced infrastructure metrics paid feature