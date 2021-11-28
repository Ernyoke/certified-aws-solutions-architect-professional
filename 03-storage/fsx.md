# FSx

## FSx For Windows File Servers

- FSx for Windows are fully managed native Windows file servers/file shares
- Designed for integration with Windows environments
- Integrates with AWS managed Directory Service or Self-Managed AD
- Resilient and highly available service. Can be deployed in single or multi-AZ within a VPC. Even in a single-AZ deployment the backend of the service uses replications to ensure that is resilient to hardware failure
- It can perform a full range of different kind of backups, including client-side and AWS-side features. On the AWS-side, it can perform automatic and on-demand backups
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
- Can be provisioned using 2 different deployment types:
    - Persistent: provides HA in one AZ only, provides self-healing, recommended for long term data storage
    - Scratch: highly optimized for short term solutions, no replication is provided
- FSx is available over VPN or DX for on-premises
- S3 repository: files are stored in S3 and they are lazily loaded into FSx for Lustre file system at first usage
- Sync changes between the file system and S3: `hsm_archive` command. The file system and the S3 bucket are not automatically in sync
- Lustre file system:
    - MST - Metadata stored on Metadata Targets
    - OST - Objects are stored on object storage targets, each can be up to 1.17 TiB
- Baseline performance of the file system is based on the size:
    - Size: min 1.2 TiB  and then we can use increments of 2.4 TiB
    - Scratch: base 200 MB/s per TiB of storage
    - Performance offers: 50 MB/s, 100 MB/s and 200 MB/s per TiB storage
    - For both types we can burst up to 1300 MB/s per TiB using credits