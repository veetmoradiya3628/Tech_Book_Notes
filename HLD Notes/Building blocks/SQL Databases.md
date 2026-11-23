
ACID internals
- WAL - Write Ahead Log
	- Every change appends to a redo record to a WAL and calls fsync before returning COMMIT OK
	- 16MB by default
- MVCC - multi-version concurrency control
	- Postgres stores old row versions in-place in the heap. Each tuple carries xmin (creating transaction ID) and xmax (deleting / updating transaction ID) 
	- A visibility check per query decides which versions this snapshot can see
	- Dead tuple accumulate until VACCUM reclaims them

Isolation levels

| Level                                  | Anomalies prevented              | Mechanism                                           |
| -------------------------------------- | -------------------------------- | --------------------------------------------------- |
| Read Committed (default)               | Dirty reads                      | Fresh snapshot per statement                        |
| Repeatable Read (= Snapshot Isolation) | + non-repeatable reads, phantoms | Single snapshot for entire transaction              |
| Serializable (= SSI)                   | + write skew                     | Runtime cycle detection, aborts with SQLSTATE 40001 |
Indexes
- An index is an auxiliary data structure that converts O(n) scans into O(log n) lookups.
- B-tree
- Hash
- GIN
- GiST
- SP-GiST
- BRIN
- Bloom (extension)

Composite indexes
Covering indexes

Three to six indexes per hot OLTP table is typical, past that writes suffer measurably
- A table with 12 indexes writes 13 physical records per INSERT each WAL-logged

Query optimizer
- Three join algorithms
	- Nestead-loop
	- Hash join
	- Merge join
- Common traps
	- stale statistics
	- random_page_cost
	- parameter sniffing
	- pg_stat_statements - table must be enabled in  production db

Replication
- Postgres streaming replication
- MySQL default to async 

For Postgres deployment
- PgBouncer - absorbs connection storms 
- replicas scale reads 
- WAL archiving gives point-in-time recovery

Postgres superpowers
- JSONB with GIN indexes
- Full text search - tsvector + GIN index + ts_rank_cd
- PostGIS
- Logical decoding
- Extensions - TimescaleDB, pgvector, foreign data wrappers

Scaling limits
- Vertical scaling goes further than engineers expects
- Connection pooling is mandatory

When to leave SQL
- Truly Key-Value workloads at millions ops/sec - use Redis or DynamoDB
- Massive write-heavy time-series with eventual consistency tolerance
- Feed fanout at instagram scale 
- Global strong consistency without operational sharding

| Approach              | Pros                                                                           | Cons                                                              | Best when                                                  | Our Pick                                          |
| --------------------- | ------------------------------------------------------------------------------ | ----------------------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------- |
| PostgreSQL            | Rich features (GIN, JSONB, PostGIS, extensions), strict MVCC, strong optimizer | VACUUM complexity, process-per-connection                         | Default for new systems                                    | **Yes, unless you have a specific reason not to** |
| MySQL / InnoDB        | Ubiquitous, Aurora + Vitess + ProxySQL, lighter per-connection cost            | Weaker defaults (RR gap locks), fewer data types                  | Existing estates, high-write OLTP, Rails at scale          | When your team already knows it                   |
| Amazon Aurora         | Decoupled storage, fast failover, up to 5x MySQL throughput                    | AWS lock-in, higher per-GB cost, commit latency floor from quorum | You are on AWS and need managed scale                      | When operational simplicity outweighs cost        |
| CockroachDB / Spanner | Horizontal SQL, strong consistency across regions                              | Per-write consensus RTT that single-region Postgres does not pay  | Global transactions where sharding ops cost > latency cost | Only when you truly need multi-region writes      |

Common pitfalls
- Missing FK indexes
- N+1 queries
- Stale statistics
- Parameter sniffing
- Unbounded connections
- Forgetting VACUUM

Key TakeAways

- PostgreSQL or MySQL solves 99% of production OLTP problems. Pick one and get deep.
- Every index speeds one query shape and slows every write. Three to six per hot table is the sweet spot.
- Vertical scaling on a single primary goes further than engineers expect: 176 vCPUs, 1.4 TB RAM, 200K writes/sec on an Aurora r4.16xlarge.
- Connection pooling (PgBouncer in transaction mode) is mandatory. The default 100 connections is not enough.
- VACUUM is not optional. Long-running transactions are the enemy of a healthy Postgres.
- Isolation levels are a contract with your code. Use Read Committed for most workloads, Serializable for money.
- Shard only when you must. Notion ran four orders of magnitude of growth on one instance before sharding.

