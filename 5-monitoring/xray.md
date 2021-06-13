# AWS X-Ray

- It is a distributed tracing application. It can track sessions through an application
- X-Ray takes data from many services (API Gateway, Lambda, DynamoDB) as part of an application and gives on single overview of the session flow
- **Tracing Header**: when an user connects to an application with X-Ray enabled, a **tracing ID** is generated an embedded into a tracing header. This header is used to track the request across all supported services
- **Segments**: supported services send data to X-Ray using segments. Segments are data blocks containing host/ip, request, response, work done (times), issues information
- **Subsegments**: segments can contain subsegments for more granularity. This can contain details to other services as part of the application component
- **Service Graph**: JSON document detailing services and resources which make up the application
- **Service Map**: visual representation of a service graph by the X-Ray console
- In order to provide X-Ray data to the AWS X-Ray service we can do the following:
    - EC2: install X-Ray Agent
    - ECS: install agent in a task
    - Lambda: enable X-Ray
    - Beanstalk: agent is preinstalled
    - API Gateway: per stage option
    - SNS and SQS: can be enabled
- Services require IAM permission in order ot send data to X-Ray service