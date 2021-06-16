# RDS - Relational Database Service

- RDS is often described as a Database-as-a-service (DBaaS)
- RDS provides managed database instances, which can themselves hold one or more databases
- Benefits of RDS are the we don't need to manage the physical hardware, the server operating system or the database system itself
- RDS supports MySQL, MariaDB, PostgreSQL, Oracle, Microsoft SQL Server
- Amazon Aurora: it is a db engine created by AWS and we can select it as well for usage

## RDS Database Instance

- Runs one of the few types of db engine mentioned above
- Can contain multiple user created databases
- A database instance after creation can be accessed using its hostname (CNAME)
- RDS instances come in various types, share many of features of EC2. Example of instances: db.m5, db.r5, db.t3
- RDS instances can be single AZ or multi AZ (active-passive failover)
- When an instance is provisioned, it will have a dedicated storage allocated as well (usually EBS)
- Storage allocated can be based on SSD storage (IO1, GP2) or magnetic (mainly for compatibility)
- Billing for RDS:
    - We are billed based on instance size on a hourly rate
    - We are also billed per storage (GB/month) + extra per iops in case of provisioned iops (IO1)

## RDS Multi AZ

- Used to add resilience to an RDS instance
- Multi AZ is an option which can be enabled on an RDS instance, when enabled secondary hardware is allocated in another AZ (standby replica)
- RDS enables synchronous replication between primary and standby instances
- RDS is accessed via provided endpoint address (CNAME)
- With a single instance the endpoint address points the instance itself, with multi AZ, by default the endpoint points to the primary instance
- We can not directly access the standby instance
- If an error occurs with the primary instance, RDS automatically changes the endpoint to point to the standby replica. This failover occurs in around 60-120 seconds
- Multi AZ is not available in the Free-tier (generally costs double as it would the single AZ)
- Backups are taken from the standby instance (removes performance impact)
- Failovers can happen if:
    - AZ outage
    - Primary instance failure
    - Manual failover
    - Instance type change
    - Software patching
