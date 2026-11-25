
Partitioning
- Divides data into smaller segments within a single database instance to optimize query performance
Sharding
- Distributes data across multiple independent servers or database instances

Why shard ?
- Single database node has hard ceilings: CPU cores, disk IOPS, RAM for the buffer pool and connection slots
- Sharding removes these ceilings by distributing the keyspace across independent nodes. Each shard handles a fraction of the traffic, has its own buffer pool, its own VACUUM schedule, and its own connection budget. But it is a one-way door: you lose cross-shard joins, cross-shard transactions, and globally unique indexes. Make sure vertical scaling and read replicas are truly exhausted first.

Sharding strategies
- Range partitions
	- rows sorted by key are split at boundary values
	- good for range scans
	- not ideal for append-heavy workloads where last shard becomes a hot partition
	- do not choose timestamp as the distribution column
- Hash partitions
	- `shard = hash(key) % N`
	- Even distribution by construction
	- If N changes remaps every key its drawback
- Consistent hashing
	- maps key and nodes on a unit circle
	- each key is assigned to the first node clockwise
	- adding a new node steals only 1/N of keys instead of nearly all
	- virtual nodes
- Directory-based
	- External look up table maps keys to shards
	- directory is single point of failure
	- adds a network hop
- Geographic
	- Data partitioned by user region for GDPR data residency or latency
