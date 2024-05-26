# SNS - Simple Notification Service

- It is a highly available, durable, secure, pub-sub messaging service
- It is a public AWS service: requires network connectivity with a public endpoint
- It coordinates the sending and delivery of messages
- Messages are <= 256 KB payloads
- SNS Topics are the base entity of SNS. On this topics are permissions controlled and configurations defined
- A publisher sends messages to a topic
- Topics can have subscribers which will receive messages from a topic
- Supported subscribers are:
    - HTTP(s)
    - Email (JSON)
    - SQS queues
    - Mobile Push Notifications
    - SMS messages
    - Lambda Functions
- SNS is used across AWS for notifications, example CloudWatch uses it extensively
- It is possible to apply filters to a subscriber
- Fan-out architecture: single topic with multiple SQS queue subscribers
- SNS offers delivery status for supported subscribers (HTTP, Lambda SQS)
- SNS supports delivery retries
- SNS it is a HA and scalable service within a region
- SNS supports Server Side Encryption (SSE)
- We can use cross-account access to a topic via topic policies

