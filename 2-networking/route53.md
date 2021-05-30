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
- **Zonefile**: it is a physical database for a zone
- **DNS Nameserver**: a server which hosts the zonefiles

## DNS Root

- DNS is structures like a tree, DNS root is at the top of the tree
- DNS root is hosted in 13 special nameservers, known as DNS Root Servers
- Root hints file: an OS file containing the address of all root servers
- Authority: when something is trusted in DNS, example root hints file
- Delegation: trusted authorities can delegate a part of themselves to other entities, those entities becoming authoritative for the part delegated
