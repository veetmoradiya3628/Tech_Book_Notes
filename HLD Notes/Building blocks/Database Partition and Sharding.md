
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

Choosing partition key
- partition key is the single most important design decision in a sharded system
	- High cardinality
	- Even distribution
	- stability
	- Present in hot-path queries
- The industry pattern for multi-tenant systems is a compound key: `(tenant_id, local_id)` where `tenant_id` partitions and `local_id` is unique inside the tenant. This gives a single unique index per shard that is globally unique without cross-shard coordination.

Resharding
- The dominant pattern is replication-based resharding
- set up new shards, stream data from old to new via logical replication, verify with dark reads, then flip the router.

Cross-shard queries
- When a query includes the shard key in WHERE, the router sends it to one shard. When it does not, the router must scatter the query to every shard and gather results.
- Scatter-gather latency is bounded by the slowest shard (tail amplification).
- Cross-shard joins
- Cross-shard transactions

Hot keys & multi-tenancy
- Request coalescing
- Compound partition keys
- Write sharding
- Per-key rate limiting

For multi-tenant SaaS, three canonical patterns exists
- silo
- pool
- bridge

Trade-offs

| Approach           | Pros                                                                         | Cons                                                                     | Best when                                                    | Our Pick                                     |
| ------------------ | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------------------- |
| Range partitioning | Efficient range scans; locality for ordered reads                            | Hot ranges on append-heavy workloads; skew near boundaries               | Time-series with explicit non-time bucketing                 | Only if range scans are on the critical path |
| Hash partitioning  | Even load by construction; simplest to reason about                          | No range queries on hashed column; `hash % N` moves most keys on reshard | Point lookups, evenly distributed writes, static shard count | **Default choice for most OLTP**             |
| Consistent hashing | Minimal data movement on rebalance (~1/N); tolerates dynamic membership      | Variance without virtual nodes; tuning complexity                        | Dynamic membership (caches, Dynamo-style clusters)           | When shard set changes frequently            |
| Directory-based    | Flexible per-tenant placement; supports moving a big tenant to its own shard | Directory lookup hop; directory is SPOF                                  | Multi-tenant SaaS with per-tenant scale outliers             | When you need per-tenant control             |

Common pitfalls
- timestamp as partition key
- Celebrity hot keys
- Scatter-gather on every query
- Cross-shard foreign key constraints
- Forgetting secondary index rebuilds
- Backfilling a partition key under load

Key take aways

- Sharding is a one-way door. Exhaust vertical scaling, read replicas, and vertical partitioning first.
- Pick the partition key for the worst access pattern, not the average one. High cardinality, even distribution, stable, present in hot-path queries.
- Hash-based routing is the default for OLTP. Use consistent hashing only when the shard set is dynamic.
- Pre-shard into many logical partitions (pick a highly divisible number like 480 or 360). You can grow physical capacity without re-bucketing data.
- Cross-shard joins and transactions are expensive or impossible. Colocate related data under the same shard key.
- Hot partitions will find you. Design so you can add request coalescing, write sharding, or split on short notice.
- Online resharding via logical replication, dark reads, and proxy-level pause-and-flip is the industry standard pattern.