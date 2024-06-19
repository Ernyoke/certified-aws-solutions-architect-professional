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
    - Supports DFS - scale-out file share structure, group file shares together in one enterprise-wise structure
    - Managed - no file server admin
    - Integrates with Amazon managed DS or our own directory

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
- FSx for Luster deployment types:
    - Scratch:
        - Is designed for pure performance, for short term and temporary workloads
        - Does not provide any type of HA or replication, in case of a HW failure all the data stored on that hardware is lost
        - Larger file systems mean more servers, more disks => more chance of failure
    - Persistent:
        - Has replication within one AZ only
        - Auto-heals when hardware failure occurs
    - Both of these options provide backups to S3 (manual or automatic with a retention of 0-35 days)

## FSx for NetApp ONTAP

- Fully managed storage built on NetAPP ONTAP
- Provides reach set of features available with NetApp's data management software:
    - Storage efficiencies: compression, deduplication, compaction, thin provisioning
    - Low-cost, fully elastic capacity pool tiering
    - Data protection: snapshots, SnapVault and native Amazon FSx backups
    - Disaster recovery using SnapMirror and Amazon FSx Backups
    - Caching: FlexCache, Global File Cache
    - Multiprotocol access from Linux, Windows
    - Other features: antivirus scanning
- Intelligent policy-based data movement between tier:
    - Amazon FSx for NetApp ONTAP file system has two storage tiers: primary storage and capacity pool storage
        - Primary storage is provisioned, scalable, high-performance SSD storage (up to 192 TB) that’s purpose-built for the active portion of our data set
        - Capacity pool storage is a fully elastic storage tier that can scale to petabytes in size and is cost-optimized for infrequently accessed data
    - Enabling tiering allows intelligent data movement between tiers based on the access
- Use case for NetApp ONTAP:
    - Backup and archive using SnapVaults
    - Cross region DR copy of FSx file data using SnapMirror
    - FlexCache for caching data and bringing data closer between regions and on-prem access
    - Can be used with Amazon WorkSpaces to provide shared network-attached storage (NAS) or to store roaming profiles for Amazon WorkSpaces accounts

## FSx for OpenZFS

- Fully managed file storage service built on the open-source OpenZFS file system
- Can be accessed over the industry-standard Network File System (NFS) protocol
- Powered by AWS Graviton processors, along with the latest AWS disk and networking technologies
- Use cases:
    - Migration of on-premises data stored in ZFS or other Linux-based file servers to AWS
    - Wide range of Linux, Windows, and macOS workloads, including big data and analytics, code and artifact repositories, DevOps solutions, web content management, front-end electronic design automation (EDA), genomics research, and media processing
- Data security:
    - Encryption of data at rest is automatically enabled when we create an Amazon FSx for OpenZFS file system through the AWS
    - Amazon FSx for OpenZFS uses industry-standard AES-256 encryption algorithm to encrypt file system data at rest
    - Amazon FSx for OpenZFS file systems automatically encrypt data in transit when they are accessed from Amazon EC2 instances that support encryption in transit