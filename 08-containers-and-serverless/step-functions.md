# AWS Step Functions

- Lambda is FaaS
- A function has an execution time of maximum 15 minutes
- Lambda functions can be chained together, but it is considered to be an anti-patterns and it can get messy. Lambda runtime environments are stateless
- Step Functions is service which lets us create state machines
- States can do things, can decide things, can take in data, modify data and output data
- Maximum duration for state machine execution is 1 year
- Step Functions can represent 2 different type of workflow:
    - Standard workflow: is the default and it has 1 year execution limit
    - Express workflow: designed for high volume event processing, which can run up to 5 minutes
- Step Functions can be initiated with API Gateway, IOT Rules, EventBridge, Lambda, etc.
- State machines for Step Functions can be created with Amazon State Language (ASL) JSON templates

## States

- Available states:
    - `SUCCEED` and `FAIL`
    - `WAIT`: wait for a certain period or time or until a date
    - `CHOICE`: allows a state machine to take a different path depending on an input
    - `PARALLEL`: allows to create parallel branches in a state machine
    - `MAP`: expects a list of things, for which it might perform a certain set of things
    - `TASK`: represents a single unit of work performed by a state machine. They can be integrated with: Lambda, Batch, DynamoDB, ECS, SNS, SQS, Glue, SageMaker, EMR, etc.


