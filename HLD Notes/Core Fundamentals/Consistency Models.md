
- Strong, eventual and casual consistency
- Read-your-writes, monotonic reads, monotonic writes,  write-follow reads
- Consistency is an reader side property and not writer side property

- Consistency
	- ACID-C - about the schema and its consistency at application level
	- CAP-C - replicas agreeing on recent writes and linearizability
	- Replica consistency - sequential, causal and eventual consistency
- Linearizability
	- every operation appears to take effect atomically at some point between its invocation and response, in a total order consistent with real-time
	- If operation A completes before B begins, every observer sees A before B. The system behaves like a single machine.
	- quorum coordination on every write, at least one cross-region round trip for global linearizability, and unavailability during partitions
- Sequential consistency
- Causal consistency
	- operations that are causally related are seen in the same order at every replica.
- Eventual consistency
	- if writes stop, all replicas eventually coverage.
	- no bound on how long eventually takes
	- readers can see stale data, out-of-order updates, and divergent views
- Session guarantees
	- Read-your-writes (RYW)
	- Monotonic reads
	- Monotonic writes
	- Writes-follow-reads

- ACID isolation levels
	- Isolation levels are orthogonal to replica consistency but often confused with it. They describe what concurrent transactions within a single database see of each other's partial work.
		- Read uncommitted - dirty reads allowed
		- Read committed - Postgres default
		- Snapshot isolation - Postgres Repeatable Read
		- Serializable 
		- Strict Serializable
	- test your isolation guarntees
- Eventual consistency and conflict resolution
	- When replicas accept writes independently conflicts are inevitable
	- Three resolution strategies
		- Last-write-wins
		- Vector clocks
		- CRDTs 
- Cost of linearizability
	- Google spanner's truetime
	- Raft ReadIndex
	- Lease reads
- Google spanner - external consistency across continents

| Model                             | Latency                      | Availability under partition | Reasoning complexity              | Best when                     | Our Pick                           |
| --------------------------------- | ---------------------------- | ---------------------------- | --------------------------------- | ----------------------------- | ---------------------------------- |
| Linearizable                      | High (coordination every op) | Low (CP)                     | Simple (single machine)           | Bank ledger, inventory, locks | When correctness is non-negotiable |
| Causal                            | Moderate (metadata tracking) | High (totally available)     | Moderate (cause/effect)           | Social feeds, chat, CRDTs     | Default for human-facing apps      |
| Session (eventual + 4 guarantees) | Low                          | Very high                    | Simple per-user, complex globally | Most user-facing apps         | When you need UX sanity cheaply    |
| Eventual                          | Lowest                       | Highest (AP)                 | Hardest to reason about           | Counters, analytics, DNS      | When staleness is truly acceptable |
| Bounded staleness                 | Low-moderate                 | High                         | Moderate (quantifiable SLA)       | Dashboards, regulatory reads  | When you need a recency SLA        |
Common pitfalls
- Calling a system "CP" or "AP" without nuance
- Snapshot isolation is not serializable
- LWW silently loses writes under clock skew
- Eventually consistent with no bound
- Causal sessions require majority concerns

