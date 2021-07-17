# VM Migrations AWS <=> On-Premises

## Application Discovery Service (AMS)

- Allows us to discover on-premises infrastructure:
    - What VM we have
    - What CPU and memory they are allocated
    - MAC addresses
    - Resource utilization
    - etc.
- It also tracks these properties over time for more effective migration
- AMS runs in 2 modes:
    - Agentless (Application Discovery Agentless Connector): 
        - OVA appliance integrating with VMWare
        - Measures performance and resource usage, information which can be obtained from the outside of a VM
    - Agent Based mode: offers additional information from inside of a VM
        - Offers data gathering for network, processes, performance
        - We can see applications running on a VM
        - We can even see dependencies between VM based on network activity
- AMS integrates with AWS Migration Hub and Athena
- **AWS Migration Hub**: tracks migrations of different types in AWS

## Server Migration Service (SMS)

- Used to migrate whole VMs into AWS (including OS, Data, Apps, etc.)
- This is the tool which actually performs the migration
- It runs on agentless mode using a connector which runs on-premises
- Integrates with VMware, Hyper-V and AzureVM
- SMS does incremental replication of live volumes
- Offers orchestration of multi-servers migrations
- Creates AMIs which can be used to create EC2 instances
- Integrates with AWS Migration Hub
