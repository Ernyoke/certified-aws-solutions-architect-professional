# Snowball and Snowmobile

- Snowball series are designed to move large amount of data in or out of AWS
- The products in Snow series are physical storage units: suitcases and trucks
- We can order them empty, load them up and return them or vice-versa

## Snowball

- It is a device which is ordered from AWS, log a job and device will be delivered to us
- Any data stored in Snowball is encrypted using KMS
- There are 2 types of devices with 50TB and 80TB capacity
- In terms of network connectivity we can have 1Gbps (RJ45 1GBase-TX) or 10Gbps (LR/SR) networking
- Economical range for a Snowball is 10TB to 10PB range of data (multiple devices can be used)
- Multiple devices can be ordered and be sent to multiple business premises
- Snowball only includes storage capability

## Snowball Edge

- Includes both storage capability and compute capability
- It has a larger capacity compared to classic Snowball and has faster networking connection
- There are 3 different type of Snowball Edge:
    - Storage optimized (with EC2 capability): 80TB, 24vCPU, 32Gib RAM, 1TB SSD
    - Compute optimized: 100TB + 7.68 NVME, 52vCPU, 208Gib RAM
    - Compute optimized with GPU: 100TB + 7.68 NVME, 52vCPU, 208Gib RAM, GPU
- Ideal for remote sites or where data processing on ingestion is needed

## Snowmobile

- Portable data center within a shipping container on a truck
- Needs to be specially ordered from AWS
- Ideal for single location when 10 PB+ is required
- Can store up to 100PB of data per Snowmobile
- Not economical for multi-site or sub 10PB