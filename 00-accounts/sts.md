# STS

- Allows to assume roles across different accounts or same accounts
- Generates temporary credentials (`sts:AssumeRole*`)
- Temporary credentials are similar to access key. They expire and they don't belong to the identity
- Temporary credentials are requested by another identity (AWS or external - identity federation)
- STS allows us to enable identity federation

## Assume a Role with STS

1. Define an IAM role within an account or cross-account
2. Define which principals can access the IAM role
3. Use the AWS STS (Secure Token Service) to retrieve the IAM role we have access to (`AssumeRole` API)
4. Temporary credentials can be valid between 15 minutes to 1 hour

## Revoke IAM Role Temporary Credentials

- **Trust policy**: specifies who can assume a role
- Roles can be assumed by many identities
- Everybody who assumes a role, gets the same set of permissions
- Temporary credentials can not be cancelled, they are valid until they expire
- Temporary credentials can last for longer time
- In case of a credential leak if we change the permissions for the policy, we will affect all legitimate users - not a good idea for revoking access
- Solution: 
    - Revoke all existing sessions, by applying an `AWSRevokeOlderSessions` inline policy to the role. This will apply to all existing sessions, sessions created afterwards will not be affected
    - We can not manually revoke credentials!

