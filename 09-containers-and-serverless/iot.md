# AWS IoT

- IoT - Internet of Things

## AWS IoT Core

- AWS IoT Core is a product set in AWS, used for managing millions of IoT devices
- IoT devices can be temp, wind, water sensors, light sensors, valve control sensors, etc.
- All of these need to be registered into a system to allow secure communication for managing them: provisioning, updates and control
- Communication to or from devices is likely to be unreliable, so AWS provides *device shadows*: virtual representations of actual devices, having the same configuration registered for the actual device. We can read from them the last communicated data, essentially the device communicates with the shadow, the last registered data can be retrieved anytime afterwards
- Device messages are sent JSON format, using MQTT protocols
- AWS IoT provides rules: event-driven integration with other AWS Services
- AWS IoT architecture:
    [AWS IoT architecture](images/ElasticTranscoder&AWSIoT.png)

## AWS IoT Device Management

- Helps us to register, organize, monitor and remotely manage IoT devices at scale

## AWS IoT Device Defender

- Used to audit configurations, authenticate devices, detect anomalies and receive alerts to help us secure our IoT device fleet

## AWS IoT 1-Click

- Used to launch AWS Lambda functions from IoT devices
- We can also create actions in the cloud or on-premises

## AWS Greengrass

- AWS Greengrass is an extension of the services provided by AWS IoT, moving those services closer to the edge
- Greengrass allow some services like compute, messages, data management, sync and ML capabilities to run from edge devices
- Devices with Greengrass Core software can locally run Lambda functions or containers => compute can run locally without leaving the local network
- Greengrass provides local device shadows which are synced back to AWS
- Allows messaging using MQTT
- Allows local hardware access for Lambda functions

## AWS IoT Analytics

- Used to run analytics on IoT data and get insights to make better and more accurate decisions
- Supports up to petabyte of data from millions of devices

## AWS IoT Events

- Used to detect and respond to events from IoT sensors and applications
- We can ingest data from multiple sources to detect the state of our processes or devices and proactively manage maintenance schedules

## AWS IoT SiteWise

- Simplifies collecting, organizing and analyzing industrial equipment data
- We can organize sensor data streams from multiple production lines and facilities to drive efficiencies across locations

## AWS IoT TwinMaker (formerly AWS IoT Things Graph)

- Used to create digital twins of real-world systems such as buildings, factories, industrial equipment and production lines
- We used this to quickly pinpoint and address equipment and process anomalies from the plant floor to improve worker productivity and efficiency