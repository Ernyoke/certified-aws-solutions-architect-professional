# ElastiCache

- It is an in-memory database for application which need high-end performance
- It is orders of magnitude faster than a classic DB, but is not persistence
- ElastiCache provides 2 types of databases: Managed Redis and Memcached as a service
- ElastiCache can be used for read heavy workloads with low latency requirements
- Reduces database workloads, by this reducing cost accumulated by heavy database usage
- Can be used to store session date, making stateful applications stateless
- Using ElastiCache requires application code changes!

## Redis vs Memcached

- Both offer sub-millisecond access to data
- Memcached supports simple data structures (string), while Redis can support more advanced type of data: lists, sets, sorted sets, hashes, bit arrays, etc.
- Redis supports replication of data across multiple AZs, Memcached supports multiple nodes with manual sharding, but it does not supports "true" replication across AZs
- Redis supports backups and restores, Memcached does not support persistance
- Memcached is multi-threaded by design, can offer better performance
- Redis supports transactions (multiple operations at once)
- Both of these engines can support a ranges of instance types