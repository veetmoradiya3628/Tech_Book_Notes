
Vertical vs horizontal scaling, stateless services, read vs write scaling, and when scaling is the wrong answer. 

- vertical - bigger machine
- horizontal - more machines

Vertical vs. Horizontal scaling
- Vertical scale - scale up - means running your workload on a more powerful machine
- Horizontal scale - scale out - distributing the workload across more machines in parallel
- Ceiling hits in three forms
	- Physical limits - 1920vCPU is largest instance available
	- Blast radius - One big machine dies takes 100% traffic, ten small machines that lose one take 10%
	- Deploy risk - Restarting a vertical giant is scary. Restarting one of a hundred small replicas is routine.
- Tip - Start vertical, go horizontal when you must. Premature horizontal scaling is one of the most common and most expensive mistakes in early-stage systems.

Stateless vs. Stateful
- A **stateless** service holds no per-client information between requests. Any replica can handle any request.
- A **stateful** service remembers something about the client or owns a partition of data.
- In k8s this split is encoded in two workload resources. `Deployment` treats pods as interchangeable (stateless). `StatefulSet` gives each pod a stable network identity and a persistent volume that follows it across rescheduling, which is what database, brokers and distributed KV stores need.
- Making a service stateless is usually matter of moving state out:
	- HTTP sessions go into Redis or a signed cookie (JWT)
	- In-memory cache become shared caches (Redis, Memcached)
	- Background job state moves into a queue (SQS, Kafka) or a database
	- File uploads go to object storage (S3) instead of local disk
- make the default path stateless. Isolate the unavoidably stateful components (databases, brokers, gateways) into a small number of well-named services you can reason about carefully.

Reading scaling vs. Write scaling
- Most systems are read-heavy, read outnumber by 10x to 10,000x.
- Read scaling
	- its straightforward because reads are idempotent and can be cached or replicated
	- Replicas
	- Caches
	- CDNs
	- CQRS
- Write scaling
	- its hard because writes must land somewhere authoritative
	- Sharing
	- Batching
	- LSM-tree storage
- Read replicas are eventually consistent with the primary, usually milliseconds behind. A user who reads back their own write on a lagging replica may not see it. the fix is to pin writes to the primary for a few seconds, then fall back to replicas.

The scaling roadmap
- Single box
- Add a cache
- Add read replicas
- Add a CDN
- Vertical partitioning
- Horizontal scaling

When scaling is the wrong answer
- Fix the query
- Fix the algorithm
- Fix the access pattern
- Shed load
- Question the cloud bill
Lesson: exhaust vertical scaling, then vertical partitioning, then cache, before you shard. Every stage is cheaper and lower-risk than the next.

Trade-offs

|Lever|Gains|Costs|When to reach for it|
|---|---|---|---|
|Fix the bottleneck (profile first)|Often free; multiplies every later lever|Needs profiling skill; sometimes not enough alone|Always first, before any infrastructure change|
|Vertical scaling|Zero code changes, simple ops, fastest relief|Fixed ceiling (1,920 vCPUs on AWS U7inh), one failure domain, per-vCPU price premium at the top|Default first infrastructure move; stateful DBs; short-term relief|
|Caching (Redis/Memcached)|Orders-of-magnitude latency drop; absorbs hot keys|Invalidation is hard; staleness windows; stampede risk|Any read-heavy path with repeated queries or idempotent reads|
|Read replicas|Linear read scaling; geo-locality|Replication lag; adds no write capacity|Read-heavy workload tolerant of seconds-old data; after cache|
|CDN / edge caching|Latency close to users globally; absorbs static traffic|Cache-invalidation races; TLS termination at edge|Any public-facing system with static or edge-cacheable assets|
|Horizontal scaling (stateless tier)|No ceiling; cheap per unit; fault tolerant|Requires stateless design; network hop per request; distributed failure modes|Stateless app tier once a single box is saturated|
|Vertical partitioning|Easy, incremental, reversible|Finite (smallest unit is a table); does not help a single hot table|Moderate growth where the schema splits naturally into table groups; before sharding|
|Sharding|Linear write scaling; isolated hot partitions|Cross-shard queries painful; rebalance complex; months of engineering|Write-heavy workload where the dataset exceeds single-node limits|
Common pitfalls
- Adding replicas to fix a write bottleneck
- Sticky sessions by default
- Premature sharding
- Horizontal scaling a stateful service without leader election (its not good idea)
- Capacity planning for peak only

