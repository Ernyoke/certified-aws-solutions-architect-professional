# AWS Global Accelerator

- Designed to optimize the flow of data from user to AWS
- Similar to CloudFront, both improve performance when communicating with services hosted in AWS
- Global Accelerator provides 2 **anycast** IP addresses. Anycast IP addresses are special type of IP addresses
- Normal IP addresses are called **unicast** IP addresses, these refer to one device in the network
- In contrast anycast IP addresses can be used by multiple devices at the same time, the traffic will be routed to the device closest to the source
- Global Accelerator uses the AWS Edge Locations. Since multiple locations advertise the given anycast IP addresses, the traffic will be routed to the Edge Location closer to the user
- AWS has its own dedicated network consisting in fiber links. Edge Locations relay traffic to this network improving performance

## CloudFront vs Global Accelerator

- CloudFront moves the content closer to the customer by caching it on the Edge Location. Global Accelerator moves the customer closer to the service pe providing access to the AWS global network
- Global Accelerator is a network product: works on any TCP/UDP applications including web apps (HTTP/HTTPS). CloudFront only caches HTTP/HTTPS content
- Global Accelerator does not cache anything. It does not understand the HTTP/HTTPS protocol