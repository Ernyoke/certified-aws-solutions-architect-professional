# AWS Systems Manager Parameter Store

- Used to store system configurations (documents, configurations, secrets, strings) in a resilient, secure and scalable way
- Stores data in a key-value format
- Many AWS services have native integration with Parameter Store
- Parameter store offers the availability to store 3 different type of values: Strings, StringLists and SecureStrings
- We can store license codes, database strings, full configs and passwords in Parameter Store
- Values can be stored in a hierarchical way. Different versions of the values are also stored
- Parameter Store can store plaintext and ciphertext which can be decrypted using the KMS integration
- Public Parameters: parameters maintained and provided by AWS, example: latest AMIs per region
- Parameter Store is public service
- Parameter Store is tightly integrated with IAM