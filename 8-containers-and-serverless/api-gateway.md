# API Gateway

- Is a service which lets us create and manage APIs
- API Gateway acts as endpoint or entry-point applications which want to talk with our services
- Sits between the application and integrations (services)
- API Gateway is HA and scalable
- Handles authorization, throttling, caching, CORS, transformations
- It also supports the OpenAPI spec and direct integration with other AWS services
- API gateway is a public service
- It can provide APIs using REST and WebSocket
- API Gateway overview:
    ![API Gateway Architecture](images/APIGateway.png)

## API Gateway - Authentication

- API Gateway supports a range of authentication types, example Cognito, Lambda based authentication (Custom based authentication - Bearer token), IAM credentials, etc.

## API Gateway - Endpoint Types

- Edge-Optimized: any incoming request is routed to the nearest CloudFront POP (point of presence)
- Region: region endpoint for clients in the same region
- Private: endpoints only accessible in a VPC via interface endpoints

## API Gateway - Stages

- When we deploy an API configuration, we are doing it into a stage
- Example we can have prod/dev stage with uniq settings and urls
- Deployments can be rolled back on a stage
- On stages we can enable canary deployments. When enabled, the deployment will be made to the canary not the stage itself
- Traffic distribution can be altered between canary and base stage
- Canary stage can be promoted to base

## API Gateway - Errors

- `4XX` - Client errors: invalid request on the client side
- `5XX` - Server errors: valid request, backend issue
- `400 - Bad Request`: generic client side error
- `403 - Access Denied`: authorizer denies request, request is WAF filtered
- `429` - API Gateway can throttle: this means we have exceeded a specified amount of requests
- `502 - Bad Gateway Exception`: bad output returned by Lambda
- `503 - Service Unavailable`: backing endpoint is offline
- `504` - Integration Failure/Timeout (29s limit)

## API Gateway - Caching

- Caching is configured per stage
- We can define a cache on a stage (500 MB up to 237 GB)
- Cache TTL default value is 300 seconds, configurable between 0 and 3600s. Can be encrypted
- Calls only will reach the backend in case of a cache miss