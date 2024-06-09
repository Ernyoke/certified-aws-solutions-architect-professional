# AWS Transit Gateway

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

## Transit Gateway - Deep Dive

- Transit gateway is a hub-and-spoke architecture, it can connect to various types of networking objects within AWS
- Integration with Direct Connect:
    - A Transit VIF is required which goes through a DX Gateway
    - The DX Gateway can be attached to the Transit Gateway with a Transit Gateway Attachment
    - 1 DX Gateway can be attached to 3 Transit Gateways
- Transit Gateway has a default route table which is populated from the attachments:
    - For the VPCs we have the CIDR ranges of these VPCs
    - For VPNs we have the routes learned via BGP
    - For DX Gateways with the Transit Gateway Attachment we define the networks within the attachment configured at the DX Gateway side
- We can peer TGWs with other TGWs between regions. We can peer a TGW with up to 50 other TGWs, and these TGWs can also peer with other TGWs
- A TGW by default has one route table
- All attachments use this RT for routing decisions, by default all attachments propagate routes to this route table, exception peering attachments
- All attachments can route to all other attachments by default

## Transit Gateway Peering

- In case of peering attachments routes are not shared, we need to use static routes, similar to VPC peering (AWS recommends using unique ASNs for future enhancements for route advertisements)
- Resolution of public DNS to private addressing is not supported over inter-region peers
- Data transfer over peering connection is encrypted and is sent over AWS network
- We can peer up to 50 peering attachments per TGW, these can be in different regions, different AWS accounts

## Transit Gateway Isolated Routing

- By default:
    - All attachments are associated with the same route table
    - All attachments propagate to the same route table, all attachments are aware of any other attachments
- Attachments can only be associated with 1 route table, route tables can be associated to many attachments
- Attachments can propagate to many RTs, event to those they are not associated with
- If we would want to isolate networks:
    - We create a route table and we configure all attachments to propagate to the route table
    - We associate the route table with only the attachments we would want to communicate with each other
    - We create another route and associate it to the attachment we don't want to communicate with each other. We configure other attachments to propagate to this route table