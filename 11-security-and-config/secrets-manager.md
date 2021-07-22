# AWS Secrets Manager

- It does share functionality with SSM Parameter Store
- Secrets Manager is designed specifically for secrets, example passwords, API Keys
- It is usable via Console, CLI, API or SDK's
- It supports the automatic rotation of secrets using a Lambda functions
- For certain AWS services, Secrets Manager offers direct integration, such as RDS (automatic synchronization when the secrets are rotated)
- Secrets are encrypted using KMS