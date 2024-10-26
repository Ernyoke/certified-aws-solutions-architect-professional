# ELB - Elastic Load Balancer

## ELB Architecture

- It is the job of the load balancer to accept connection from customers and distribute those connections to any registered backend compute
- ELBs support many different type of compute services
- LB architecture:
![LB Architecture](images/ELBArchitecture1.png)
- Initial configurations for ELB:
    - IPv4 or double stacking (IPv4 + IPv6)
    - We have to pick the AZ which the LB will use, specifically we are picking one subnet in 2 or more AZs
    - When we pick a subnet, AWS places one or more load balancer nodes in that subnet
    - When an LB is created, it has a DNS A record. This A record points to all the nodes provisioned for the LB => all the incoming connections are distributed equally
    - The nodes are HA: if the node fails, a different one is created. If the load is to high, multiple nodes are created
    - We have to decide on creation if the LB is internal or internet facing. The internet facing the nodes will have public IP addresses otherwise private IP address is assigned. EC2 innstances need not have public IP address for internet facing LB. <span style="color: #ff5733;"><!Important for EXAM></span>
- Load Balancers are configured with listeners which accept traffic on a port and protocol and communicate with the targets
- An internat facing load balancer can connect to both public and private instances
- Minimum subnet size for a LB to function is /28 - 8 or more fee addresses per subnet (AWS suggests a minimum of /27)
- Internal LB are same as internet LB but they have private IP address assiged to the nodes. Internal LB are used to connect to private nodes and help in internal scaling,

## Cross-Zone Load Balancing

- An LB by default has at least one node per AZ that is configured for
- Initially each LB node could distribute connections to instances in the same AZ
- Cross-Zone Load Balancing: allows any LB node to distribute connections equally across all registered instances in all AZs.
- This help with the uneven distribution of load and could be helpful in <span style="color: #ff5733;">EXAM</span>
![CROSS-ZONE LB Architecture](images/ELBArchitecture2.png)

## User Session State

- Session state: 
    - A piece of server side information specific to one single user of one application
    - It does persist while the user interacts with the application
    - Examples of session state: shopping cart, workflow position, login state
- The date representing a sessions state is either stored internally or externally (stateless applications)
- Externally hosted session:
    - Session data is hosted outside of the back-end instances => application becomes stateless
    - Offers the possibility to do load balancing for the back-end instances, the session wont get lost in case the LB redirects the user to a different instance

## ELB Evolution

- Currently there are 3 different types of LB in AWS
- Load balancers are split between v1 and v2 (preferred)
- LB product started with Classic Load Balancers (v1)
- CLBs can load balance HTTP/HTTPS and lower level protocols as well, although they can not understand the http protocol, they can't make decision based on HTTP protocols features
- CLBs can have only 1 SSL certificate per load balancer
- They can not be considered entirely being a layer 7 product
- We should default to using v2 load balancer for newer deployments
- Version 2 (v2) load balancers:
    - Application Load Balancer (ALB - v2 LB) are layer 7 devices, they support HTTP(S) and WebSocket protocols
    - Network Load Balancers (NLB) are also v2 load balancers supporting lower level protocols such as TCP, TLS and UDP. 
      These could be used for applications like Email servers, Games or applications which does't use HTTP/s protocols.
- In general v2 load balancers are faster and they support target groups and rules, this allow to use single LB for multiple things.

## Application and Network Load Balancers

- Consolidation of load balancers:
    - Classic load balancers do not scale, they do not support multiple SSL certificates (no SNI support) => for every application a new load balancer is required
    - V2 load balancers support rules and target groups
    - V2 load balancers can have host based rules using SNI
- **Application Load Balancer (ALB)**:
    - ALB is a true layer 7 load balancer, configured to listen either HTTP or HTTPS protocols
    - ALB can not understand any other layer 7 protocols (such as SMTP, SSH, etc.)
    - ALB requires HTTP and HTTPS listeners
    - It can understand layer 7 content, such as cookies, custom headers, user location, app behavior, etc.
    - Any incoming connection (HTTP, HTTPS) is always terminated on the ALB - no unbroken SSL
    - All ALBs using HTTPS must have SSL certificates installed
    - ALBs are slower than NLBs because they require more levels of networking stack to process. Any <span style="color: #ff5733;">EXAM</span> question which talks about performance, NLB should be considered instead of ALB.
    - ALB offer health checks evaluation at application layer
    - Application Load Balancer Rules:
        - Rules direct connection which arrive at a listener
        - Rules are processed in a priority order, default rule being a catch all
        - Rule conditions: host-header, http-header, http-request-method, path-pattern, query-string and source-ip
        - Rule actions: forward, redirect, fixed-response, authenticate-oidc and authenticate-cognito
    - The connection from the LB and the instance is a separate connection
    - If you need to forward connections without terminating on the LB, then you need to consider NLB. (<span style="color: #ff5733;">EXAM</span>)
- **Network Load Balancer (NLB)**:
    - NLBs are layer 4 load balancers, meaning they support TPC, TLS, UDP, TCP_UDP connections
    - They have no understanding of HTTP or HTTPS => no concept of network stickiness
    - They are really fast, can handle millions of request per second having 25% latency of ALBs because they don't have to deal with any  of the heavy computational upper layers.
    - Recommended for SMTP, SSH, game servers, financial apps (not HTTP(S)) <-- <span style="color: #ff5733;">EXAM</span>
    - Health checks can only check ICMP or TCP handshake
    - They can be allocated with static IP addresses which is udeful for whitelisting which is beneficial for corporate client.
    - They can forward TCP straight through the instances => unbroken encryption <-- <span style="color: #ff5733;">EXAM</span>
    - NLBs can be used for PrivateLink <-- <span style="color: #ff5733;">EXAM</span>

- **Scenarios for NLB**:
    - Unbroken encryption
    - Static IP for whitelisting
    - Fast performance
    - Protocols other tha HTTP or HTTPS
    - Privatelink

## Session Stickiness

- Stickiness: allows us to control which backend instance to be used for a given connection
- With no stickiness connections are distributed across all backend services
- Enabling stickiness:
    - CLB: we can enable it per LB
    - ALB: we can enable it per target group
- When stickiness is enabled, the LB generates a cookie: `AWSALB` for ALB / `AWSELB` for CLB which is delivered to the end-user
- This cookie has a duration defined between 1 sec and 7 days
- When the user accesses the LB, it provides the cookie to the LB
- The LB than can decide to route the connection to the same backend instance every time while the cookie is not expired
- Change of the backed instance if the cookie is present:
    - If the instance to which the cookie maps to fails, then a new instance will be selected
    - If the cookie expires => the cookie will be removed, new cookie is created while a new instance is chosen
- Session stickiness problems: load can become unbalanced
- Enable session stickiness if an application does't use external sessions

## Connection Draining and Deregistration Delay

- Connection draining a setting which controls what happens when instances are unhealthy or deregistered
- Default behavior: LB closes all connections and the instance receives no new connections
- Connections draining allows in-flight requests to complete for a certain amount of time, while no new connections are sent to the instance
- Connection draining is supported on Classic Load Balancers only! It is defined on the load balancer itself
- Connection draining is a timeout between 1 and 3600 seconds (default 300)
- If the instance become unhealthy because if a failed health check, connection draining settings do not apply to it
- If an instance is taken out of service manually or by an ASG, it is listed "InService: Instance deregistration currently in progress". If we use an ASG, it will wait for all connections to complete before terminating or for the timeout value
- Deregistration delay is essentially the same feature as connection draining, but it is supported by ALB, NLB and GWLBs
- It is defined on target groups, not on the LB
- It works by stopping sending connections to deregistering targets. Existing connections can continue until thy complete naturally or the deregistration delay is reached
- Deregistration delay is enabled by default on all the new LBs, default value for it is 300 seconds (configurable between 0-3600 seconds)

## `X-Forwarded-For` and PROXY protocol

- In case a client connects to a backend without any load balancing in the front of the backend, the IP address of the client is visible and can be recorded
- With load balancers this can be more complicated, this is where `X-Forwarded-For` header and the PROXY protocol become handy
- `X-Forwarded-For` is a HTTP header, it only works with HTTP/HTTPS. This is a layer 7 header.
- This header is added/appended by proxies/load balancers. It can have multiple values in case the request is passing multiple proxies/load balancers. E.g X-Forwarded-For: 1.3.3.7(ClientIP), proxy1, proxy2..
- The backend server needs to be aware of this header and needs to support it
- Supported on CLB and ALB, NLB does not supports it because they don't support the layer 7 of the OSI stack.
- PROXY protocol works at Layer 4, it is an additional layer 4 (tcp) header => works with a wide range or protocols (including HTTP/HTTPS)
- There are 2 versions of PROXY protocol:
    - v1: human readable, works with CLB
    - v2: binary encoded, works with NLB
- v2 can support an unbroken HTTPS connection (tcp listener). Use case for this: end to end encryption
- When using PROXY protocol, we can add a HTTP header, the request is not decrypted