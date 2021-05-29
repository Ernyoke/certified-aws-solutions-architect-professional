# DX - Direct Connect

- It is a physical entry point into the AWS network
- It is a physical fibre connection through which we can access AWS services without sending traffic through the public internet
- In order to get a Direct Connect we have to create a connection (logical entity) inside an AWS account. This connection will have an unique ID
- We we create the connection, we have to specify a few details, the most important being:
    - The connection speed required (1 Gbps or 10 Gbps)
    - DX location
- AWS will allocate a DX port in the selected DX location, which is a fibre port on AWS DX router. Depending on the speed we will interface with it using **1000-Base-LX** or **10GBASE-LR** standards
- We will have to install a cross-connect in our network
![DX architecture](images/DirectConnectArchitecture1.png)
- Virtual Interface (VIF): VLAN (which provides isolation) and BGP session, grant access to AWS services
- Direct Connect ordered directly from AWS can have up to 50 VIFs per DX (+transit VIFs)
- Direct Connect ordered from AWS partner can have restrictions for the number of VIFs
- 3 types of VIFs can be run over DX:
    - Private VIF (VPC)
    - Public VIF (Public Zone Service)
    - Transit VIF
- Private VIF:
    - Provides private network connectivity, connects on-prem networks with VPCs
    - By default a private VIF can only connect to VPC in the same region where the connection is
- Public VIF:
    - Grants access to public AWS services such as S3, DynamoDB, SNS, SQS, etc.
    - Public VIF can not be used to access the public internet
- Hosted VIFs: we can share VIFs with other AWS accounts. Can be connected to a virtual private gateway in a VPC of the other account

## Direct Connect Types

- There are a few ways to get a DX:
    - Get the connection directly from AWS
        - Offers to speed options 1Gbps and 10Gbps
        - We get a port at a DX location, from which we have to arrange to connection to our location
        - We can run 50 VIFs + 1 transit VIF
    - Get the connection via partner
        - Offers wider range of speeds from 50Mbps up to 10Gbps
        - Hosted Connection: DX connection hosted an managed by the partner with **one VIF**
        - Hosted VIF individually: less ideal than a hosted connection, no dedicated bandwidth allocated

## Direct Connect - Other Notes

- Direct Connect offers no encryption!
- To overcome this: create a public VIF and establish a Site-to-Site VPN over it
- With direct connect we do not share any data cap with internet providers
- No transit over the internet, which means low and consistent latency
- DX offers cheaper data transfers and faster speeds compared to other methods
