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