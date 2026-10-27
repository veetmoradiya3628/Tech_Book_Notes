
Works of Reverse proxy
- A reverse proxy is concierge. it sits between every client and backend service
- terminates TLS so backend do not manage certificates
- It compresses responses so backends do not burn CPU on gzip.
- It buffers slow clients so backends are not held hostage by a trickle of bytes
- it routes requests to the right upstream based on path, header, or hostname.

API Gateway vs. Reverse Proxy vs. Load Balancer
- A load balancer
	- distributes traffic across N identical backends. Its job is even spread and health tracking
- A reverse proxy
	- terminates the client connection and forwards it to an upstream. Its job is protocol handling: TLS, HTTP/2, compression, buffering, header rewriting, and connection pooling.
- An API Gateway
	- enforces API product policy. Its job is business-layer concerns: authentication, authorization, per-consumer rate limits, request transformation, schema validation, and developer portals.

What does reverse proxies do
- TLS termination
- HTTP/2 and HTTP/3 at the edge
- Compression
- Request buffering
- Connection pooling
- Header rewriting and request routing

API Gateway
- Authentication and authorization
- Request limiting and quotas
- Request and response trasformation
- Developer portal and key management

North-south vs. East-west
- **North-south** traffic crosses the trust boundary: browsers, mobile apps, and partner APIs hitting your edge. It needs heavy security: WAF, DDoS defense, TLS termination, geographic routing, bot detection, and rate limiting by IP.
- **East-west** traffic flows between internal services. It cares about mTLS, service discovery, retries, circuit breakers, and locality-aware load balancing. The threat model is different: you trust the network (mostly), but you need authenticated service identity.

Patterns
- BFF - Backend for front end
	- Instead of one general-purpose gateway, each client type (web, iOS, Android) gets its own dedicated edge service that aggregates downstream microservices into client-shaped responses. The web BFF concatenates four calls into a single response matching the web view model. The mobile BFF returns a smaller, battery-friendly shape. Each BFF is owned by the team that owns that client.
- strangler fig
	- The gateway fronts both the legacy monolith and new services, routing requests to the new service when ready and leaving the rest on the monolith. The monolith shrinks one route at a time until it can be decommissioned. Shopify built exactly this with Lua in NGINX+OpenResty: rules routed requests between the legacy Rails storefront and the new Storefront Renderer, with rates for render, forward-verify, reverse-verify, and self-verify
- request coalescing
	- When many clients request the same resource simultaneously (a cache stampede), the gateway collapses duplicate in-flight requests into one upstream call and fans the response back to all waiting clients. NGINX calls this `proxy_cache_lock`; Envoy achieves similar behavior through its cache filter configuration.

Tools
- NGINX
- Envoy
- Traefik
- Caddy
- Kong

Trade-offs

| Approach                                 | Pros                                                                                                                         | Cons                                                                                       | Best when                                            | Our Pick                             |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ | ---------------------------------------------------- | ------------------------------------ |
| Thin reverse proxy (NGINX, Envoy, Caddy) | Fast, minimal, easy to reason about                                                                                          | You build auth, rate limiting, and portals yourself                                        | Routing + TLS + health checks, not product features  | **Default for internal services**    |
| Full API gateway (Kong, Apigee, Tyk)     | Batteries included: dev portal, plugins, analytics                                                                           | Vendor lock-in, cost, shared SPOF                                                          | Public API product with many external consumers      | **Public APIs with SLAs**            |
| Cloud-managed (AWS API Gateway, GCP)     | Zero ops, pay per request                                                                                                    | Per-request cost at scale, cold starts (100 ms to seconds without Provisioned Concurrency) | Low-to-moderate traffic APIs on Lambda backends      | Small teams, < 100 rps sustained     |
| Service mesh sidecar (Envoy + Istio)     | Gateway features per service, mTLS everywhere                                                                                | Operational weight, sidecar CPU/memory overhead                                            | Large microservices estate where east-west dominates | **East-west default**                |
| GraphQL gateway (Apollo Router, Cosmo)   | One client query, optimal subgraph fan-out, 8x throughput vs Node.js                                                         | Newer tooling, N+1 pitfalls, schema governance overhead                                    | Many teams, many clients, shared domain model        | When you have the org for federation |
| Edge compute (Cloudflare Workers)        | Anycast network spanning 330+ cities in 120+ countries, effectively zero cold start (isolate preloaded during TLS handshake) | Limited runtime (V8/Wasm), vendor API surface                                              | Global latency-sensitive APIs, custom edge logic     | Latency-critical global APIs         |

Common pitfalls
- Upstream timeout cascade
- Slowloris exhausting connections
- Cache poisoning via unkeyed headers
- Header injection and request smuggling
- GraphQL N+1 via aggregation
- Business logic creep into the gateway

Key takeaways 

- A reverse proxy handles routing and TLS. An API gateway adds product features: auth, quotas, keys, portals. Most tools (Envoy, NGINX, Kong) can play multiple roles.
- Keep business logic out of the gateway. It should enforce cross-cutting policy, not implement features. If gateway config grows faster than your service fleet, you are centralizing logic that belongs in services.
- North-south and east-west traffic have different threat models and different optimal tools. Do not force one gateway to serve both.
- The BFF pattern gives each client team its own gateway, eliminating the shared-gateway coordination bottleneck. Use it when you have distinct client shapes (web, mobile, partner).
- The strangler fig pattern uses the gateway as a traffic splitter during migrations. Rollback is setting one percentage to zero.
- Cloud gateways win below ~5,000 rps sustained. Above that, per-request cost dominates and self-hosted wins.
- Netflix Zuul 2 proves that separating filter logic (team-owned) from data-plane plumbing (platform-owned) scales to 1M+ rps across 80+ clusters