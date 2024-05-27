# IAM: Identity and Access Management

- When accessing AWS, the root account should **never** be used. Users must be created with the proper permissions. IAM is central to AWS
- **Users**: A physical person
- **Groups**: Functions (admin, devops) Teams (engineering, design) which contain a group of users
- **Roles**: Internal usage within AWS resources
    - **Cross Account Roles**: roles used to assumed by another AWS account in order to have access to some resources in our account
- **Policies (JSON documents)**: Defines what each of the above can and cannot do. **Note**: IAM has predefined managed policies
    - There are 3 types of policies:
        - AWS Managed
        - Customer Managed
        - Inline Policies
- **Resource Based Policies**: policies attached to AWS services such as S3, SQS

## IAM Roles vs Resource Based Policies

- When we assume a role (user, application or service), we give up our original permission and take the permission assigned to the role
- When using a resource based policy, principal does not have to give up any permissions
- Example: user in account A needs to scan a DynamoDB table in account A and dump it in an S3 bucket in account B. In this case if we assume a role in account B, we wont be able to scan the table in account A
  
## Best practices

- One IAM User per person **ONLY**
- One IAM Role per Application
- IAM credentials should **NEVER** be shared
- Never write IAM credentials in your code. **EVER**
- Never use the ROOT account except for initial setup
- It's best to give users the minimal amount of permissions to perform their job