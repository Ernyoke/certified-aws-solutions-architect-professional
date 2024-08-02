## Amazon Aurora

## Aurora Architecture

- Aurora architecture is very different from RDS
- It uses the base entity of a cluster, which something that other instances of RDS database do not have
- Aurora does not use local storage for the compute instances, instead an Aurora cluster has a shared custom volume
- A cluster is made up from a number of important things:
    - A single primary instance + 0 or more replicas
    - The replicas can be used for read during normal operations
- Aurora uses a *Cluster*:
    - Made up of a single primary instance and 0 or more replicas
    - The replicas can be used for reads (not like the standby replica in RDS)
    - Storage: the cluster uses a shared cluster volume (SSD based by default). Provides faster provisioning, improved availability and better performance. Size can go up to 128 TiB
    - The storage has 6 replicas across AZs. The data is replicated synchronously. Replication happens at the storage level, no extra resources are consumed for replication
    - By default only the primary instance are able to write to the storage, replicas and the primary can perform read operation
    - Self-healing: Aurora can repair its data if a replica or part of the replica if there is a disk failure
    - Aurora uses the cluster to repair the data with no corruption. As a result, Aurora avoids data loss and reduces needs for point-in-time restores or snapshot restores
    - With Aurora can have up to 15 replicas, any of the replicas can be failed over to
    - Billing for Aurora storage:
        - Storage is billed to what we consume up to 128 TiB limit
        - High water mark: we get billed for the most used data at a time, in case of free-up we will be billed for the max usage consumed
        - In case we have to reduce data significantly, we will need to migrate the database to another cluster to avoid paying for the storage
        - Recently Aurora introduced dynamic resizing, where we have to pay for only what we use. It is recommended to upgrade our database to an Aurora version which supports dynamic resizing
    - Aurora clusters use endpoints, providing multiple endpoints:
        - **Cluster endpoint**: always point to the primary instance, can be used for reads and writes
        - **Reader endpoint**: points to the primary instance and also to the read-replicas. Aurora does load balancing we using this endpoint
        - **Custom endpoint**: can be created by us
        - **Instance endpoint**: each instance has its own endpoint

## Aurora Costs

- No free-tier option, Aurora does not support micro instances
- Beyond RDS singleAZ (micro) Aurora offers better value compared to other RDS options
- Compute: hourly charge, per second, 10 minute minimum
- Storage: GB/month consumed, IO cost per request made to the cluster's shared storage
- Backups: 100% DB size in backups are included

## Aurora Restore, Clone and Backtrack

- Backups in Aurora work the same way as other RDS
- Restores create a new cluster
- Backtrack can be enabled per cluster. They allow in-place rewinds to a previous point-in-time
- Fast clone: makes a new database much faster than copying all the data. Aurora references the original storage, only stores any differences between the old data and the new one

## Aurora Serverless

- It provides a version of Aurora without the need to statically provision the database instance
- Removes admin overhead for managing db instances
- Aurora Serverless uses the concept of ACU - Aurora Capacity Units: represent a certain amount of compute and a corresponding amount of memory
- We can set minimum and maximum ACU values per cluster, can go down to 0
- Consumption billing is per-second basis
- Aurora Serverless provides the same resilience as Aurora provisioned (6 copies across AZs)
- Aurora cluster architecture still exists, but in a for of serverless cluster. Instead of using provisioned instances we have capacity units
- Capacity units are allocated from a warm pool of Aurora instances managed by AWS
- ACUs are stateless, shared across multiple AWS customers
- If the load increases beyond the ACU limit and the pool allows it, than more ACU will be allocated to the instance
- In Aurora Serverless we have shared Proxy Fleet for connection management:
    - It is used to distribute connections from us, Aurora users, to Aurora capacity units
    - We never directly connect to Aurora, this makes Aurora scaling seamless
- Aurora use cases:
    - Infrequently used applications
    - New applications where we are unsure about the levels of load that will be places on the application
    - Variable and/or unpredictable workloads
    - Development and test databases: Aurora can be configured to stop itself
    - Multi-tenant applications where the scaling is aligned with infrastructure size and revenue

## Aurora Multi-Master

- Default Aurora mode is single-master: one read/write endpoint and 0 or more read replicas
- In contrast with default mode for Aurora, multi-master offers multiple endpoints which can be used for reads and writes
- There is no cluster endpoint to use, the application is responsible for connection to instances in the cluster
- Benefits:
    - Multiple writer endpoints, if we have an application which can failover between endpoints, the failover time can be significantly reduced
    - Fault tolerance can be implemented at the application level, but the application needs to manually load balance between the instances