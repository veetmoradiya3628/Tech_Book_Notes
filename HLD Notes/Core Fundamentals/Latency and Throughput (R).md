
- Latency is how long one request takes.
- Throughput is how many requests per second you handle.
- parallelism
- tail latency amplification in fanout architecture

Latency vs. Throughput - where queue lives
- Latency - time per request, start to finish on one timeline. nanoseconds, microseconds, milliseconds, seconds etc
- Throughput - its rate of completed work. queries per second (QPS), requests per seconds (RPS), bytes per second

Percentiles, not averages
- The average is the first metric you compute and the last one you should trust. mean and standard deviation are useless at best, highly misleading at worst.
- correct tool is the percentiles
	- p50 median - half your requests are faster, half slower. A good health signal.
	- p95 - catches common slow cases (GC minor pauses, cache misses)
	- p99 - the metric your power users feel. They make enough requests to hit the 1-in-100 slow path regularly.
	- p99.9 - GC major pauses, failovers, timeouts, and cold caches live here

Coordinated omission
- Coordinated omission (CO) is a measurement error where a load generator fails to send requests during a system stall, biasing percentiles toward optimistic values by orders of magnitude
- Use load testers that measure from planned start time, not actual send time. `wrk2` is a fixed version of `wrk` that avoids CO at the source.
- Most load testers lie by default

Little's Law and queueing
- Little's law is most useful equation in capacity planning:
	`L = lambda * W`
	- L = average number of in-flight requests
	- lambda - arrival rate (requests per second)
	- W = average time each request spends in the system
- Applications
	- Thread pool sizing
		- 1000 QPS at 50ms latency means L = 50 concurrent requests. you need at least 50 workers, plus headroom for variance
	- Connection pool sizing
		- 500 QPS with 20ms DB latency needs L = 10 concurrent DB connections on average. Size the pool at 2 to 3x for spikes.
	- Incident narrative
		- Lambda spiked from retries, W rose from contention, L hit the pool limit, and requests queued unboundedly

Latency hierarchy

|Operation|Latency|Relative to L1|
|---|---|---|
|L1 cache reference|0.5 ns|1x|
|Mutex lock/unlock|25 ns|50x|
|Main memory reference|100 ns|200x|
|Compress 1 KB (Snappy)|2 us|4,000x|
|Send 1 KB over 1 Gbps NIC|10 us|20,000x|
|NVMe SSD random 4 KB read|16 us|32,000x|
|Read 1 MB sequentially from RAM|250 us|500,000x|
|Round trip within same datacenter|500 us|1,000,000x|
|Read 1 MB sequentially from SSD|1 ms|2,000,000x|
|HDD seek|10 ms|20,000,000x|
|Send packet CA to Netherlands to CA|150 ms|300,000,000x|

- Key rules
	- RAM is 100x faster than NVMe for random reads - This is why caching works
	- A datacenter round trip is 5000x slower than RAM access - Every network hop is enormous compared to local work
	- Transcontinental is 300x slower than intra-datacenter - Physics - light in fiber travels about 200,000 km/s. You cannot engineer around the speed of light.

Throughput patterns
- Batching
	- amortizes fixed per-call overhead (TCP handshake, syscall, crypto) across N items. Kafka's `batch.size` and database bulk inserts can raise throughput 10x to 100x, but per-request latency worsens by half the batch interval on average.
- Pipelining
	- overlaps request issuance with response processing, hiding RTT. HTTP/2 multiplexing and Redis pipelining exploit this.
- Zero-copy IO
	- (Linux `sendfile()`, `splice()`) avoids kernel-to-userspace memory copies for file-to-socket transfers. Nginx's `sendfile` directive lets a single server saturate multi-10 Gbps NICs with low CPU
- Amdahl's Law
	- caps parallelism gains: speedup = 1 / ((1-P) + P/N), where P is the parallelizable fraction and N is the number of processors. A 95% parallelizable workload (serial fraction = 0.05) is capped at 1/0.05 = 20x no matter how many cores you add
- Gunther's Universal Scalability Law (USL)

| Symptom                                    | Lever                                                | Latency impact                          | Throughput impact                   | Caveat                                                                     |
| ------------------------------------------ | ---------------------------------------------------- | --------------------------------------- | ----------------------------------- | -------------------------------------------------------------------------- |
| Not enough capacity at current rho         | Add more replicas / workers (size with Little's Law) | Neutral until USL coherency dominates   | Linear increase until Nmax          | Works only for stateless services; past Nmax more nodes hurt               |
| Throughput ceiling on writes / producers   | Batch requests                                       | Worse per-request (half batch interval) | 10x to 100x better                  | Raises per-call latency; unsuitable when the user is waiting synchronously |
| High p50 / p95 on hot reads                | Add a cache                                          | Much better p50/p95                     | Better (fewer backend hits)         | Does NOT compress the tail. GC pauses, compaction, SSD GC still spike      |
| High p99 / p99.9 on fanout reads           | Hedged or tied requests                              | p99.9 compression 10x to 25x            | Slightly worse (2 to 5% extra load) | Needs idempotent reads and a second replica able to serve                  |
| NIC saturation on file-to-socket transfers | Zero-copy (`sendfile`, `splice`)                     | Neutral                                 | Saturates NICs at low CPU           | File-to-socket paths only; irrelevant to dynamic payloads                  |

