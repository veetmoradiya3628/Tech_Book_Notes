
NoSQL Umbrella
- Key-value store
	- DynamoDB
	- Redis
	- Riak
- Document
	- MongoDB
	- Couchbase
	- Firestore
- Wide-column
	- Cassandra
	- ScayllaDB
	- Bigtable / HBase
- Graph
	- Neo4j
	- Neptune
	- JanusGraph

Key-value store
- Its hashmap
- O(1) lookup
- Opaque key and value
- DynamoDB adds a sort key and optional secondary indexes
- Its useful for session stores, shopping carts, feature flags, caches, leaderboards. for any workload where you always know the key at query time

Document stores
- A document store holds self-describing JSON (or BSON) documents identified by `_id` with indexes on arbitrary fields
- content catalogs, product listings, user profiles with variable schemas, early-stage products where iteration speed matters more than reporting

Column-family stores
- A row is identified by a partition key plus clustering columns. Rows within a partition are stored together and sorted by the clustering key. The engine is an LSM-tree: writes append to a commit log plus memtable and flush to immutable SSTables that are compacted later.
- Query first schemas - you design one table per read pattern
- For a chat app fetching recent messages by channel
- Useful for time-series data, write-heavy workloads, multi-region deployments,  messaging at Discord/Netflix scale

Graph databases
- Nodes and typed edges with property maps, stored so each node keeps direct pointers to its edges (index-free adjacency). A traversal of k hops costs O(d^k) with a small constant, not a join-per-hop cost.
- Neo4j, Meta TAO
- Useful for social graphs, fraud detection, knowledge graphs, recommendation path queries, not for general OLTP

NewSQL hybrids
- NewSQL systems expose SQL and ACID but shard and replicas like NoSQL, it exists for teams that want NoSQL's scale with SQL's correctness
- Google spanner
- CockroachDB
- TiDB
- 