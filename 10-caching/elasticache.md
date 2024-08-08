# ElastiCache

- It is an in-memory database for application which need high-end performance
- It is orders of magnitude faster than a classic DB, but is not persistence
- ElastiCache provides 2 different engines: Managed Redis and MemcacheD as a service
- ElastiCache can be used for read heavy workloads with low latency requirements
- Can be used for reduction of database workloads, by this reducing cost accumulated by heavy database usage
- Can be used to store session date, making stateful applications stateless
- Using ElastiCache requires application code changes!

## Redis vs MemcacheD

- Both offer sub-millisecond access to data
- MemcacheD supports simple data structures (string), while Redis can support more advanced type of data: lists, sets, sorted sets, hashes, bit arrays, etc.
- Redis supports replication of data across multiple AZs, MemcacheD supports multiple nodes with manual sharding, but it does not supports "true" replication across AZs for scalability reasons
- Redis supports backups and restores, MemcacheD does not support persistance
- MemcacheD is multi-threaded by design, can take better advantage of multithreaded CPUs, can offer better performance
- Redis supports transactions (multiple operations at once)
- Both of these engines can support a ranges of instance types