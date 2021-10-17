# ELB - Elastic Load Balancer

## ELB Architecture

- It is the job of the load balancer to accept connection from an user base and distribute it to the underlying services
- ELBs support many different type of compute service
- LB architecture:
![LB Architecture](images/ELBArchitecture1.png)
- Initial configurations for ELB:
    - IPv4 or double stacking (IPv4 + IPv6)
    - We have to pick the AZ which the LB will use, specifically we are picking one subnet in 2 or more AZs
    - When we pick a subnet, AWS places one or more load balancer nodes in that subnet
    - When an LB is created, it has a DNS A record. The DNS name resolves all the nodes located in multiple AZs. The nodes are HA: if the node fails, a different one is created. If the load is to high, multiple nodes are created
    - We have to decide on creation if the LB is internal or internet facing (have public IP addresses or not)
- Listener configuration: what the LB is listening to (what protocols, ports etc.)
- An internat facing load balancer can connect to both public and private instances
- Minimum subnet size for a LB is /28 - 8+ fee addresses per subnet (AWS suggests a minimum of /27)

## Cross-Zone Load Balancing

- Initially each LB node could distribute traffic to instances in the same AZ
- Cross-Zone Load Balancing: allows any LB node to distribute connections equally across all registered instances in all AZs

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
- CLBs can load balance http and https and lower level protocols as well, although they can not understand the http protocol
- CLBs can have only 1 SSL certificates
- They can not be considered entirely being a layer 7 product
- Application Load Balancer (ALB - v2 LB) are layer 7 products supported HTTP(S) and WebSocket
- Network Load Balancers (NLB) are also v2 load balancers supporting lower level protocols such as TCP, TLC and UDP

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
    - ALBs are slower than NLBs because they require more levels of networking stack to process
    - ALB offer health checks evaluation at application layer
    - Application Load Balancer Rules:
        - Rules direct connection which arrive at a listener
        - Rules are processed in a priority order, default rule being a catch all
        - Rule conditions: host-header, http-header, http-request-method, path-pattern, query-string and source-ip
        - Rule actions: forward, redirect, fixed-response, authenticate-oidc and authenticate-cognito
    - The connection from the LB and the instance is a separate connection
- **Network Load Balancer (NLB)**:
    - NLBs are layer 4 load balancers, meaning they support TPC, TLS, UDP, TCP_UDP connections
    - They have no understanding of HTTP or HTTPS => no concept of network stickiness
    - They are really fast, can handle millions of request per second having 25% latency of ALBs
    - Recommended for SMTP, SSH, game servers, financial apps (not HTTP(S))
    - Health checks can only check ICMP or TCP handshake
    - They can be allocated with static IP addresses
    - They can forward TCP straight through the instances => unbroken encryption
    - NLBs can be used for PrivateLink

## Session Stickiness

- Stickiness: allows us to control which backend instance to be used for a given connection
- With no stickiness connections are distributed across all backend services
- Enabling stickiness:
    - CLB: we can enable it per LB
    - ALB: we can enable it per target group
- When stickiness is enabled, the LB generates a cookie: `AWSALB` which is delivered to the end-user
- This cookie has a duration defined between 1 sec and 7 days
- When the user accesses the LB, it provides the cookie to the LB
- The LB than can decide to route the connection to the same backend instance every time while the cookie is not expired
- Change of the backed instance if the cookie is present:
    - If the instance to which the cookie maps to fails, then a new instance will be selected
    - If the cookie expires => the cookie will be removed, new cookie is created while a new instance is chosen
- Session stickiness problems: load can become unbalanced