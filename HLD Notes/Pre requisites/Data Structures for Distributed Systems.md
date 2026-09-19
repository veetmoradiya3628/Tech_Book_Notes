
```
hashtable for O(1) lookup
B-trees for range queries in read-heavy database
LSM-trees for write-heavy storage
Bloom filter for does it exist
skip lists for sorted sets
consistent hashing for shared placement
```

hash tables
- maps key to value in O(1) time
- bucket index, collisions resolution via chaining or open addressing
- load factors
- its backbone of 
	- Redis, Memcached, every language's dict or HashMap
	- shard routing
	- database indexes
	- hash joins
- Its useful when point lookups, partition routing, deduplication, any key-value access where you never need range scans.

B tree vs. LSM-tree
- On disk storage engines for database pickup
- B-tree 
	- A balanced tree where each node is a disk page holding hundreds of keys and child pointers.
	- Primarily used by PostgreSQL, MySQL and Oracle for indexes
- LSM-tree
	- Log-structured merge tree
	- writes append to an in-memory memtable (typically a skip list) plus a write-ahead log. Full memtables flush to disk as immutable Sorted String Tables (SSTables). Background compaction merges smaller SSTables into larger ones, organizing them into levels where each level is ~10x the previous.
	- Reads may check the memtable plus one file per level, so each SSTable carries a Bloom filter to skip absent keys.
- B-tree is useful for read-heavy OLTP, range scans, workloads where read latency matters more than write throughput
- LSM-tree is useful for write-heavy workloads, time-series data, append-heavy patterns, RocksDB, Cassandra, and ScyllaDB all use LSM storage.

Bloom filters
	- A Bloom filter answers "might this key exist?" using a fraction of the space a hash set would need. It never returns false negatives, but it does return false positives at a tunable rate.
	- K hash functions and m bit array
	- It does not support delete items use case
	- Useful when existence checks as a cheap negative-cache layer before expensive disk or network lookups.

Skip lists
	- A skip list is a sorted linked list augmented with probabilistic "express lanes." Each new node is assigned a random level; higher levels skip over more nodes, giving O(log n) expected search. No rotations, no rebalancing, much simpler concurrency than AVL or red-black trees.
	- Redis uses a skip list for sorted sets (ZADD, ZRANGE, ZRANK).
	- Level and sampling configuration with skip list DS
	- Useful when sorted operations with simpler concurrency than balanced trees. Redis leaderboards, in-memory sorted indexes, LSM memtables

Consistent Hashing
	- When we have N cache servers, Naive sharing with hash(key) % N remaps nearly every key when N changes, causing a thundering stampede of cache misses on the origin.
	-  place servers on a ring (0 to 2^32). Hash keys onto the same ring. Each key is owned by the next server clockwise. Adding or removing a server moves only ~1/N of keys, not all of them.
	- **Virtual nodes** solve the "unlucky server owns a huge arc" problem. Each physical server is represented by 100 to 300 points on the ring. This smooths load distribution and makes rebalancing incremental.
	- Each physical node owns multiple arcs via virtual nodes; a key maps to the next vnode clockwise and replicates to the next R distinct physical nodes.
	- Useful when sharing with minimal reshuffling on membership change. Dynamo-style databases, distributed caches, partition assignment in Kafka

| Engine   | Read                                                                         | Write                                                                          | Space overhead                                     | Best when                                                                                       | Our Pick                           |
| -------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | -------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ---------------------------------- |
| B-tree   | O(log n), one root-to-leaf path per lookup                                   | O(log n) with random in-place page writes                                      | O(n), modest per-page slack                        | Read-heavy OLTP with range scans where read latency matters more than write throughput          | PostgreSQL, MySQL/InnoDB workloads |
| LSM-tree | O(log n) worst case across memtable + levels; Bloom filters skip absent keys | O(1) amortized, sequential appends; compaction pays 10-30x write amplification | O(n) + compaction churn + per-SSTable Bloom filter | Write-heavy workloads, time-series, append patterns where sequential-write throughput dominates | Cassandra, RocksDB, ScyllaDB       |

**Hash table.** Use for point lookups where ordering is never needed (in-memory caches, shard-key routing, hash joins). Destroys ordering by design; never use it as a substitute for a B-tree when range predicates are required. Standard cost: O(1) average read/write, rehashing at ~0.75 load factor.

**Bloom filter.** Pair with any expensive "might it exist?" check in front of disk or network. Standard sizing is ~10 bits per item for 1% false-positive rate with 7 hash functions. Canonical pairings: LSM SSTables, CDN URL caches, Cassandra per-SSTable filters Cannot delete. Use a Cuckoo filter if you need deletion support

**Skip list.** Reach for sorted primitives that need simple concurrency (no rotations or rebalancing). Standard uses: Redis sorted sets (`ZADD`/`ZRANGE` with per-level span counts for O(log n) rank queries) and memtables inside LevelDB/RocksDB before SSTable flush

**Consistent hashing.** Use for sharding with elastic membership so adding or removing a server moves only ~1/N of keys instead of nearly all of them. Always pair with 100-300 virtual nodes per physical server to smooth load and enable incremental rebalancing. Canonical deployments: DynamoDB, Cassandra, Memcached clients.

Common Pitfalls
- Using a hash index for range queries, can not use hash index for between or range scan as hashing destroys ordering by design, the planner falls back to a full table scan. use a B-tree index for range predicates
- **Under-sizing a Bloom filter.** If you size for 100K items and get 10M, the false-positive rate explodes from 1% to effectively 50%. The filter stops filtering. Monitor actual item counts versus capacity. In LSM systems, each SSTable gets a fresh filter at compaction, so this self-heals, but application-level Bloom filters need manual resizing.
- **Naive modulo sharding.** `hash(key) % N` remaps nearly every key when N changes. The first time you add a shard, your cache hit rate collapses and the origin gets stampeded. Use consistent hashing with virtual nodes from day one if you plan to scale.
- Ignoring LSM compaction stalls.


