# AWS SAM - Serverless Application Model

- A serverless application is not just a Lambda function, it can include many more services such as:
    - Front end code and assets hosted by S3 and CloudFront
    - API endpoint - API Gateway
    - Compute - Lambda
    - Database - DynamoDB
    - Event sources, permissions and more...
- The SAM group of products and features within AWS has 2 main parts:
    - AWS SAM template specification, which is an extension of CloudFormation. Adds components to CloudFormation templates designed specifically for serverless applications: Transforms, Globals & Resources (can include normal CFN resources as well as SAM specific ones)
    - AWS SAM CLI allows local testing, local invocation and deployments into AWS