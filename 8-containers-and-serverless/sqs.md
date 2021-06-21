# SQS - Simple Queue Service

- SQS provides managed message queues
- It is a public AWS service
- It is fully managed with highly available and highly performant queues by design
- Queues come in two types: Standard (ordering is best effort) and FIFO (guarantee an order)
- Messages added to the queue are up to 256 KB in size
- `VisibilityTimeout`: when a client receives a message, the message is hidden for a period of time. The `VisibilityTimeout` is the period which is given to the client in order to process the message
- If the client processes the message it has to delete the message from the queue. If the message is not deleted, after the `VisibilityTimeout` the message will be available for other clients
- **Dead-Letter queues**: all unprocessed messages can be sent here after a given number of retries. Allows different types of processing on the messages
- Queues can be used for scaling by ASGs, Lambdas can be invoked based on messages in the queue
- With SQS we are billed per request: 1 request can receive 1 - 10 messages up to 64KB total
- Polling SQS queues:
    - **Short polling** (immediate): can return 0 or more messages. It will return immediately if there are no messages on the queue
    - **Long polling**: we can specify `waitTimeSecond` value which can be up to 20 seconds to wait form messages when polling. It uses fewer requests than short polling
- SQS supports encryption at rest (KMS) and encryption in transit

## SQS Standard vs FIFO Queues

- Standard: 
    - Messages are delivered at least once
    - The order of the delivery is not guaranteed (best-effort ordering)
    - They don't have performance limitations
- FIFO: 
    - Messages are delivered exactly once
    - The order of the delivery is guaranteed to be the same as the messages were sent
    - Performance is limited: 3000 messages per second with batching or up to 300 messages per second without batching
    - There is also available a high throughput mode for FIFO queues
    - FIFO queues have to have `.fifo` suffix in order to be valid FIFO queues

## SQS Extended Client Library

- SQS has a message size limit of 256KB
- Extended Client Library can be used when we want to send messages larger than this size
- It can process large payloads and have the bulk of the payloads stored in S3
- When we send a message using `SendMessage` API, the library uploads the content to S3 and stores a link to this content in the message
- When receiving a message, the library loads the payload from S3 and replaces the link with the payload in the message
- When deleting a message from a queue, the large S3 payload will also be deleted
- The Extended Client Library has an implementation in Java

## SQS Delay Queues

- Delay queues allow us to postpone the delivery of messages in SQS queues
- For a delay queue we configure a `DelaySeconds` value. Messages added to the queue will be invisible for the amount of `DelaySeconds`
- The default value of `DelaySeconds` is 0, the max value is 15 minutes. In order for a queue to be delay queue, the value should be set to be greater than 0
- Message times allow a per-message basis invisibility to be set, overriding the queue setting. Min/max values are the same. Per message setting is not supported for FIFO queues

## SQS Dead Letter Queues (DLQ)

- Dead Letter Queues are designed to help handle reoccurring failures while processing messages from a queue
- Every time a message is received, the `ReceiveCount` is incremented. In order to not process the same message over and over again, we define a `maxReceiveCount` and a DLQ for a queue. After the number of retries is greater than the `maxReceiveCount`, the message is pushed into the DLQ
- Having a DLQ, we can define alarms to get notified in case of a failure
- Messages in DLQ can be further analyzed
- Every queue (including DLQ) have a retention period for messages. Every message has an `mq-timestamp` which represents when was the message was added to the queue. In case a message is moved into a DLQ, this timestamp is not altered
- Generally the retention period for a DLQ should be longer compared to normal queues
- A single DLQ can be used for multiple sources