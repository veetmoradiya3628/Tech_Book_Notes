ACID
- Atomicity - all or nothing
- Consistency - constraints (foreign keys, check, unique) holds after every commit
- Isolation - concurrent transactions do not see each other's partial work
- Durability - committed data survives crashes

- Atomicity and durability share one mechanism which is WAL - write-ahead log
	- Every change is appended to the WAL and fsynced to disk before the transaction is told "commit" OK.
	- The actual data pages are updated later at a checkpoint time.
- fsync with group commit needs to be used as its expensive operation to 1 to 10 ms on NVme

|Level|Dirty Read|Non-Repeatable Read|Phantom|Write Skew|
|---|---|---|---|---|
|Read Uncommitted|Yes|Yes|Yes|Yes|
|Read Committed|No|Yes|Yes|Yes|
|Repeatable Read (Snapshot)|No|No|Postgres: No|Yes|
|Serializable|No|No|No|No|
- Postgres defaults to Read commited
- Postgres serializable

Indexes - B-tree, composite, covering
- An index is an auxiliary data structure that converts full-table scans into O(logn) lookups. Postgres defaults to B-tree
- A B-tree with fan-out in the hundreds keeps billions of rows within 3 to 5 levels; double-linked leaf pages support efficient range scans

- composite index ordering
	- an index on (a, b) is sorted by a first, then b. queries filtering on a along or on a AND b use the index. queries filtering on b alone cannot.
- converting index
	- Index only scan
- partial indexes
- cost of indexes
	- every index must be updated on every insert and on updates that touch indexed columns.

- Use UUIDv7 (time-ordered) instead of UUIDv4 (random) for primary keys on high-write tables.

Query Execution and EXPLAIN
- A SQL string passes through three stages: parser (builds an AST), planner (picks the cheapest plan using table statistics from pg_statistic) and executor (runs the plan)
- The planner picks Seq Scan for low-selectivity predicates and Index Scan for high-selectivity ones; Bitmap Heap Scan handles the middle ground by sorting random index hits into sequential heap order.
- EXPLAIN ANALYZE allows us to see how query will be handled by actually running it against the data so destructive queries should go in BEGIN; ... ROLLBACK; block

MVCC
- Readers never block writers
- Multi-version concurrency control keeps multiple versions of each row so readers see a consistent snapshot without locking writers.
- PostgreSQL implements this using xmin and xmax on transactions

- VACCUM cost
	- dead tuples accumulate until VACCUM reclaims them. if VACUUM falls behind, tables and indexes bloat indefinitely.

Normalization vs. denormalization
- **Normalization** stores every fact once, in the table where the key for that fact lives. A 3NF schema minimizes write anomalies and storage.
- **Denormalization** duplicates facts so reads skip joins. A social feed is the classic example: on post creation, the post ID fans out into each follower's precomputed timeline in Redis, replacing a `JOIN posts ON followers` at read time with an O(1) list lookup.

|Approach|Writes|Reads|Best when|
|---|---|---|---|
|Normalized (3NF)|Small, clean, one place to update|Requires joins|OLTP, mutating data, write-heavy|
|Denormalized|Write amplification (fanout)|O(1), no joins, cacheable|Feeds, dashboards, read:write > 100:1|
- Normalize for your source of truth. Denormalize for your read path. Connect them with a replication pipe (CDC, event stream).

Connection pooling
- Postgres uses a process-per-connection model. Each backend costs 5 to 10 MB of resident memory.
- default max_connections is 100
- PgBouncer multiplexes thousands of application connections onto a small pool of real Postgres backends.
- transaction pooling mode, a backend is assigned for the duration of one transaction and returned to the pool immediately after. this is the production default for high-concurrency web apps
- **Transaction pooling breaks session state.** Session-scoped `SET`, advisory locks across statements, `WITH HOLD` cursors, and session-level prepared statements all break under transaction pooling. Design your application to be stateless between transactions.

Design decisions
- Normalization vs. Denormalization
- whether to add an index
- which isolation level
- whether an index should be covering

Common pitfalls
- N+1 query problem with ORMs
- Over-indexing
- Write skew at snapshot isolation
- Connection exhaustion without pooling
- Long-running transactions causing MVCC bloat

Key takeaways
- ACID is four guarantees with concrete mechanisms: WAL for atomicity and durability, MVCC or locks for isolation, constraints for consistency.
- Every index speeds one query shape and slows every write. Measure with `EXPLAIN ANALYZE` before adding one.
- Postgres defaults to Read Committed. Understand write skew before choosing weaker or stronger isolation.
- MVCC gives non-blocking reads but requires VACUUM to reclaim dead tuples. Long transactions are the enemy.
- Normalize for your source of truth, denormalize for your read path. Connect them with replication.
- Connection pooling (PgBouncer in transaction mode) is mandatory for production Postgres. The default 100 connections is not enough for modern web apps.
- Use UUIDv7 over UUIDv4 for primary keys on write-heavy tables to preserve B-tree locality.