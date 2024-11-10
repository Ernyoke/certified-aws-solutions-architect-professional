# VPC Flow Logs

- Essential diagnostic tools for complex networks
- They only capture packet metadata, they do not capture packet content. For packet content a packet sniffer is required to be installed on an instance
- Metadata can include: source/destination IP, source/destination ports, packet size, other externally visible metadata, etc.
- Flow logs can capture data at various different points:
    - Applied to a VPC: all interfaces in that VPC
    - Subnet: every network interface in the subnet only
    - Network interface: only monitor traffic at a specific interface
- VPC Flow Logs are NOT realtime, there is delay between traffic leaving monitored interfaces and showing up in the flow logs <span style="color: red;">EXAM</span>
- Flow logs can be configured to use S3, CloudWatch Logs or Kinesis Firehose for the destination
- Flow logs can be cofigured to capture data only at accepted connections, only at rejected connections or they can capture metadata at all connections.

## VPC Flow Logs Record Content

- `<version>`
- `<account-id>`
- `<interface-id>`
- `<srcaddr>`: source IP address
- `<dstaddr>`: destination IP address
- `<srcport>`: source port, 0 if no port is used (example in case of ICMP ping)
- `<dscport>`: destination port
- `<protocol>`: ICMP=`1`, TPC=`6`, UDP=`17`, etc. <span style="color: red;">Need to remember for EXAM</span>
- `<packets>`
- `<bytes>`
- `<start>`
- `<end>`
- `<action>`: traffic is `ACCEPT`ed or `REJECT`ed
- `<log-status>`

## Notes

- VPC Flow Logs do not log all the traffic, things like the communication with the metadata IP (169.254.169.254), AWS time sync server (169.254.169.123), DHCP, Amazon DNS server and Amazon Windows license is not recorded
