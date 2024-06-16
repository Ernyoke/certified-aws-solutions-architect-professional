# AWS PrivateLink

- Allows us to connect to services hosted by other AWS accounts 
- We can connect to them directly or we can utilize AWS Marketplace partner services
- In both cases these services are presented in our VPC as private IP address ans ENIs
- AWS PrivateLink is the technical basis for Interface Endpoints
- For HA we should make sure we deploy multiple endpoint. Recommended one per AZ in each subnet we need to consume the service
- PrivateLink supports IPv4 and TCP only (~~IPv6 is not supported!~~, see: https://aws.amazon.com/about-aws/whats-new/2022/05/aws-privatelink-ipv6/)
- Private DNS is supported for overriding public DNS names (if there is a public DNS provided by the service we consume)
- PrivateLink endpoints can be accessed through Direct Connect, S2S VPN and VPC Peering

## VPC Endpoints 

### Gateway Endpoints

- Gateway endpoints provide private access to supported services: **S3** and **DynamoDB**
- They allow any resource in a private only VPC to access S3/DynamoDB
- We crate a gateway endpoint per service per region and associate it to one or more subnets in a VPC
- We allocate a gateway endpoint to a subnet, a *Prefix List* is added to the route table for the subnet. This prefix lists targets the gateway endpoint
- Any traffic targeted to S3/DynamoDB will go through the gateway endpoint and not through the internet gateway
- Gateway endpoints are highly available across all AZs in a region, they are not directly inside a VPC/subnet
- **Endpoint policy**: allows what things can be connected to the by the endpoint (example: a particular subset of S3 buckets)
- Gateway endpoints can be used to access services in the same region only
- Gateway endpoints allow private only S3 buckets: S3 buckets can be set to private allowing only access from the gateway endpoint. This will help prevent *Leaky Buckets*
- Gateway endpoints are logical gateway objects, they can be only accessed from inside the assigned VPC

### Interface Endpoints

- Interface endpoints provide private access to AWS public services similar to Gateway Endpoints
- Historically they have been used to provide access to services other than S3 and DynamoDB, recently AWS allowed interface endpoints to provide access to S3 as well
- Difference between gateway endpoints and interface endpoints is that interface endpoints are not HA by default. Interface endpoints are added to subnets as an ENI
- In order to have HA, we have to add an interface endpoint to every subnet per AZ inside of a VPC
- Interface endpoints are able to have security groups assigned to them (gateway endpoints do not allow SGs)
- We can also use endpoints policies, similar to gateway endpoints
- Interface endpoints support TCP only over IPv4
- Interface endpoints use PrivateLink behind the scene
- Gateway endpoints use prefix lists, interface endpoints use DNS. Interface endpoints provide a new DNS name for every service they are meant communicate with
- Interface endpoints are given a number of DNS names:
    - Endpoint Region DNS
    - Endpoint Zonal DNS
    - PrivateDNS: overrides the default service DNS with a new version pointing to interface endpoint

## VPC Endpoints Policies

- Endpoints policies don't grant access to any AWS services in isolation
- Identities accessing resources still need they permissions to access resources
- An endpoint policy only limits access if the service is accessed to the specific endpoint
- The endpoint policy contains a policy and conditions (who has access to what)
- Policies are commonly used to limit what private VPCs can access