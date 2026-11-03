A cache trades memory and a bounded risk of staleness for a 10x to 1000x latency win.
Every layer of a modern stack has one, from the browser's HTTP cache to the database buffer pool.

The cache hierarchy
- Browser cache - private, per-user
- CDN Edge - Cloudflare, Fastly
- Reverse proxy - Varnish, Nginx
- App-level cache - Caffeine, Guava
- Distributed cache - Redis, memcached
- Database - buffer pool
- Disk

Every layer from browser to buffer pool is a cache; a request that hits at any layer short circuits the rest
Each layer has its own TTL and consistency model. Long TTLs at the browser for immutable asserts, short TTLs at the app layer for user-specific state.

Caching Patterns
- Cache-aside - Lazy Loading - The application reads the cache first. On miss, it loads from the database, populates the cache, and returns. On write, it updates the database and invalidates the cache key.
- Read-through - The cache library loads from the database on miss. The application calls only `get`. DynamoDB DAX works this way. 
- Write-through - Every write goes to cache and database synchronously. Guarantees read-your-writes consistency, but doubles write latency. Use it for low-write workloads where freshness matters more than speed.
- Write-behind (write-back) - Write to cache first, flush to the database asynchronously. Fast writes, batched database load. The risk: data loss on crash between cache write and database flush.
- Refresh-ahead (stale-while-revalidate) - The cache detects a key approaching its TTL and refreshes it in the background before expiry. HTTP's `stale-while-revalidate` directive (RFC 5861) is the standard version for edge caches. Readers never wait, but they may see bounded stale data.

Eviction policies
- LRU - Least recently used
- LFU - Least frequently used
- TinyLFU
- W-TinyLFU

TTL and statleness
- Absolute TTL
- Sliding TTL
- Jittered TTL
- Stale-while-revalidate
- XFetch

Cache invalidation
- Write-invalidate (delete on write)
- Double-delete
- Versioned keys
- Pub/sub invalidation
- Leases

Distributed cache architectures
- Client-side sharding
- Proxy-based routing
- Redis cluster
- Cross-region replication

Trade offs

|Approach|Pros|Cons|Best when|Our Pick|
|---|---|---|---|---|
|Cache-aside|Simple, failure-tolerant, works with any KV store|Stale reads on race, explicit invalidation needed|Default for most apps|Default choice|
|Read-through|Transparent to app, centralized load logic|Cache outage = origin outage, couples to DB schema|Library-managed caches (Caffeine, DAX)|When using Caffeine or DAX|
|Write-through|Strong read-your-writes consistency|Write latency doubles, cache must be online|Low-write workloads needing freshness|Rarely; only for config/metadata|
|Write-behind|Fast writes, batched DB load, absorbs spikes|Durability risk on crash, complex recovery|Counters, metrics, Discord-style read states|High-write, loss-tolerant workloads|
|Refresh-ahead|Hides DB latency for hot keys, reader never waits|Wastes work if key goes cold, bounded staleness|Predictable hot sets, CDN edge|HTTP stale-while-revalidate at CDN|

Common pitfalls
- Cache stampede (thundering herd)
- Negative cache poisoning
- Stale data after failover
- Double-write inconsistency
- Hot-key saturation

Key takeaways

- Caching is the single highest-leverage optimization in most systems. A 50 ms database call becomes a 0.3 ms cache hit. Measure hit ratio before anything else.
- Cache-aside is the safe default. It tolerates cache unavailability and works with any key-value store. Reach for write-through only when you need strong read-your-writes.
- Stampedes are inevitable at scale. Jittered TTLs plus single-flight coalescing are not optional. At Facebook scale, leases cap recomputation to one per key per 10 seconds
- Invalidation is genuinely hard because it is a distributed consistency problem. Prefer short TTLs and versioned keys over clever invalidation schemes.
- W-TinyLFU (Caffeine's default) significantly outperforms LRU on real-world traces. If you are on the JVM, use Caffeine. On Redis, choose `allkeys-lfu` for frequency-skewed workloads.
- A service that cannot operate with a cold cache has become addicted to its cache. Design for graceful degradation: the origin must survive a full cache restart.
- Every layer of the hierarchy has its own TTL and consistency model. Reason about staleness at each layer independently.