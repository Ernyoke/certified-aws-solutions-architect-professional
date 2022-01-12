# VPC - Virtual Private Cloud

## Public vs Private Services

- Public service: a service which is accessed by using public endpoints
- Private service: a service which runs inside a VPC
- Either private or public, every service can have permissions in order to be accessible
- VPC: private network isolated from the internet. Can't communicate to the network unless we are allowing it. Nothing from the internet can reach the services from a VPC as long as we do not configure it otherwise
- Internet Gateway: we can connect it to a VPC, this will allow the services in the VPC to communicate with the public internet

## DHCP in a VPC

- DHCP - Dynamic Host Configuration Protocol: offers auto configuration for network resources
- Every device has a hard-coded MAC address (Layer 2 address)
- DHCP begins with a L2 broadcast to discover a DHCP server on the local network
- Once discovered a DHCP server and a DHCP clients communicate, meaning that the client will get in the end an IP address, a Subnet Mask and Default Gateway address (L3 configuration)
- DHCP also configures which DNS server should a resource use in a VPC
- Also configures NTP servers, NetBios Name Servers and Node types
- For DNS server we can explicitly provide values or we can use `AmazonProvidedDNS`
- We also get allocated 1 or 2 DNS names for the services in the VPC. One can be public if the instance has a public IP address allocated
- Custom DNS names: we can give custom DNS names to EC2 instances if we use our own custom DNS servers
- DHCP options sets:
    - Once created option sets can not be changed
    - Can be associated with 0 or more VPCs
    - Each VPC can have a max of 1 option set associated
    - We we change a DHCP option set associated to the VPC, the change is immediate, but any new setting will only affect anything once a DHCP renew occurs
    - What we can configure in an option set:
        - DNS server (Route 53 resolver) what we can use in the VPC
        - NTP server

## VPC Router Deep Dive

- Is at the core of any network which involves AWS
- Is a virtual router in a VPC
- It is HA across al AZs in a region, no management overhead is required
- It is scalable, no management overhead required
- VPC routes routes traffic between subnets in a VPC
- Routes traffic from external network into the and vice-versa
- VPC router has an interface in every subnet in a VPC: `subnet+1` address (Default Gateway), the first IP address in each subnet after the network address itself
- We control how the VPC routes traffic using Route Tables

## VPC Route Tables

- Every VPC is created with a main Route Table (RT), which is the default for every VPC
- Custom route tables can be created for each subnet
- Subnets can be associated with only one RT which can be the main one or custom
- If we disassociate a custom RT form a subnet, the main RT will be attached to it
- Main RT should not be changed, custom RT should be used for any routing changes
- RT have routes, routes have an order, the most specific route wins
- Edge Association: a RT tables is associated with network gateway
- All RTs have at least one route: the local route which matches the VPC cidr range. These routes are un-editable

## NACL - Network Access Control Lists

- A NACL can be considered to be a traditional firewall in an AWS VPC
- NACLs are associated with subnets, every subnet has a NACL associated to it
- Connection inside a subnet are not affected by NACLs
- NACls can be considered stateless firewalls, so we can talk about the following type of rules:
    - Inbound rules: affect data coming into the subnet
    - Outbound rules: affects data leaving from the subnet
- Rules can explicitly **ALLOW** and explicitly **DENY** traffic
- Rules are processed in order:
    1. A NACL determines if a the inbound or outbound rules apply
    2. It starts from the lower rule number, evaluates traffic against each rule until is a match (based on IP range, port, protocol)
    3. Traffic is allowed/denied based on the rule
- Last rule is an implicit deny in every NACL, if no rule before that applies, traffic will be denied
- Default NACL: when a VPC is created, a default NACL is attached to it. The default NACL is allowing all traffic
- Custom NACL: we can create them and attach them to subnets. The default NACL denies by default all traffic. Can be associated with many different subnet, however each subnet can have only one NACL associated to it at any time
- NACL are not aware af any logical resources within a VPC, they are aware of IPs, CIDRs and protocols

## SG - Security Groups

- Security Groups are stateful firewalls, meaning they detect response traffic to a request and they automatically allow traffic
- SGs do not have explicit **DENY** rules, they can be used to block bad actors (use NACLs for this)
- SGs support IP/CIDR rules and also allow to reference logical resources
- SGs are attached to Elastic Network Interfaces (ENI), when we attach a SG to an EC2, the SG will be attached to the primary ENI
- SGs are capable to reference logical resources, ex. other security groups or self referencing

## AWS Site-to-Site VPN

- A Site-to-Site VPN is a logical connections between a VPC and an on-premise network running over the public internet. The connection is encrypted using IPSec
- Can be fully HA if it is implemented correctly
- It is quick to provision, it can be provisioned in less than an hour (contrast to DX)
- Components involved in creating a VPN connection:
    - **VPC**
    - **Virtual Private Gateway (VGW)**: it is a gateway object which can be the target of one or more rules in a Route Tables. It can be associated to a single VPC
    - **Customer Gateway (CGW)**: can refer to 2 different things:
        - Often is referred to the logical configuration in AWS
        - Physical on-premises router which the VPN connects to
    - **VPN Connection** itself: the connection linking the VGW from the AWS to the CGW
- Static vs Dynamic VPN:
    - **Dynamic VPN** uses BGP protocol, if customer router does not support BGP, we can not use dynamic VPNs
    - **Static VPN** uses static network configuration: static routes are added to the route tables AWS side, static networks has to be identified on the VPN connection on-premise side. It is simple, it just uses IPSec, works anywhere, having limitation on terms of HA
    - Dynamic VPN uses BGP: allows routing on the fly, allows multiple links to be used at once between the same locations. Allows using HA available architectures.
        - Route propagation: if enabled means that routes are added ro the Route Table automatically
- Considerations for VPN:
    - Speed Limitation for VPN: *1.25 Gbps*, AWS limitation
    - Latency considerations: inconsistent, traffic goes through the public internet
    - Cost: hourly cost for outgoing traffic
    - VPN can be used for Direct Connect backup or they can be used over the Direct Connect for adding a layer of encryption

## AWS Transit Gateway

- It is a network transit hub which connects VPCs to each other and to on-premise networks using Site-to-Site VPNs and Direct Connects
- It is designed to reduce the network architecture complexity in AWS
- It is a network gateway object, it is HA and scalable
- Attachments: we create attachments in order for the TGW to connect to VPCs and on-premise networks. Valid attachments are:
    - VPC attachments
    - Site-to-Site VPN attachments
    - Direct Connect Gateway attachments
- Attachments are configured in each subnet of the connected VPCs
- We can also peer transit gateways across cross regions and/or cross accounts
- We can also attach transit gateways to the DX connections
- Transit Gateway Considerations:
    - Supports transitive routing: single transit gateway with multiple attachments using route tables
    - Can be used to create global networks with peering
    - We can share transit gateways using AWS RAM
    - Transit Gateways offer less complex architectures compared to VPC peering solutions

## Advanced VPC Routing

- **Subnets are associated with 1 route table (RT) only, no more noe less!**
- This route tables is either the main route table from the VPC or a custom route table
- In case of a custom route table association with a subnet, the main route table is disassociated. In case the custom RT is removed, the main RT is associated again with the subnet
- RT can associated with an internet gateway (IGW) or virtual private gateway (VGW)
- IPv4/6 are handled separately within a RT
- Routes send traffic based on a destination to a target
- Route tables have a maximum of 50 static routes and 100 dynamic routes
- When a traffic arrives to an interface (IGW, VGW), it is matched to the relevant route table
- All routes from a route table are evaluated - highest-priority matching is used
- Route tables can contain 2 types of routes:
    - Static routes: added manually by us
    - Propagated routes: added when enabled by us on the VPC or on any individual RT
- Evaluation rule for the routes: 
    1. Longest prefix wins, example /32 wins over /24, /16 or /0. More specific routes always win!
    2. Static routes take priority over propagated routes
    3. For any routes learned by propagation:
        1. DX
        2. VPN Static
        3. VPN BGP
        4. AS_PATH (distance within two logical systems)

## Ingress Routing

- All outgoing traffic is routed to a security appliances
- The security appliance is sitting in the public subnet which has a RT assigned to it. This RT sends all unmatched traffic out through the IGW and anything for the corporate network through the VGW
- Ingress routing allows to assign route tables to gateways (Gateway route tables). **Gateway route tables** can be attached to internet gateways or virtual gateways and can be used to take action on inbound traffic (route to a security instance for assessment)
![Ingress Routing](images/AdvancedRouting5.png)

## Accelerated Site-to-Site VPN

- Performance enhancement for AWS Site-to-Site VPN that uses the AWS global network, the same network used for Global Accelerator and CloudFront
- Using a classic Site-to-Site VPN, the traffic goes through the public internet. In order to avoid this, some companies use a Site-to-Site VPN over Direct Connect. Direct Connect offers more better performance, but at a higher cost. Since DX is not an option for everybody, accelerated Site-to-Site VPN was created to improve performance compared to classic Site-to-Site VPNs
- Accelerated Site-to-Site VPN architecture:
![Accelerated Site-to-Site VPN](images/AcceleratedS2SVPN1.png)
- Acceleration can be enabled when creating a Transit Gateway attachment only! Not compatible with VPNs using virtual gateways (VGW)
- Accelerated Site-to-Site VPN has a fixed accelerator cost fee and a transfer fee

## VPC Endpoints

### Gateway Endpoints

- Gateway endpoints provide private access to supported services: **S3** and **DynamoDB**
- They allow any resource in a private only VPC to access S3/DynamoDB
- We crate a gateway endpoint per service per region and associate it to one or more subnets in a VPC
- We allocate a gateway endpoint to a subnet, a *Prefix List* is added to the route table for the subnet
- Any traffic targeted to S3/DynamoDB will go through the gateway endpoint and not through the internet gateway
- Gateway endpoints are highly available across all AZs in a region, they are not directly inside a VPC/subnet
- **Endpoint policy**: allows what things can be connected to the by the endpoint (example: a particular subset of S3 buckets)
- Gateway endpoints can be used to access services in the same region only
- Gateway endpoints allow private only S3 buckets: S3 buckets can be set to private allowing only access from the gateway endpoint. This will help prevent *Leaky Buckets*
- Gateway endpoints are logical gateway objects, they can be only accessed from inside the assigned VPC

### Interface Endpoints

- Interface endpoints provide private access to AWS public services similar to Gateway Endpoints
- Historically they have been used to provide access to services other than S3 and DynamoDB, recently AWS allowed interface endpoints to provide access to S3 as well
- Difference between gateway endpoints and interface endpoints is that interface endpoints are not HA. Interface endpoints are added to subnets as an ENI
- In order to have HA, we have to add an interface endpoint to every subnet per AZ inside of a VPC
- Interface endpoints are able to have security groups assigned to them (gateway endpoints do not allow SGs)
- We can also use endpoints policies, similar to gateway endpoints
- Interface endpoints support TCP only over IPv4
- Interface endpoints use PrivateLink behind the scene
- Gateway endpoints use prefix lists, interface endpoints use DNS. Interface endpoints provide a new DNS name for every service they are meant communicate with
- Interface endpoints are given a number of DNS names:
    - Endpoint Region DNS
    - Endpoint Zonal DNS
    - PrivateDNS overrides the default service DNS with a new version pointing to interface endpoint

### VPC Endpoints Policies

- Endpoints policies don't grant access to any AWS services in isolation
- Identities accessing resources still need they permissions to access resources
- An endpoint policy only limits access if the service is accessed to the specific endpoint
- The endpoint policy contains a policy and conditions (who has access to what)
- Policies are commonly used to limit what private VPCs can access

### Advanced VPC DNS and DNS Endpoints

- In every VPC the VPC.2 IP address is reserved for the DNS
- In every subnet the .2 is reserved for Route53 resolver
- Via this address VPC resources can access R53 Public and associated private hosted zones
- Route53 resolver is only accessible from the VPC, hybrid network integration is problematic both inbound and outbound
![Isolated DNS Environments](images/Route53Endpoints1.png)
- Solution to the problem before Route53 endpoints were introduced:
![Before Route53 Endpoints](images/Route53Endpoints2.png)
- Route53 endpoints:
    - Are deliver as VPC interfaces (ENIs) which can be accessed over VPN or DX
    - 2 different type of endpoints:
        - Inbound: on-premises can forward request to the R53 resolver
        - Outbound: interfaces in multiple subnets used to contact on-premises DNS
        - Rules control what requests are forwarded
        - Outbound endpoints have IP addresses assigned which can be whitelisted on-prem
- Route53 endpoint architecture:
![Route53 Endpoints Architecture](images/Route53Endpoints3.png)

## IPv6 Capability in VPCs

- IPv6 addresses are all publicly routable
- NAT is not used for IPv6, IPv6 does not need network address translation simply because of the huge number of available IPv6 addresses
- IPv6 needs to be manually enabled on a VPC. We can either bring our own IP address in a VPC or utilize an AWS provided range
- In case of AWS provided IPv6 addresses, AWS will allocate an uniq /56 range to the VPC. This range will be entirely uniq and all addresses will be publicly routable
- If we chose to allocate an IP range for a VPC, AWS will use a hex pair to uniquely allocate IP addresses to the subnets
- Routing is handled separately for the IPv6 addresses, we will have IPv4 routes and IPv6 routes
- Egress only internet gateway: similar to NAT gateway, allows outbound traffic denying inbound traffic in case of IPv6 addressing. NAT gateways or instances do not support IPv6!
- We can have both internet gateway and egress only interne t gateway associated to the same subnet
![IPv6 Architecture](images/IPv6EOIGW.png)
- IPv6 can be set up while creating a VPC/subnet or we can migrate an existing VPC to IPv6
- We can enable IPv6 on specific subnets only
- We can point IPv6 traffic to internet gateway and egress only internet gateways as well
- Not every service in AWS supports IPv6!

## Advanced VPC Structure - Subnets and Tiers

- Public subnets can be configured to not give public IP addresses to all instances by default. We can explicitly allocate public IP addresses to some resources
- If no public IP is addressed to a resource in a public subnet, it wont be accessible from the outside
- Security groups: we can restrict inbound traffic by allowing traffic from only selected instances
- How many subnets does an app need:
    - We don't need public and private subnets for addressing and security. This can be configured within one subnet. Exception to this: filter traffic using a NACL
    - We need different subnets for different routing
    - Internet-facing load balancers can communicates with private instances. Internet facing load balancer needs to run in a public subnet
    - Number of subnets needed: number of subnets needed for the APP * AZs
