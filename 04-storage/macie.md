# Amazon Macie

- It is a data security and data privacy service
- Macie is a service to discover, monitor and protect data stored in S3 buckets
- Once enabled and pointed to buckets, Macie will automatically discover data and categorize it as PII, PHI, Finance etc.
- Macie is using data identifier. There are 2 types of data identifier:
    - Managed Data Identifier: built-in, can use machine learning, pattern matching to analyze and discover data. It is designed to detect sensitive data from many countries
    - Custom Data Identifier: created by clients, they are proprietary to accounts and they are regex based
- Discovery Jobs: these jobs will use data identifiers to manage and search for sensitive content. They will generate findings which can be used for integration with other AWS services (ex: Security Hub from where findings can be passed to Event Bridge) in order to do automatic remediation
- Macie uses multi account architecture: one account is the administrator account which can used to manage Macie within the member accounts to discover sensitive data
- This multi-account structure can be done with AWS Organizations or by explicitly inviting accounts in Macie

## Macie Identifiers

- Data Discovery Jobs: analyzes data in order to determine wether the objects contain sensitive data. This is done using data identifiers
- **Managed Data Identifiers**:
    - Created and managed by AWS
    - Can be used to detect a growing list of common sensitive data types: credentials, financial data, health data, personal identifiers (addresses, passports, etc.)
- **Custom Data Identifiers**:
    - Can be created by us, AWS account users/owners
    - They are using regex patterns to match data
    - We can add optional keywords: optional sequences that need to be in the proximity to regex match
    - Maximum Match Distance: how close keywords are to regex pattern
    - We can also include ignore words

## Macie Findings

- Macie will produce 2 types of findings:
    - **Policy Findings**: are generated when the policies or settings are changed in a way that reduces the security of the bucket after Macie is enabled
    - **Sensitive Data Findings**: generated when sensitive data is identified based on identifiers
- Types if policy findings:
    - `Policy:IAMUser/S3BlockPublicAccessDisabled`: all bucket-level block public access settings were disabled for the bucket
    - `Policy:IAMUser/S3BucketEncryptionDisabled`: default encryption settings for the bucket were reset to default Amazon S3 encryption behavior, which is to encrypt new objects automatically with an Amazon S3 managed key
    - `Policy:IAMUser/S3BucketPublic`: an ACL or bucket policy for the bucket was changed to allow access by anonymous users or all authenticated AWS Identity and Access Management (IAM) identities
    - `Policy:IAMUser/S3BucketSharedExternally`: an ACL or bucket policy for the bucket was changed to allow the bucket to be shared with an AWS account that's external to (not part of) your organization
- Types of sensitive data findings:
    - `SensitiveData:S3Object/Credentials`: object contains sensitive credentials data, such as AWS secret access keys or private keys
    - `SensitiveData:S3Object/CustomIdentifier`: object contains text that matches the detection criteria of one or more custom data identifiers
    - `SensitiveData:S3Object/Financial`: object contains sensitive financial information, such as bank account numbers or credit card numbers
    - `SensitiveData:S3Object/Multiple`: object contains more than one category of sensitive data
    - `SensitiveData:S3Object/Personal`: object contains sensitive personal information—personally identifiable information (PII) such as passport numbers or driver's license identification numbers, personal health information (PHI) such as health insurance or medical identification numbers, or a combination of PII and PHI