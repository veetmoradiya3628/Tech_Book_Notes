
A load balancer accepts a virtual IP, picks a backend, and quietly removes the ones that stop responding.
Layer 4 forwards based on 5 tuple at line rate vs Layer 7 terminates TCP, parses HTTP and routes on headers

Load balancer does three jobs - network traffic - distribution, health checking and termination

**Layer 4 vs. Layer 7**
- An L4 load balancer operates on TCP/UDP packets.
- It hashes the 5-tuple (source IP, source port, destination IP, destination port, protocol) into a backend slot and forwards the packet without ever reading the payload.
- Responses typically bypass the LB entirely via Direct Server Return (DSR), so the LB only touches ingress traffic

- An L7 load balancer terminates the TCP connection, decrypts TLS, parses HTTP (or gRPC, WebSocket, HTTP/2), and opens a new connection to the chosen backend.
- This lets it route by path, header, cookie, or method, retry failed requests, inject observability headers, and enforce per-tenant rate limits
- The cost is CPU. Every request parsed is a request the LB must buffer, decrypt, and re-serialize. L7 proxies top out an order of magnitude lower on requests per second than L4 forwarders on the same hardware.

- Use L4 when you need raw throughput, protocol-agnostic forwarding, or a DDoS absorption tier. Use L7 when you need path-based routing, retries, auth, or per-request metrics. Most production stacks use both: Cloudflare runs Unimog (L4, XDP) in front of Pingora (L7)
- AWS customers put NLB in front of ALB or Envoy

**Algorithms**
- Round robin
	- walks an ordered list. It is dead simple and distributes evenly across homogeneous hosts. It ignores actual load: a stalled host keeps receiving traffic until a health check removes it
- Weighted round robin
	- repeats higher-weight hosts proportionally. Use it when backends have different capacities (mixed-generation hardware).
- Least-connection / least-outstanding-requests
	- picks the host with the fewest in-flight requests. The naive version herds: when many LB instances simultaneously pick the same "least loaded" backend, that backend spikes.
- Power of two choices (P2C)
	- fixes the herding problem. Pick two backends at random; send the request to whichever has fewer outstanding requests.
	- Not 1 or not 3
- Consistent Hashing
	- routes requests by hashing a stable key (user ID, cache key, 5-tuple) into a server slot. When one of N backends is removed, roughly 1/N of keys move.

- Use round robin for homogeneous stateless services. Use P2C least-request as the default for heterogeneous or latency-sensitive workloads. Use consistent hash only when you need cache affinity or flow pinning, and accept that hot keys can overload one backend

**Health checks and outlier detection**
- Active health check
- Passive (outlier) health check
- Connection draining
- Slow start

Session persistence
- sticky sessions are a smell

Global load balancing
- BGP Anycast
- DNS-based GSLB

TLS termination
- Termination at the LB
- Passthrough
- mTLS in a service mesh

Where does the load balancer fit ?

|Approach|Pros|Cons|Best when|Our Pick|
|---|---|---|---|---|
|L4 (NLB, Maglev, Katran)|Line-rate throughput, protocol agnostic, flow pinning via consistent hash|No HTTP features, no header routing, no retries|High throughput, non-HTTP, DDoS absorption tier|**Front tier always**|
|L7 (ALB, Envoy, NGINX, HAProxy)|Rich routing, TLS termination, retries, auth, rate limiting|CPU cost per request, adds latency, TLS keys at the LB|HTTP services, microservice mesh, multi-tenant ingress|**Behind L4 for HTTP**|
|Client-side LB (gRPC, Finagle)|No extra network hop, latency-aware selection|Complex client, needs service discovery, every language must implement|Internal RPC meshes between your own services|When you own both ends|

Which algorithm picks the backend ?

| Algorithm                      | Pros                                                                         | Cons                                                                     | Best when                                 | Our Pick                           |
| ------------------------------ | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------ | ----------------------------------------- | ---------------------------------- |
| Round robin / weighted RR      | Dead simple, no state                                                        | Ignores actual load; stalls a slow backend until health check removes it | Homogeneous stateless services            | Starting default, replace with P2C |
| P2C least-request              | Cheap, self-balancing, resistant to herding                                  | Requires per-backend in-flight count                                     | Heterogeneous backends, latency-sensitive | **Default for L7**                 |
| Consistent hash (ring, Maglev) | Stable assignment for cache affinity; ~1/N reassignment on membership change | Sensitive to key skew; hot keys overload one backend                     | Cache routing, flow pinning               | Only for affinity                  |

Common pitfalls
- Treating the LB as single point of failure
- Deep health checks causing cascading failures
- Botched connection draining during deploys
- DNS TTL too high for failover
- Not tuning SO_REUSEPORT
- Sticky sessions making statefulness

Key takeaways

- L4 is fast and dumb; L7 is smart and expensive. Most production stacks use both: L4 in front for packet volume, L7 behind for routing intelligence.
- P2C least-request is the best default algorithm. It self-balances, resists herding, and handles heterogeneous backends without configuration.
- The load balancing algorithm matters less than good health checks and connection draining. A perfect algorithm with broken health checks still sends traffic to dead servers.
- Sticky sessions are a smell. Externalize state to Redis or a JWT; use consistent hash only for cache affinity where losing affinity degrades latency, not correctness.
- Anycast with stateless L4 gives sub-second global failover. DNS-based GSLB is simpler but fails over in minutes due to TTL caching.
- Every server can be a load balancer. Cloudflare's Unimog proves that eliminating a separate LB tier removes a capacity-planning problem at less than 1% CPU overhead
- Enable `SO_REUSEPORT` on any multi-core proxy. It is one config line that eliminates accept-queue contention and can double throughput


