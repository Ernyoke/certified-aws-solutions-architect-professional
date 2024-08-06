# EFS - Elastic File System

- It is a network based file system which can be mounted on Linux based instance
- Can be mounted to multiple instances at once
- EFS it is an implementation of the NFSv4 file system format
- EFS file systems can be mounted in a folder in Linux based operating systems
- EFS storage exists separately from the lifecycle of an EC2 instance
- It can be shared between many EC2 instances
- It is a private service, it can be mounted via mount targets inside a VPC. By default an EFS it is isolated to the VPC in which was provisioned
- EFS can be accessed outside of the VPC over hybrid networking: VPN or DX. EFS is a great tool for storage handling across multiple units of compute
- EFS is accessible for Lambda functions. Lambda has to be configured to use VPC networking in order to use EFS
- Mount targets: provide IP addresses in the range of the VPC. For HA we should provision mount targets in every availability zones present in a VPC
- EFS offers 2 performance modes:
    - General Purpose: ideal for latency sensitive use cases (it is the default)
    - Max I/O: can be used to scale to higher levels of aggregate throughput. Has higher latencies
- EFS offers 2 different throughput modes:
    - Bursting (default): works similar to EBS GP2 storage
    - Provisioned: we can specify throughput requirements independent of the size
- EFS offers 3 storage classes:
    - Standard: for data that is accessed and modified frequently
    - Infrequent Access (IA): cost-optimized storage class for data that is less frequently accessed (few times a quarter)
    - Archive: for data that is accessed a few times a year
- We can automatically move data between these 2 classes using lifecycle policies

<img width="1290" alt="image" src="https://github.com/user-attachments/assets/2fc3161d-1c4d-4287-a1d4-ab8f424130b0">
