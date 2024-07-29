# Policies

- IAM policies define permissions for an action regardless of the method that you use to perform the operation

## Policy types

- **Identity-based policies**: attach managed and inline policies to IAM identities (users, groups to which users belong, or roles). Identity-based policies grant permissions to an identity
- **Resource-based policies**: attach inline policies to resources. The most common examples of resource-based policies are Amazon S3 bucket policies and IAM role trust policies. Resource-based policies grant permissions to a principal entity that is specified in the policy. Principals can be in the same account as the resource or in other accounts
- **Permissions boundaries**: use a managed policy as the permissions boundary for an IAM entity (user or role). That policy defines the maximum permissions that the identity-based policies can grant to an entity, but does not grant permissions. Permissions boundaries do not define the maximum permissions that a resource-based policy can grant to an entity
- **Organizations SCPs**: use an AWS Organizations service control policy (SCP) to define the maximum permissions for account members of an organization or organizational unit (OU). SCPs limit permissions that identity-based policies or resource-based policies grant to entities (users or roles) within the account, but do not grant permissions
- **Access control lists (ACLs)**: use ACLs to control which principals in other accounts can access the resource to which the ACL is attached. ACLs are similar to resource-based policies, although they are the only policy type that does not use the JSON policy document structure. ACLs are cross-account permissions policies that grant permissions to the specified principal entity. ACLs cannot grant permissions to entities within the same account
- **Session policies**: pass advanced session policies when you use the AWS CLI or AWS API to assume a role or a federated user. Session policies limit the permissions that the role or user's identity-based policies grant to the session. Session policies limit permissions for a created session, but do not grant permissions. For more information, see Session Policies

## Policies Deep Dive

- Anatomy of a policy: JSON document with `Effect`, `Action`, `NotAction` (inverse condition of `Action`), `Resource`, `Conditions` and `Policy Variables`
- Priority order of permissions in AWS is: deny (explicit) > allow > deny (implicit). A policy always assumes a default (implicit) deny => if we do not allow explicitly to do something, we wont be able to do it
- An explicit `DENY` has always precedence over `ALLOW`
- Best practice: use least privilege for maximum security
    - Access Advisor: a tool for seeing permissions granted and when last accessed
    - Access Analyzer: used of analyze resources shared with external entities
- Common Managed Policies:
    - `AdministratorAccess`
    - `PowerUserAccess`: does not allow anything regarding to IAM, organizations and account (with some exceptions), otherwise similar to admin access
- IAM policy condition:

    ```
    "Condition": {
        "{condition-operator}": {
            "{condition-key}": "{condition-value}"
        }
    }
    ```

- Operators:
    - String: `StringEquals`, `StringNotEquals`, `StringLike`, etc.
    - Numeric: `NumericEquals`, `NumericNotEquals`, `NumericLessThan`, etc.
    - Date: `DateEquals`, `DateNotEquals`, `DateLessThan`, etc.
    - Boolean
    - IpAddress/NotIpAddress:
        - `"Condition": {"IpAddress": {"aws:SourceIp": "192.168.0.1/16"}}`
    - ArnEquals/ArnLike
    - Null
        - `"Condition": {"Null": {"aws:TokenIssueTime": "192.168.0.1/16"}}`
- Policy Variables and Tags:
    - `${aws:username}`: example `"Resource:["arn:aws:s3:::mybucket/${aws:username}/*"]`
    - AWS Specific:
        - `aws:CurrentTime`
        - `aws:TokenIssueTime`
        - `aws:PrincipalType`: indicates if the principal is an account, user, federated or assumed role
        - `aws:SecureTransport`
        - `aws:SourceIp`
        - `aws:UserId`
    - Service Specific:
        - `ec2:SourceInstanceARN`
        - `s3:prefix`
        - `s3:max-keys`
        - `sns:EndPoint`
        - `sns:Protocol`
    - Tag Based:
        - `iam:ResourceTag/key-name`
        - `iam:PrincipalTag/key-name`

## Permission Boundaries

- Only IDENTITY permissions are impacted by boundaries - any resource policies are applied full
- Permission boundaries can be applied to IAM Users and IAM Roles
- **Permission boundaries don't grant access to any action. They define maximum permissions an identity can receive**
- Use cases for permission boundaries:
    - Delegation problem: if we give elevated permissions to an user, he/she could promote itself to have administrator permissions or could create another user with administrator permissions
    - Solution is to have a boundary which forbids changing its onw user's permissions and forbid creating other users/roles with elevated permissions

## Policy Evaluation Logic

- Components involved in a policy evaluations:
    - Organization SCPs
    - Resource Policies
    - IAM Identity Boundaries
    - Session Policies
    - Identity Policies
- Policy evaluation logic - same account:
    ![policy evaluation logic - same account](images/PolicyEvaluation1.png)
- Policy evaluation logic - different account:
    ![policy evaluation logic - different account](images/PolicyEvaluation2.png)

## AWS Policy Simulator

- When creating new custom policies you can test it here:
  - https://policysim.aws.amazon.com/home/index.jsp
  - This policy tool can you save you time in case your custom policy statement's permission is denied
- Alternatively, you can use the CLI:
    - Some AWS CLI commands (not all) contain `--dry-run` option to simulate API calls. This can be used to test permissions.
    - If the command is successful, you'll get the message: `Request would have succeeded, but DryRun flag is set`
    - Otherwise, you'll be getting the message: `An error occurred (UnauthorizedOperation) when calling the {policy_name} operation`
