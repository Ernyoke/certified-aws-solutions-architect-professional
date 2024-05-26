# AWS Glue

- Is a serverless ETL (Extract, Transform, Load) product
- Similar to AWS Datapipeline (which can do ETL) but it uses servers (EMR clusters)
- Glue is used to move data and transform data between source and destination
- Glue also crawls data sources and generates the AWS Glue Data catalog
- Supports a range of data collection as data sources: S3, RDS, JDBC compatible data sources and DynamoDB
- Supports data streams as data sources such as Kinesis Data Streams and Apache Kafka
- Data targets supported: S3, RDS, JDBC data sources

## Data Catalog

- Collection of persistent metadata about data sources in a region
- Provides one uniq data catalog in every region per account
- It helps avoid data silos: makes data not visible in an organization be visible a able to be browsed
- Data Catalog can be used by Amazon Athena, Redshift Spectrum, EMR and AWS Lake Formation
- Data is discovered by configuring crawlers and givin it credentials to access data sources

## Glue Job

- Extract, Transform an Load jobs
- Jobs can do transformation by using scripts created by us
- Jobs are serverless, AWS maintains a pool of resources
- We are only billed by resources we consume