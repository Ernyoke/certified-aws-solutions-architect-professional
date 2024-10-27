# Gateway Load Balancers (GWLB)

- It is a product to help us run and scale third party security appliances
- This appliances can be firewalls, intrusion detection and prevention systems or even data analysis tools
- We can use these to perform inbound and outbound transparent traffic inspection and protection
- At high level a GWLB has to major components:
    - GWLB endpoints: run from a VPC where traffic enters/leaves via these endpoints. This are similar to interface endpoints with some key improvements
    - GWLB itself: there are normal EC2 instances running security software
- The GWLB needs to forward traffic without any alteration, the security appliance needs to review packets as they are sent/received
- GWLB use a protocol named GENEVE, this is a traffic and metadata tunneling protocol
- GWLBs are layer 3/4 devices, similar to NLB, but they integrate with GWLB endpoints and they encapsulate all traffic between them and the target using the GENEVE protocol. When the scanning is finished, the traffic is returned on the same tunnel to the GWLB from where, using the GWLB endpoint, it is returned to the intended destination
- GWLB will load balance between security appliances, so we can horizontally scale
- GWLB manage flow stickiness, one flow of data will always use the same appliance
- Ingress route tables:
    - They are placed on the internet gateway
    - They influence what happens with the traffic arriving to the VPC
    - They can redirect traffic to GWLB endpoints
- GWLB architecture:

    Below is an architecture where EC2 instances are running in a pair of private subnets followed by ALB which is runig in pair of public subnets. On thie right side we have a VPC running a set of security applicances inside ASG which ca grow or sink based on the load. 

    1. The traffic hits the IG which is configure with route table which determines what happens if the traffic arrives at VPC.
    2. Traffic is routed to GWLB endpoint. 
    3. Traffic is forwarded to GWLB itself whichis running in security VPC. At this point the packets still have original IP address.
    4. Packets are encapsulated using GENEVE protocol and forwarded to security appliances.
    5. Once the packets are aalyzed, they are returned to the GWLB.
    6. The packets are stripped and returned back to GWLB endpoint via the internet.
    7. Since the original IP are maintained, the traffic is routed to ALB using local route table.
    8. The traffic is forwarded to the choosen application instance.
    9. The traffic is returned back from instance to the GWLB in steps from 9 to 14, following the same flow as it came to the instance.
    15. The data is sent back to the original client using the IG. 


    ![GWLB Architecture](images/GWLB3.png)