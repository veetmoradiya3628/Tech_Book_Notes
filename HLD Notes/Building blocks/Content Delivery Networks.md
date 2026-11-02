
A CDN is a globally distributed cache and security front door. It terminates TLS near the user, serves cacheable content from the edge, and forwards only misses to your origin.

Cloudflare operates in 330+ cities with 500 Tbps of capacity.

Well-tuned CDN achieves 90 to 99% cache hit ratio, meaning your origin handles 1 to 10% of traffic.

Push aggressively to edge; 95%+ hit rate is the bar for static assets

Why CDN exist
- The speed of light
- Origin offload
- DDoS absorption

BGP Anycast vs DNS-based GSLB - Global Server Load Balancing
- Anycast has mostly won for new deployments, it gives automatic failover, simpler configuration, and no DNS propagation delay. GSLB remains relevant for fine-grained geo-steering and compliance routing

Cache Keys and hit rates
- The golden pattern for static assets
- The Vary trap
- Revalidation
- Target hit rates

Purge and invalidation
- URL purge
- Tag-based purge (surrogate keys)
- Soft purge
- Wild card/prefix purge

Edge compute
- Cloudflare Workers
- Fastly Compute
  AWS Lambda @ Edge

Streaming at the edge

Security at the edge
- WAF rules
- DDoS mitigation
- Bot management

Common pitfalls
- Cache key poisoning via `Vary`
- Forgetting `Vary: Accept-Encoding`
- Over-aggressive TTLs breaking invalidation
- Ignoring origin egress costs
- Not using Origin shield
- Going to production without a CDN
- Single-CDN blast radius

Key takeaways

- A CDN is a cache first, a compute platform second. Cache hit ratio is the metric that matters. Target 95%+ for static assets.
- The speed-of-light argument is not rhetorical: 200 ms NYC-to-Sydney RTT drops to under 20 ms to the nearest PoP. This is the largest latency win available without code changes.
- Long TTLs plus hash-busting URLs (`app.a3f2b1.js` with `max-age=31536000, immutable`) are the highest-leverage CDN pattern ever invented
- Stale-while-revalidate hides origin failures and latency. It is nearly free to enable and should be on every cacheable response
- Tag-based purge (surrogate keys) lets you cache aggressively with long TTLs and still invalidate in 150 ms globally.
- Edge compute is a request-shaping layer, not a general runtime. Workers caps CPU at 10 ms (free) or up to 5 minutes (paid, default 30 seconds) and memory at 128 MB per isolate. Use it for auth, A/B splits, and personalization stamps.
- Your CDN is a single point of failure. The 2021 Fastly outage took down half the internet for an hour. Multi-CDN is a resilience pattern for any tier-1 product.

Hands on
1. Write a go server with two endpoints (1 static, 1 dynamic), cache it using AWS CloudFront or any other service and simulate few use cases with cache hit, cache miss etc different use cases