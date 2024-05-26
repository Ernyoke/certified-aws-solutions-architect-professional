# AWS IOT

- IOT - Internet of Things
- AWS IOT is a product set in AWS, used for managing millions of IOT devices
- IOT devices can be temp, wind, water sensors, light sensors, valve control sensors, etc.
- All of these need to be registered into a system to allow secure communication for managing them: provisioning, updates and control
- Communication to or from devices is likely to be unreliable, AWS provides *device shadows*: virtual representations of actual devices, having the same configuration registered for the actual device. We can read from them the last communicated data, essentially the device communicates with the shadow, the last registered data can be retrieved anytime afterwards
- Device messages are sent JSON format, using MQTT protocols
- AWS IOT provides rules: event-driven integration with other AWS Services
- AWS IOT architecture:
    [AWS IOT architecture](images/ElasticTranscoder&AWSIoT.png)

## AWS Greengrass

- AWS Greengrass is an extension of the services provided by AWS IOT, moving those services closer to the edge
- Greengrass allow some services like compute, messages, data management, sync and ML capabilities to run from edge devices
- Devices which Greengrass software can locally run Lambda functions, containers
- Provides local device shadows which are synced back to AWS
- Allows messaging using MQTT
- Allows local hardware access for Lambda functions