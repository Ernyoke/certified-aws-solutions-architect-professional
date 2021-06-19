# DynamoDB

- NoSQL, wide column, DB-as-service product
- DynamoDB can handle key/value data or document data
- It requires no self-managed servers of infrastructure to be managed
- Supports a range of scaling options:
    - Manual/automatic provisioned performance IN/OUT
    - On-Demand mode
- DynamoDB is highly resilient across AZs and optionally globally
- DynamoDB is really fast, provides single-digit millisecond data retrieval
- Provides automatic backups, point-in-time recovery and encryption at rest
- Supports event-driven integration, provides actions when data is modified inside a table

## DynamoDB Tables

- A table in DynamoDB is a grouping of items with the same primary key
- Primary key can be a simple primary key (Partition Key - PK) or composite primary key (Partition Key + Sort Key - SK)
- In a table there are no limits to the number of items
- In case of composite keys, the combination of PK and SK should be unique
- Items can have, besides primary key, other data named attributes
- Every item can be different as long as it has the same primary key
- An item can be at max 400 KB
- DynamoDB can be configured with provisioned and on-demand capacity (capacity = speed)
- For on-demand capacity, we have to set:
    - Wite-Capacity Units (WCU): 1 WCU = 1KB per second
    - Read-Capacity Units (RCU): 1 RCU = 4KB per second

## DynamoDB Backups

- On-demand backups:
    - Full copy of the table is retained until the backup is removed
    - On-demand backups can be used restore data and config to same region or cross-region
    - We can retain or remove indexes
    - We can adjust encryption settings
- Point-in-time Recovery:
    - Not enabled by default, has to be enabled
    - It is a continuous record of changes
    - Allows replay to any point in the window (35 days recovery window)
    - From this 35 day window we can restore to another table with a 1 second granularity