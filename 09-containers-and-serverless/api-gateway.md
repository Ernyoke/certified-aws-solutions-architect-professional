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

## API Gateway Methods and Resources

- API Gateway URL example: `https://1nj7i16t37.execute-api.us-east-1.amazonaws.com/dev/listcats`
- The URL can be represents the following: `[api-gateway-endpoint]/[stage]/[resource]`
- Stages are logical configurations. APIs are deployed into stages. Stages can be used for different application versions or lifecycle points for an API
- API changes only take effect after it is deployed into a stage
- Methods are the desired action to be performed. Methods are HTTP verbs
- Methods are where integrations are configured which provide the functionality of an API. Methods can integrate with Lambda, HTTP and other AWS services

## API Gateway Integrations

- API Gateway is capable of connecting to Lambda, HTTP Endpoints (running on-premises or on AWS), Step Functions, SNS, DynamoDB
- APIs have 3 phases:
    - Request: authorize, validate and transform the request
    - Integrations
    - Response: transform, prepare and return the response
- The request and response phases are split into 2 parts:
    - Method Request: defines everything about the client request to method
    - Integration Request: parameters from the method request are transferred to the integrations
    - Integration Response: converts the data from the backend to a form which can be sent back to the client
    - Method Response: how the communication is delivered back to the client
- API methods which are on the client side decide what the client request to method is like. There are integrated to a backend endpoint via integrations
- Integration types:
    - **Mock**: used for testing, no backed involved. No real backend needed
    - **HTTP**: http custom integration. We have to configure both integration request and integration response
    - **HTTP Proxy**: subtype of HTTP where. Allows the access HTTP endpoint with a streamline integration. Proxying is where the request is passed to the endpoint as is and sent back to the client as is
    - **AWS**: allows an API to expose AWS services. We have to configure both the integration request and response
    - **AWS_PROXY (LAMBDA)**: integration request/response does not have to be defined, API Gateway passes the request unmodified
- Mapping template: used for non-proxy integrations. Used for:
    - Modify or rename parameters
    - Modify the body or header of the request
    - Filtering - remove anything from the request
- Mapping uses VTL (Velocity Template Langue) for editing the request

## API Gateway Stages and Deployments

- Editing an API, we are editing settings which are not live (not published)
- The current state of the API needs to be deployed to a stage
- Each stage has its own configuration. Configurations are not immutable, can be modified, overwritten or rolled back
- Stage variables: environment variables for stages

## Swagger and OpenAPI

- OpenAPI (OAS) defines a standard language-agnostic interface to RESTful APIs
- OpenAPI v2 is formerly known as Swagger
- OpenAPI v3 is a more recent version
- OpenAPI defines endpoints, operation (GET, POST, etc.), input and output parameters and authentication methods
- API Gateway is capable of import OpenAPI format and generating it. Useful for backups and migrations