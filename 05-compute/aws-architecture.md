# Regional and Global AWS Architecture

- There are 3 main type of architectures:
    - Small scale architectures: one region/one country
    - Small architecture with DR: one region + backup region for disaster recovery
    - Multiple region based systems
- Architectural components at global level:
    - Global Service Location and Discovery
    - Content Delivery (CDN) and optimization
    - Global health checks and Failover
- Regional components:
    - Regional entry point
    - Scaling and resilience
    - Application services and components
![Regional and Global Architecture](images/RegionalandGlobalInfrastructure2.png)


Web Tier : Customer facing layer. There would be regional based services like ALB or API gateway dependig on application architecture. Abstracts customers from underlying architecture.
Compute Tier: The infra to web tier is provided by compute tier using EC2, lambda or containers.
Storage Services: Service like EBS, EFS or S3.
Data Storage: Products like RDS, Aurora, DynamoDB, and RedShift.
Caching: Elastic cache for general caching, DynamoDB acclerator(DAX)
App Services: Kinesis, Step Functions, SQS, SNS

