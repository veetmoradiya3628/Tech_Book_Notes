
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

