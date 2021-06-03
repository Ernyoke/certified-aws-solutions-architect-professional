# FSx

## FSx For Windows File Servers

- FSx for Windows are fully managed native Windows file servers/file shares
- Designed for integration with Windows environments
- Integrates with Directory Service or Self-Managed AD
- Resilient and highly available service. Can be deployed in single or multi-AZ within a VPC
- We can perform on-demand and scheduled backups using FSx
- FSx can be accessed over VPC peering, VPN and DX
- FSx supports de-duplication, scaling through Distributed File System (DFS), KMS at rest encryption and enforced encryption in transit
- Allows for volumes shadow copies, we can initiate previous versions for a file
- It is highly performant: 8 MB/s up to 2 GB/s throughput, 100k's IOPS, <1ms latency
- Features:
    - VSS - user-driven restores (view previous versions)
    - Native file system accessible over SMB
    - Uses Windows permission model
    - DFS - scale-out file share structure
    - Managed - no file server admin

## FSx for Lustre

- File system designed for high performance workloads
- Is a managed implementation of the Lustre file system, designed for HPC - Linux clients (POSIX file system)
- Lustre is designed for machine learning, big data, financial modelling
- Can scale to 100's GB/s throughput and offers sub millisecond latency