# AWS Lambda

- Lambda is a Function-as-a-Service (FaaS) product. We provide specialized short running a focused code for Lambda and it will take care running it and billing us for only what we consume
- Every Lambda function uses a supported runtime, example: Python 3.8
- Every Lambda function is loaded into an executed ina runtime environment
- When we create a function we define the resources the function will use. We define the memory directly and CPU usage allocation indirectly (based on the amount of memory)
- We are only billed for the duration the function is running based on the number of invocations and the resources specified
- Lambda is a key part of serverless architectures in AWS
- Lambda has support for the following runtimes:
    - Python
    - Ruby
    - Go
    - Java
    - C#
    - Custom using Lambda layers
- Lambda functions are stateless, meaning there is no data left over after an invocation
- When creating a Lambda function we define the memory. The memory can be between 128MB and 3GB in 64MB steps(historically, nowadays there is support for up to 10GB)
- We do not directly define the vCPU allocated to each function, it will automatically scale with the memory: 1792MB of memory gives 1 vCPU
- The runtime env. has a 512MB storage available as `/tmp`
- Lambda function can run up to 15 minutes, after this a timeout will occur

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
        - At the creation of the function, a certain ENI might be created for accessing the VPC. The initial setup would take up to 90 seconds

## Lambda Security

- There are 2 key parts of the security model
    - Lambda Functions will assume an execution role in order to access other AWS resources
    - Resources policies: similar to resource policies for S3. Allows external accounts to invoke a Lambda functions, or certain services to use Lambda functions. Resources polices can be modified using the CLI (not with the console)

## Lambda Logging

- Lambda uses CloudWatch Logs and X-Ray
- Logs from Lambda executions are stored in CloudWatch Logs
- Details about Lambda metrics are stored in CloudWatch Metrics
- Lambda can be integrated with X-Ray for distributed tracing
- For Lambda to be able to log we need to give an execution log to it