
Leader-follower, multi-leader, leaderless replication 
sync vs. async
failover

Q. Why replicate ?
- Durability
- Availability
- Read scaling
- Geo-latency

These four goals pull in different directions. Durability wants synchronous writes to at least one replica. Read scaling wants many async replicas. Geo-latency wants replicas far away, which makes synchronous writes expensive. Every replication design is a weighted combination of these four

Topologies
- Single-leader
	- One master node for writes
	- Multiple followers for reading changes
	- PostgreSQL WAL, MySQL bin log, MongoDB oplog
- Multi-leader
	- two or more nodes accept writes and replicate to peers
	- DynamoDB Global tables in MREC mode
	- Conflict resolution. almost always last-write-wins LWW which silently discards the "loser" when two regions write the same key concurrently
- Leaderless
	- No designated primary
	- client or coordinator fans writes to N replicas and waits for W acknowledgements
	- Reads fan to R replicas and take the freshest
- Consensus-based
	- Raft, Paxos is treated as single-leader but the safety guarantees differ
	- Every write is committed only after a majority of replicas persist it
	- No data loss on single node failure
	- Automatic leader election
	- etcd, CockroachDB, Spanner and TiKV all use this model
- Chain replication
	- Arranges replicas in a linear chain
	- Writes enter at the head and flow tail-ward

Sync vs. Async vs. Semi-sync
- Async
	- Primary replies immediately, ships log later
	- RPO seconds to minutes
	- Lowest write latency 
	- Ex. Postgres default, MySQL default
- Semi-sync
	- Primary waits up to timeout T for the replica to log the events, then reverts to async
	- RPO 0 if replica acks, else seconds
	- +1 RTT local
	- Github MySQL (500 ms timeout)
- Sync
	- Primary waits for standby confirmation with no timeout, commit can block indefinitely
	- 0 RPO
	- +1 RTT (possibly cross-region), unbounded on standby failure
	- Postgres synchronous_commit = on
- Quoram
	- Primary waits for W of N replicas
	- RPO 0 if W met
	- +1 RTT to W-th fastest
	- Aurora 4-of-6, Raft majority

Log based replication
- Physical log - WAL
- Binary log -binlog
- Oplog
- Logical replication

Consensus replication (Raft and Paxos)
- Raft decomposes consensus into three sub-problems: leader election, log replication and safety
- A leader accepts proposals, appends them to its log, sends `AppendEntries` RPCs to followers, and declares an entry committed when a majority have persisted it.

- The `R + W > N` rules gives read-your-writes within the coordinator's view of the quorum. It does NOT mean reads are linearizable. and it does NOT prevent silent data loss under clock skew with LWW. For linearizability you need consensus (Raft/Paxos) or a read  from the leader with a read-index check

Failover
- Detection
- Election
- Fencing
	- split brain
	- STONITH - shoot the other node in the head

Multi-region replication
- DynamoDB Global Tables (MREC)
	- Async cross-region with LWW conflict resolution
- DynamoDB Global Tables (MRSC)
	- Synchronous replication to at least one other region before the write returns, RPO of zero.
- Aurora Global Database
- Spanner

Trade-offs

|Approach|Pros|Cons|Best when|Our Pick|
|---|---|---|---|---|
|Single-leader, async|Lowest write latency, simple topology|Data loss on primary failure, read lag|Most OLTP workloads tolerant of seconds of data loss|**Default for most systems**|
|Single-leader, semi-sync|Near-zero RPO in common case|Write latency +1 local RTT, falls back to async under replica failure|Financial, regulated, or audit-critical writes|When RPO matters|
|Multi-leader (LWW)|Local writes in every region, survives full region loss|Silent write loss under clock skew, hard-to-debug conflicts|Multi-region with idempotent or commutative writes|Only with proven geo-latency need|
|Leaderless (Dynamo)|High availability, tunable per-request consistency|Eventual consistency, operational complexity (repair, compaction, gossip)|Write-heavy workloads tolerant of eventual consistency|Cassandra/DynamoDB workloads|
|Consensus (Raft/Paxos)|Strong consistency, automatic failover, no data loss|Consensus latency per write, cross-region expensive|Metadata stores, systems needing linearizability|etcd, Spanner, CockroachDB|

Common pitfalls
- Split-brain without fencing
- Serving reads from async replicas for read-your-writes flows
- Threating replication as backup
- Ignoring replication lag alerts
- Cross-region promotion without application readiness

Take aways

- Single-leader async is the default and solves most problems. Understand replication lag and you are 90% of the way there.
- Synchronous replication trades latency for durability, and unguarded Postgres sync commit can block a commit indefinitely if a synchronous standby crashes. MySQL semi-sync reverts to async on timeout instead, which is why semi-sync with a short timeout (500 ms, against a 10-second default) is the pragmatic choice within a datacenter.
- Multi-leader is deceptively attractive. Conflicts are always harder than they look, and LWW silently loses data.
- Leaderless (Dynamo-style) with tunable R/W gives you a knob to trade consistency for availability per query, but `R + W > N` is not linearizability.
- Automated failover without fencing is a split-brain waiting to happen. Always fence the old primary before promoting the new one.
- Replication is not a backup. A bad `DROP TABLE` replicates everywhere in seconds. You still need point-in-time restore.
- Aurora's 4-of-6 quorum tolerates an AZ failure plus one extra segment loss without losing data. That is the bar for cloud-native durability.

