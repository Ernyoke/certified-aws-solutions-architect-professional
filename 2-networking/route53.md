# Route53

## DNS Fundamentals

- DNS is a discovery service, translate information which machines need into information that humans need and vice-versa
- Example: www.amazon.com => 104.98.34.131
- DNS database is a huge distributed database
- DNS allows a DNS resolver server to find a Zone File ona Name Sever (NS) and query the it, retrieving the necessary IP address for a DNS name

## DNS Terminology

- **DNS Client**: refers to a customer PC, laptop, tablet, etc.
- **DNS Resolver**: software running on a device or a server which queries DNS on our behalf
- **DNS Zone**: a part of the DNS database (example: amazon.com)
- **Zone file**: it is a physical database for a zone, contains all the DNS information for a particular domain
- **DNS Name server**: a server which hosts the zone files

## DNS Root

- DNS is structures like a tree, DNS root is at the top of the tree
- DNS root is hosted in 13 special nameservers, known as DNS Root Servers
- Root hints file: an OS file containing the address of all root servers
- Authority: when something is trusted in DNS, example root hints file
- Delegation: trusted authorities can delegate a part of themselves to other entities, those entities becoming authoritative for the part delegated

## Route 53 Introduction

- It is a managed DNS product
- Provides 2 main services:
    - Register domains
    - Can host zone files on managed nameservers
- It is a global service, its database is distributed globally and resilient

## Hosted Zones

- DNS as a service
- Let's us create and manage zone files
- Zone files are hosted on four managed nameservers
- A hosted zone can be public, accessible for the public internet, part of the public DNS system
- It can be private, linked to a VPC(s)
- A hosted zone stores DNS records (recordsets)

## DNS Record Types

- Nameserver (NS): allow delegation to occur in DNS
- A and AAAA records: map host names to IP addresses. A records maps a host name to IPv4, AAAA maps the host to IPv4 addresses
- CNAME (canonical name): maps host to host records, example ftp, mail, www can reference different servers. CNAME can not point directly to IP addresses, they can point to other names only
- MX records: used for sending emails. Can have 2 parts: priority and value
- TXT (text) records: arbitrary text to domain. Commonly used to prove ownership

## DNS TTL - Time To Live

- Indicates how long records can be cached for
- Resolver server will store the records for the amount of time specified by the TTL in seconds
- TTL is a balance: low values means less queries to the server, high values mean less flexibility when the records are changed

## Route53 Public Hosted Zones

- A hosted zone is DNS database for a given section of the global DNS database
- Hosted zones are created automatically when a domain is registered in R53, they can be created separately as well
- There is a monthly fee for each running hosted zone
- A hosted zone hosts DNS records, example A, AAAA, MX, NS, TXT etc.
- Hosted zones are authoritative for a domain
- When a public hosted zone is created, R53 allocates 4 public name servers for it, on these name servers the zone file is hosted
- We use NS records to point at these name servers to be able to connect to the global DNS
- Externally registered domains can point to R53 public zone

## Route53 Private Hosted Zones

- Similar to a public hosted zone except it can not be accessed from the public internet
- It is associated with VPCs from AWS and it only can be accessed from VPCs from the account. Cross account access is possible
- Split-view: it is possible to create split-view (split-horizon) DNS for public and internal use with the same zone name. Useful for accessing systems from the private network without accessing the public internet

## CNAME vs ALIAS Records

- The problem with CNAME:
    - An A record maps a NAME to an IP Address
    - A CNAME records maps a NAME to another NAME
    - We can not have a CNAME record for an APEX/naked domain name
    - Many AWS services use DNS Name (example: ELBs)
- For the APEX domain to point to another domain, we can use ALIAS records
- An ALIAS record maps a NAME to an AWS resource
- Can be used for both apex and normal records
- There is no additional charge for ALIAS requests pointing at AWS resources
- An alias is a subtype, we can have an A record alias and a CNAME record alias

## Route53 Simple Routing

- With simple routing with can create one record per name
- Each record can have multiple values
- In case of a request, all the values for the record are returned to the client
- The client choses one of the values an connects to the server
- Limitations: does not support health checks!

## Route53 Health Checks

- Health checks are separate from, but used by records in Route53
- They are created separately and they can be used by records in Route53
- Health checks are performed by a fleet of health checkers distributed globally
- Health checks are not just limited to AWS targets, can be any service with an IP address
- Health checkers check every 30s (or 10s at extra cost)
- Health checks can be TCP, HTTP, HTTPS, HTTP(S) with String Matching. A TPC connections should be completed in 4s and endpoint should respond with a 2xx or 3xx status code within 2s after connections
- In case of string matching the text should be present entirely in the first 5120 characters of the request body or the endpoint fails the health check
- Based on the health checks the service can be categorized as `Healthy` or `Unhealthy`
- Health checks can be of 3 types:
    - Endpoint
    - CloudWatch Alarm
    - Calculated (status of other health checks)

## Route53 Failover Routing

- We can add 2 records of the same name (a primary and a secondary)
- Health checks happen on the primary record
- If the primary record fails the health checks, the address of the secondary record is returned
- Failover routing should be used when we configure active-passive failover

## Route53 Multi Value Routing

- Multi Value Routing is mixture to simple and failover routing
- With multi value routing we can create many records with the same name
- Each record can have an associated health check
- When queried, 8 records are returned to the client. If more than 8 records are present, 8 records will be randomly selected and returned
- The client picks one a of the values and connects to the service
- Any records which fails the health checks won't be returned when queried
- Multi value routing is not a substitute for an actual load balancer

## Route53 Weighted Routing

- Weighted Routing can be used when looking for a simple form of load balancing or when we want to test new versions of an API
- With weighted routing we can specify a weight for each record
- For a given name the total of weight is calculated. Each record is returned depending on the percentage of the record compared to the total weight
- Setting a weight to 0, the record will not be returned
- Weighted routing can be combined with health checks. Health checks don't remove records from the calculation of the total weight. If a record is selected, but it is unhealthy, another selection will be made until a healthy record is selected

## Route53 Latency-Based Routing

- Should be used when we trying to optimize for performance and better user experience
- For each record we can specify an region
- AWS maintains a list of latencies for each region from the world (source - destination latency)
- When a request comes in, it will be directed to the lowest latency destination based on the location from where the request is coming from
- Latency-based routing can be combined with health checks. If the lowest latency record fails, the second lowest latency record will be returned to the client
- The database maintained by AWS is not updated in real time

## Route53 Geolocation Routing

- It is similar to latency-based routing
- The location of customer and the location of the resource are used to influence resolution decisions
- When a record is created, it is tagged with a location (country, continent or default)
- Subdivision: in America we can tag records with a state
- Geolocation does not return the closest record, it will return the relevant record
- Geolocation is ideal for restricting content