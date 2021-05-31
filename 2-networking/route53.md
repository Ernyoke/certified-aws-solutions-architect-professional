# Route 53

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

## Route 53 Public Hosted Zones

- A hosted zone is DNS database for a given section of the global DNS database
- Hosted zones are created automatically when a domain is registered in R53, they can be created separately as well
- There is a monthly fee for each running hosted zone
- A hosted zone hosts DNS records, example A, AAAA, MX, NS, TXT etc.
- Hosted zones are authoritative for a domain
- When a public hosted zone is created, R53 allocates 4 public name servers for it, on these name servers the zone file is hosted
- We use NS records to point at these name servers to be able to connect to the global DNS
- Externally registered domains can point to R53 public zone

## Route 53 Private Hosted Zones

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

## Route 53 Simple Routing

- With simple routing with can create one record per name
- Each record can have multiple values
- In case of a request, all the values for the record are returned to the client
- The client choses one of the values an connects to the server
- Limitations: does not support health checks!

## Route 53 Health Checks

- Health checks are separate from, but used by records in Route 53
- They are created separately and they can be used by records in Route 53
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