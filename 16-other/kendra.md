# Amazon Kendra

- Is an intelligent search service
- It's primary aim is to be designed to mimic interacting with a human expert
- Supports wide range if question types such as:
    - Factoid: Who, What, Where
    - Descriptive: How do I get my cat to spot being a jek?
    - Keyword: What time is the keynote address (address can have multiple meaning) - Kendra helps determine intent

## Key Concepts

- **Index**: searchable data organized in an efficient way
- **Data Source**: where the data lives, Kendra connects and indexes from this location. Example of locations are: S3, Confluence, Google Workspaces, RDS, OneDrive, Salesforce, Kendra Web Crawler, Workdocs, FSX, etc.
- We configure Kendra to synchronize a data source with an index based on a **schedule**. This should keep the index current
- **Documents**: can be structured (FAQs) and unstructured (HTML, PDF, etc.)

## Others

- Kendra integrates with other AWS services: IAM, Identity Center (SSO) and others