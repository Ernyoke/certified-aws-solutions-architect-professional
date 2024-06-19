# Amazon Macie

- It is a data security and data privacy service
- Macie is a service to discover, monitor and protect data stored in S3 buckets
- Once enabled and pointed to buckets, Macie will automatically discover data and categorize it as PII, PHI, Finance etc.
- Macie is using data identifier. There are 2 types of data identifier:
    - Managed Data Identifier: built-in, can use machine learning, pattern matching to analyze and discover data. It is designed to detect sensitive data from many countries
    - Custom Data Identifier: created by clients, they are proprietary to accounts and they are regex based
- Discovery Jobs: these jobs will use data identifiers to manage and search for sensitive content. They will generate findings which can be used for integration with other AWS service (ex: Event Bridge) in order to do automatic remediation
- Macie uses multi account architecture: one account is the master account which can manage other accounts to discover sensitive data

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