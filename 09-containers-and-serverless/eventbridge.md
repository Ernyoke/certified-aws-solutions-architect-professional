# CloudWatch Events and EventBridge

- Deliver a near real-time stream of system events
- These events describe changes in AWS services, example: EC2 instance is started
- EventBridge is a newer system replacing CloudWatch Events. It can perform the same functionality, in addition it can handle events from third-parties and custom applications
- Both of the services operate using an event bus. Both have a default event bus
- In CloudWatch Events there is only the default event bus, which is explicit and it is not exposed to the UI
- In EventBridge we can have additional event buses
- In both systems we create rules matching incoming events, or we have scheduled based rules
- Events themselves are JSON objects, including for example which EC2 instance changed state, in what state changed into as well as other things such as date and time