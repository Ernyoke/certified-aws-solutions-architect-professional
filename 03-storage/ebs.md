# EBS and Instance Store

## EBS Volume Types

- **General Purpose SSD (GP2/GP3)**:
    - GP2 is the default storage type for EC2, GP3 is the newer version
    - A GP2 volume can be as small as 1GB or as large as 16TB
    - IO Credit:
        - An IO is 16 KB of data
        - 1 IOPS is 1 IO in 1 second
        - 1 IO credit = 1 IOPS
    - If we have no credits for the volume, we can not perform any IO
    - The IO bucket has 5.4 million of credits, it refills based at rate based on the baseline performance of the storage
    - The baseline performance for GP2 is based on the volume size, we get 3 IO credits per second, per GB of volume size
    - By default GP2 can burst up to 3000 IOPS
    - If we consume more credits than the bucket is refilling, than we are depleting the bucket
    - We have to ensure the buckets are replenishing and not depleting down to 0, otherwise the storage will be unusable
    - Volume larger than 1TB will exceed the burst rate of 3000 IOPS, they will always achieve the baseline performance as standard, they wont use the credit system
    - Max IO 16000 IO credits per second, any volume larger than 5.33 TB will achieve this maximum rate constantly
    - GP2 can be used for boot volumes
    - GP3 is similar to GP2, but it removes the credit system for a simpler way of working:
        - Every GP3 volume starts at a standard 3000 IOPS and 125 MiB/s regardless of volume size
    - Base price for GP3 is 20% cheaper than GP2
    - For more performance we can pay extra cost for up to 16000 IOPS or 1000 MiB/s throughput
- **Provisioned IOPS SSD (IO1/2)**:
    - There are 3 types of provisioned IOPS storage options: IO1 and its successor IO2 and IO2 BlockExpress (currently in preview)
    - For this storage category the IOPS value can be configured independently of the storage size
    - Provisioned IOPS storages are recommended for usage where consistent low latency and high throughput is required
    - Max IOPS per volume is 64_000 IOPS per volume and 1000 MB/s throughput, while with BlockExpress we can achieve 256_000 IOPS per volume and 4000 MB/s throughput
    - Volume size ranges from 4 GB up to 16 GP for IO2/IO3 and up to 64 TiB for BlockExpress
    - We can allocate IOPS performance values independently of the size of the volume, there is a maximum IOPS value per size:
        - IO1 50 IOPS / GB MAX
        - IO2 500 IOPS / GB MAX
        - Block Express 1000 IOPS / GB MAX
    - Per instance performance: maximum performance between EBS service and EC2. Usually this implies more than one volume in order to be saturated. Max values:
        - IO1 260_000 IOPS and 7500 MB/s (4 volumes to saturate)
        - IO2 160_000 IOPS and 4750 MB/s 
        - Block Express 260_000 IOPS and 7500 MB/s
    - Use cases: smaller volumes and super high performance
- HDD based volume types:
    - There are 2 types of HDD based storages: ST1 Throughput Optimized, SC1 Cold HDD
    - **ST1**:
        - Cheaper than SSD based volumes, ideal for larger volumes of data
        - Recommended for sequential data, applications when throughput is more important than IOPS
        - Volume size can be between 125 GB and 16 TB
        - Offers maximum 500 IOPS, data is measured in blocks of 1 MB => max throughput of 500 MB/s
        - Works similar as GP2 with a credit system
        - Offer a base performance of 40 MB/s per TB of volume size with bursting to 250 MB/s per TB
        - Designed for frequently accessed sequential data at lower cost
    - **SC1**:
        - SC1 is cheaper than ST1, has significant trade-offs
        - Geared towards maximum economy when we want to share a lot of data without caring about performance
        - Offers a maximum of 250 IOPS, 250 MB/S throughput
        - Offer a base performance of 12 MB/s per TB of volume size with bursting to 80 MB/s per TB
        - Volume size can be between 125 GB and 16 TB
        - It is the lower cost EBS storage available

## Instance Store Volumes

- Provides block storage devices, raw volumes which can be mounted to a system
- They are similar to EBS, but they are local drives instead of being presented over the network
- These volumes are physically connected to the EC2 host, instances on the host can access these volumes
- Provides the highest storage performance in AWS
- Instance stores are included in the price of EC2 instances with which they come with
- Instance stores have to be attached at launch time, they can not be added afterwards
- If an EC2 instance moves between hosts the instance store volume loses all its data
- Instances can move between hosts for many reasons: instance are stopped and restarted, maintenance reasons, hardware failure, etc.
- **Instance store volumes are ephemeral volumes!**
- One of the primary benefit of instance stores is performance, ex: D3 instance provides 4.6 GB/s throughput, I3 volumes provide 16 GB/s of sequential throughput with NVMe SSD
- Instance store considerations:
    - Instance store can be added only at launch
    - Data is lost on an instance stores in case the instance is moved, resized or there is a hardware failure
    - Instance stores provide high performance
    - For instance store volumes we pay for it with the EC2 instance
    - Instance store volumes are temporary!

## Choosing between Instance Store and EBS

- Fer persistence storage we should default to EBS
- For resilience storage we should avoid instance store an default to EBS
- If the storage should be isolated from EC2 instance lifecycle we should use EBS
- Resilience with in-built replication - we can use both, it depends on the situation
- For high performance needs -  we can also use both, it depends on the situation
- Fos super high performance we should use instance store
- If cost is a primary concern we can use instance store if it comes with the EC2 instance
- Cost consideration: cheaper volumes: ST1 or SC1
- Throughput or streaming: ST1
- Boot volumes: HDD based volumes are not supported (no ST1 or SC1)
- GP2/3 - max performance up to 16000 IOPS
- IO1/2 - up to 64000 IOPS (BlockExpress: 256000)
- RAID0 + EBS: up to 260000 IOPS (maximum possible on an EC2 instance)
- For more than 260000 IOPS - use instance store