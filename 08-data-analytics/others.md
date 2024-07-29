# Other Data Analytics Related Products

## AWS Data Exchange

- AWS Data Exchange is a service that helps AWS easily share and manage data entitlements from other organizations at scale
- As a data receiver, we can track and manage all of our data grants and AWS Marketplace data subscriptions in one place
- For data senders, AWS Data Exchange eliminates the need to build and maintain any data delivery and entitlement infrastructure

## AWS Data Pipeline

- AWS Data Pipeline is a web service that we can use to automate the movement and transformation of data
- We you can define data-driven workflows, so that tasks can be dependent on the successful completion of previous tasks
- Components of a data pipeline:
    - Pipeline definition:  specifies the business logic of our data management
    - Pipeline: schedules and runs tasks by creating Amazon EC2 instances to perform the defined work activities
    - Task Runners: polls for tasks and then performs those tasks. For example, Task Runner could copy log files to Amazon S3 and launch Amazon EMR clusters. Task Runner is installed and runs automatically on resources created by your pipeline definitions
- Use case examples:
    - We can use AWS Data Pipeline to archive your web server's logs to Amazon Simple Storage Service (Amazon S3) each day and then run a weekly Amazon EMR (Amazon EMR) cluster over those logs to generate traffic reports

## AWS Lake Formation

- AWS Lake Formation is a service that makes it easy to set up a secure data lake in days
- Creating a data lake with Lake Formation is as simple as defining data sources and what data access and security policies we want to apply
- Helps us collect and catalog data from databases and object storage, move the data into new Amazon S3 data lake, clean and classify data using machine learning algorithms, and secure access to sensitive data