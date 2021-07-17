# DMS - Database Migration Service

- Database migrations are complex actions to perform
- DMS it is a managed database migration service
- It starts with using a replication instance running on EC2
- This instance runs one or more replication task
- For these tasks we have to specify the source and destination endpoints at source and target databases. **One endpoint must be on AWS!**
- Databases supported are: MySQL, Aurora, Microsoft SQL, MariaDB, MongoDB, PostgreSQL, Oracle, Azure SQL, etc.
- Jobs can be one of 3 types:
    - Full load migrations: used to migrate existing data, simply migrates the data from source to target. Great if we can afford an outage for the source DB
    - Full load + CDC: migrates the existing data and replicates any ongoing changes
    - CDC only: designed to replicate only data changes. In some situations might be more efficient to use other tools for full migration and use CDC only for ongoing changes afterwards
- DMS does not support any form of schema conversions, for this we should use Schema Conversion Tool (SCT) provided by AWS

## SCT - Schema Conversion Tool

- SCT is a standalone app used for converting one database engine to another including conversion of schema from a DB to S3
- SCT is not used when migrating between DBs of the same type
- SCT works with OLTP DBS (MySQL, Oracle, Aurora, etc.) and OLAP databases (Teradata, Oracle, Vertica, Greenplum, etc.)

## DMS and Snowball

- Larger migrations might imply moving dbs with sizes of multi-TB
- Moving data over networks takes time and consumes capacity
- DMS is able to utilise Snowball products to migrate databases
- Migration steps:
    1. Use SCT to extract data locally and move the data to a Snowball
    2. Ship the device back to AWS. They will load the data into an S3 bucket
    3. DMS migrates from S3 into a target source
    4. Change Data Capture (CDC) can capture changes and via S3 intermediary they are also written to the target database