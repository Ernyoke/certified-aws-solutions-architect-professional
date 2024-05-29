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