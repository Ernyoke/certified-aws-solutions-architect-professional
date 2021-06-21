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