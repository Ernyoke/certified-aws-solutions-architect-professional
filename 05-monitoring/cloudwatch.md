# CloudWatch

- Provides services to ingest, store and manage metrics
- It is a public service - provides public space endpoints
- Many services have native management plan integration with CloudWatch, for example EC2. Also, EC2 provides external gathered information only, for metrics from inside an EC2 we can use CloudWatch agent
- CloudWatch can be used from on-premises using the agent or the CloudWatch API
- CloudWatch stores data in a persistent way
- Data can be viewed from the console, CLI or API, but also CloudWatch also provides dashboards and anomaly detection
- CloudWatch Alarms: react to metrics, can be used to notify or to perform actions

## CloudWatch - Data

- Namespace: container for metrics
- Data point: timestamp, value, unit of measure (optional)
- Metric: time ordered set of data point. Example of builtin metrics: `CPUUtilization`, `NetworkIn`, `DiskWriteBytes` for EC2
- Every metric has a `MetricName` and a namespace
- Dimension: name/value pair, example: a dimension is the way for `CPUUtilization` metric to be separated from one instance to another
- Dimensions can be used to aggregate data, example aggregate data for all instances for an ASG
- Resolution: standard (60 second granularity) or high (1 second granularity)
- Metric resolution: minimum period that we can get one particular data point for
- Data retention:
    - sub 60s granularity is retained for 3 hours
    - 60s granularity retained for 15 days
    - 5 min retained for 63 days
    - 1 hour retained for 455 days
- As data ages, its aggregated and stored for longer period of time with less resolution
- Statistics: get data over a period and aggregate it in a certain way
- Percentile: relative standing of a value within the dataset

## CloudWatch Alarms

- Alarm: watches a metric over a period of time
- States: `ALARM` or `OK` based on the value of a metric against a threshold over time
- Alarms can be configured with one or more actions, which can initiate actions on our behalf. Actions can be: send notification to an SNS topic, attempt an auto scaling policy modification or use Event Bridge to integrate with other services
- High resolution metrics can have high resolution alarms

## CloudWatch Logs

- CloudWatch Logs provides two type of functionalities: ingestion and subscription
- CloudWatch Logs is a public service designed to store, monitor and provide access logging data
- Can provide logging ingestion for AWS products natively, but also for on-premises, IOT or any application
- CloudWatch Agent: used to provide ingestion for custom applications
- CloudWatch can also ingest log streams from VPC Flow Logs or CloudTrail (account events and AWS API calls)
- CloudWatch Logs is regional service, certain global services send their logs to `us-east-1`
- Log events consist of 2 parts:
    - **Timestamp**
    - **Raw message**
- Log Events can be collected into Log Streams. Log Streams are sequence of log events sharing the same source
- Log Groups: are collection of Log Streams. We can set retention, permissions and encryption on the log groups. By default log groups store data indefinitely
- Metric Filter: can be defined on the log group and will look for pattern in the log events. Essentially creates a metric from the log streams by looking at occurrences of certain patterns defined by us (example: failed SSH logs in events)
- Export logs from CloudWatch:
    - **S3 Export**: we can create an export task (`Create-Export-Task`) which will take up to 12 hours
    - **Subscription**: deliver logs in real time. We should create a subscription filter for the following destination: Kinesis Data Firehose (near real time), Elastic Search using Lambda or custom Lambda, Kinesis Data Streams (any KCL consumer)
- Subscription filters can be used to create a logging aggregation architecture