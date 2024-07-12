# AWS Lambda

- Lambda is a Function-as-a-Service (FaaS) product. We provide specialized short running focused code for Lambda and it will take care running it and billing us for only what we consume
- Every Lambda function uses a supported runtime, example: Python 3.8, Java 8, NodeJS
- Every Lambda function is loaded into an executed in a runtime environment
- When we create a function we define the resources the function will use. We define the memory directly and CPU usage allocation indirectly (based on the amount of memory)
- We are only billed for the duration the function is running based on the number of invocations and the resources specified
- Lambda is a key part of serverless architectures in AWS
- Lambda has support for the following runtimes:
    - Python
    - Ruby
    - Go
    - Java
    - C#
    - Custom using Lambda layers (such as Rust)
- Lambda deployment package size:
    - 50 MB zipped
    - 250 MB unzipped
    - Up to 10 GB as a Docker image
- Lambda functions are stateless, meaning there is no data left over after an invocation
- When creating a Lambda function we define the memory. The memory can be between 128 MB and 10240 MB in 1 MB steps
- We do not directly define the vCPU allocated to each function, it will automatically scale with the memory: 1769 MB of memory gives 1 vCPU
- The runtime env. has a 512 MB (by default) storage available as `/tmp`. We can scale this storage up to 10240 MB. We can use this storage for whatever we need as long as we assume that it is blank at each execution of the function
- Lambda function can run up to 15 minutes, after this a timeout will occur
- The security for a Lambda function is controlled by the execution role. This is an IAM role attached to the function. This can have permissions for integration with other AWS services

## Lambda Networking

- Lambda functions can have 2 types of networking modes:
    - Public (default):
        - Lambda can access public AWS services such as SQS, DynamoDB, etc. and also internet based services
        - Lambda has network connectivity to public services running on the internet
        - Offers the best performance for Lambda, no customer specific networking is required
        - With public networking mode Lambda function wont be able to access resources in a VPC unless the resources do have public IPs and security controls allow external access
    - VPC Networking:
        - Lambda functions will run inside a VPC, so they will access everything in a VPC, assuming NACLs and SGs allow access
        - They wont be able to access services outside of the VPC, unless networking configuration exists in the VPC to allow external access
        - The Lambda needs `EC2Networking` permissions in order ot be able to create ENIs in the VPC
        - VPC based Lambda functions do not directly in the VPC, they will use a shared ENI to access resources in the VPC as long as all the functions have the same Security Group. In case new Security Groups are attached to a certain Lambda, new ENIs are placed inside the VPC
        - At the creation of the function, a certain ENI might be created for accessing the VPC. The initial setup would take up to 90 seconds. This setup will take place only once, not at every invocation

## Lambda Security

- There are 2 key parts of the security model
    - Lambda Functions will assume an execution role in order to access other AWS resources
    - Resource policies: similar to resource policies for S3. Allows external accounts to invoke a Lambda functions, or certain services to use Lambda functions. Resources polices can be modified using the CLI/API (currently cannot be changed with the console)

## Lambda Logging

- Lambda uses CloudWatch Logs and X-Ray
- Logs from Lambda executions are stored in CloudWatch Logs
- Details about Lambda metrics are stored in CloudWatch Metrics
- Lambda can be integrated with X-Ray for distributed tracing
- For Lambda to be able to log we need to give permissions via the execution role

## Lambda Invocations

- There are 3 ways Lambda functions can be invoked:
    - **Synchronous invocation**:
        - Command line or API directly invoking the function
        - The CLI or API will wait until the function returns
        - API Gateway will also invoke Lambdas synchronously, use case for many serverless applications
        - Any errors or retries have to be handled on the client side
    - **Asynchronous invocation**:
        - Used typically when AWS services invoke the function (example: S3 events)
        - The service will not wait for the response (fire and forget)
        - Lambda is responsible for any failure. Reprocessing will happen between 0 and 2 times
        - The function should be idempotent in order to be rerun
        - Lambda can be configured to send events to a DLQ in case of the processing did not succeed after the number of retries
        - Destination: events processed by Lambdas can be delivered to destinations like SQS, SNS, other Lambda, EventBride. Success and failure events can be sent to different destinations
    - **Event Source mapping**:
        - Typically used on streams or queues which don't generate events (Kinesis, DynamoDB streams, SQS)
        - Event Source mappers polls these streams and retrieves batches. These batches can be broken in pieces and sent to multiple Lambda invocations for processing
        - We can not have a partially successful batch, either everything works or nothing works
- In case of event processing in async invocation, in order to process the event we don't explicitly need rights to read from the sender
- In case of event source mapping the event source mapper is reading from the source. The event source mapping uses permissions from the Lambda execution role to access the source service
- Even if the function does not read data directly from the stream, the execution role needs read rights in order to handle the event batch
- Any batch that consistently fails to be processed, it can be sent to an SQS queue or SNS topic for further processing

## Lambda Versions

- We can define different versions for given functions
- A version of a function is the code + configuration of the function
- When we publish a version, it becomes immutable, it no longer can be changed. It event gets its own ARN (Amazon Resource Name)
- `$Latest` points to the latest version of Lambda version (it is not immutable)
- We can also define aliases (DEV, STAGE, PROD) which point to a version of the function. Aliases can be changed to point to other versions

## Lambda Start-up Times

- Lambda code runs inside of a runtime environment (execution context)
- At first invocation this execution context needs to be created and this will take time
- This process is known as cold start and it can take 100ms or more
- If the function is invoked again without too much of a gap, it might use the same execution context. This is called warm start
- One function invocation runs in an execution environment at a time. If multiple parallel instances are needed, the contexts will require cold starts
- **Provisioned concurrency**: we can provision one or more execution contexts in advance for Lambda invocations
- The improve performance we can use the `/tmp` folder to pre-download data to it. If another invocations uses the same execution context, it will be able to access the previously downloaded data
- We can create database connections outside of the Lambda handler. These will also be available for other invocations afterwards

## Lambda Function Handler

- Lambda function executions have life cycles
- The function code runs inside an execution environment
- Lifecycle phases:
    - `INIT`: creates or unfreezes the execution environment
        - It has the following sub-components:
            - `EXTENSION INIT`
            - `RUNTIME INIT`
            - `FUNCTION INIT`
        - Init phase runs only at cold-starts
    - `INVOKE`: runs the function handler (cold start)
    - `NEXT INVOKE`(s): warm start using the same environment
    - `SHUTDOWN`: execution environment is terminated after a period of inactivity
        - It has the following sub-components:
            - `RUNTIME SHUTDOWN`
            - `EXTENSION SHUTDOWN`
        - We can use Provisioned Concurrency to avoid having a cold-start in case the Lambda should be terminated

## Lambda Versions and Aliases

- Unpublished functions can be changed and deployed
- `$LATEST` version of the Lambda code can be edited and deployed
- We can take the current state of the function and publish it which will create an immutable version
- If the function is published, the code, dependencies, runtime settings and env. variables in the version created can not be edited
- Each version gets an uniq ARN (Qualified ARN)
- Unqualified ARN points at the function without a specific version (`$LATEST`)
- An alias is a pointer to a function version
- Example: PROD => function:1, BETA => function:2
- Each alias has an unique ARN
- Aliases can be updated, changing which version they reference
- Useful for PROD/DEV, BLUE/GREEN deployments, A/B testing
- We can also use alias routing: sending a certain percentage of request to v1 and other percentage to v2. Both versions need the same role, same DLQ (or no DLQ) will be used and both versions need to be published

## Lambda Environment Variables

- Key and value pairs for associated with Lambda functions
- By default they are associated with `$LATEST` - can be edited
- If they are publishes, they can not be edited
- They can be accessed within the execution environment
- The environment variables can be encrypted with KMS
- They allow code execution to be adjusted based on variables

## Lambda Layers

- Used to split off libraries and dependencies from Lambda functions
- Reduces the size of the deployment package
- Layers can be reused by multiple Lambda functions
- Libraries in layers are extracted in the `/opt` folder
- Layers allow new runtimes which are not explicitly supported by AWS

## Lambda Container Images

- Until recently Lambda was considered to be a Function as a Service (FaaS) product, which means creation of a function, uploading code and executing it
- Many organizations use containers and CI/CD processes built for containers
- Lambda is now capable to use containers images
- It is an alternative way of packaging the function code and using it with the Lambda product
- Lambda Runtime API - has to be included with the container images, it is a package which allows interaction between a container and the Lambda
- AWS Lambda Runtime Interface Emulator (RIE): used for local Lambda testing

## Lambda and ALB

- Lambda functions can be registered to ALB target groups
- Communication between the user and the ALB is via HTTP/HTTPS, there is no difference than connecting to a classic server (EC2) from the user's perspective
- When the ALB receives a request from the client it synchronously invokes the Lambda function
- The LB passes in a JSON structure to the Lambda function, inside the `Event` structure. This has to be interpreted by the Lambda. What actually happens is that the LB translates the HTTP(S) request to a Lambda compatible event, to which the Lambda answers with a JSON object that gets translated back to HTTP/HTTPS response
- Multi-Value headers:
    - For an example lets us this URL for the Lambda: http://catagram.io?&search=roffle&search=winkie
    - Without multi-value headers the Lambda receives the following:
        ```
        "queryStringParameters": {
            "search": "winkie"
        }
        ```
    - If the multi-value headers are enabled, we get this delivered to Lambda:
        ```
        "multiValueQueryStringParameters": {
            "search": ["roffle", "winkie"]
        }
        ```