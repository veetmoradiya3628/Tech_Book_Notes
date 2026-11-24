
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

Trade-offs

| Approach             | Pros                                                                                   | Cons                                                                                    | Best when                                                       | Our Pick                                                      |
| -------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------- |
| DynamoDB             | Serverless, predictable single-digit ms latency, 151M req/sec proven at Prime Day 2025 | AWS lock-in, modeling discipline required, GSI eventually consistent                    | AWS-native, known access patterns, need zero-ops                | When you are on AWS and can model access patterns upfront     |
| Cassandra / ScyllaDB | Linear write scale, tunable consistency, multi-DC native                               | Operational complexity, no joins, query-first modeling, compaction debt                 | Time-series, write-heavy, multi-region at Discord/Netflix scale | When you need multi-region writes and can invest in ops       |
| MongoDB              | Flexible schema during iteration, rich queries, wide driver ecosystem                  | Weak defaults (99.6% use unsafe readConcern), sharding pain, Jepsen anomalies           | Content catalogs, variable schemas, early-stage products        | Only if you override defaults and accept schema-in-code       |
| Neo4j / graph DBs    | Multi-hop traversals in O(pointer chases), expressive Cypher                           | Only one primary is elected writer per database at a time; secondaries scale reads only | Social graphs, fraud rings, knowledge graphs                    | When traversal depth > 3 hops and SQL joins are provably slow |
| Postgres JSONB       | One system, full ACID, SQL joins, GIN indexes                                          | Not horizontally scaled without sharding middleware                                     | When "document" needs are modest and you want boring            | **The default. Start here.**                                  |
| Distributed SQL      | SQL + horizontal scale + ACID; Spanner gives strict serializability                    | Newer operational scar tissue, latency tax, innovation-token cost                       | Global OLTP needing SQL, unwilling to run OLAP/OLTP split       | Only when you truly need multi-region strong consistency      |

Common pitfalls
- Picking nosql for the wrong reason
- Trusting mongodb default
- Underestimating operational burden
- "Schemaless" means schema in application code
- Expecting joins in Cassandra or DynamoDB
- Denormalizing without pricing the loss of integrity
- Ignoring Jepsen results

Key Takeaways
- Default to Postgres. Reach for NoSQL only when you can state the access pattern or scale wall in a specific number.
- The four NoSQL families (key-value, document, wide-column, graph) solve different problems. Picking the wrong family is worse than staying on SQL.
- Wide-column stores like Cassandra shine for write-heavy, time-series, multi-region workloads but are terrible at ad-hoc queries and joins.
- DynamoDB rewards disciplined single-table modeling and punishes lazy access-pattern design. It proved 89.2M req/sec at Prime Day 2021 and 151M req/sec at Prime Day 2025.
- MongoDB's read-concern default is still unsafe. The implicit write-concern default became `w: majority` in 5.0, but `readConcern: local` still returns uncommitted data, so override it or accept dirty reads that can vanish on failover.
- NewSQL (Spanner, CockroachDB) gives you SQL + horizontal scale, but costs an innovation token and a latency tax.
- Model for your reads, not your writes. Every NoSQL family forces this discipline; the ones that succeed embrace it early.