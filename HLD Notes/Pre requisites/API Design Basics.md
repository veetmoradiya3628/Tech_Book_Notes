REST, GraphQL, gRPC, versioning, idempotency, pagination

- REST API - for public APIs
- GraphQL - many clients need varying response shapes
- gRPC - default for internal service-to-service calls
	- 10x throughput over REST for large payloads

REST resource modeling
- REST models server state as named resources identified by URLs and manipulated with a small, fixed set of HTTP verbs.
- Resource is a noun: `/users/42`, `orders/123/line-items`. not `/getUser` or `/createOrder`

|Verb|Purpose|Safe?|Idempotent?|
|---|---|---|---|
|GET|Read|Yes|Yes|
|POST|Create or action|No|No|
|PUT|Replace entirely|No|Yes|
|PATCH|Partial update|No|Depends|
|DELETE|Remove|No|Yes|
Status codes
- 2xx success - 200 OK, 201 Created with Location header, 204 No content
- 4xx client error - 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 409 Conflict, 422 Processable Entity, 429 Too many requests
- 5xx server error - 500 Internal Server Error, 504 Service Unavailable

- Model the entity first then the URL. if you find yourself writing `/user/update` or `/order/cancel` step back. can that be `PATCH /users/42` or `POST /orders/42/cancellations`

GraphQL and the N+1 problem
- GraphQL is a query language where clients post a single query naming exactly the fields they want, and the server resolves each field with a dedicated resolver function.
- One round trip. The server returns exactly those fields. Mobile clients love this because they avoid over-fetching on slow networks.
- Use GraphQL when - many different clients (web, iOS, Android, partners) need different shapes of the same data. You control both client and server.
- Do not use GraphQL when you have a simple CRUD API with one client (REST is less work), or a public API where query cost must be predictable (GraphQL's flexibility makes rate limiting harder).
- GraphQL resolvers run per-field per-parent. A query for 100 users and each user's orders becomes 1 + 100 = 101 database queries. this is problem with GraphQL
	- DataLoader collapses 1+N database round trips into 2 by batching all `.load(id)` calls within a single event-loop tick.
	- The DataLoader cache is per-request to avoid cross-user data leaks.

gRPC, Protobuf and streaming models
- gRPC is a contract-first RPC framework. You declare services in a `.proto` file, compile to strongly-typed client stubs and server skeletons in 10+ languages. Transport is HTTP/2.  Default serialization is Protocol Buffers.
- Protobuf is binary and typically around 3x smaller than equivalent JSON
- Benchmarks show gRPC delivering 15 to 40% greater throughput at lighter loads and up to 10x at large payloads
- HTTP/2 multiplexes many concurrent calls on one TCP connection. Deadlines are first-class: if not completed in time, the call terminates with `DEADLINE_EXCEEDED`
- Types
	- unary
	- server streaming
	- client streaming
	- bidirectional
- Use gRPC for internal service-to-service communication where you control both ends.

Versioning strategies
- Common methods for versioning
	- URL path
	- custom header
	- date-based rolling
- Use header-based versioning (Accept-Version) if you can. Use URL-path versioning if you need simplicity for partners who curl your API. Avoid integer major versions that force clients to rewrite URLs

Idempotency keys
- A client-generated unique token sent with a write request so the server can detect and dedupe retries. The client generates a UUID, puts it in the `Idempotency-Key` header, and retries with the same key on network error.
- The server saves the response (status code and body) keyed by `(api_key, idempotency_key)` and returns the cached copy on repeat. Stripe keeps keys for 24 hours and limits them to 255 characters
- If the same key arrives with different parameters, Stripe returns an error to prevent accidental reuse.
- Always accept an `Idempotency-Key` header on POST endpoints that mutate money or trigger side effects. Store the key and response atomically in the same transaction as the side effect. If you do not, a crash between "commit payment" and "write cached response" leads to a double-charge on retry.

Pagination: cursor vs. offset
- Offset pagination
	- is simple but breaks at scale. The database implements `OFFSET 100` by scanning and discarding 100 rows. At page 5,000 with limit 20, the DB reads 100,020 rows to return 20  Worse: if rows are inserted between page fetches, items are skipped or duplicated.
- Cursor pagination
	- encodes the last-seen row's sort key into an opaque token.
	-  Performance is independent of page depth. Slack migrated to cursors after endpoints designed for "several hundred records" grew to "hundreds of thousands of records"
- Always use cursor (keyset) pagination for anything that can reach 10K+ rows. Reserve offset for admin UIs with small, bounded datasets.

Rate limiting
- The token bucket algorithm is the most common for public APIs: tokens refill at rate R up to burst capacity B; each request consumes one token. 
- Always return `429 Too Many Requests` with a `Retry-After` header. Publish `x-ratelimit-remaining` so clients can self-throttle before hitting the limit. GitHub allocates 5,000 points per hour per user for their GraphQL API

Error handling
Authentication
- API keys for simple server-to-server. OAuth 2.0 for delegated user authorization. JWT for stateless token validation.

idempotency and versioning are not features you bolt on later. They are architectural decisions that must be correct on day one because every client immediately depends on them.


|Style|Pros|Cons|Best when|Our Pick|
|---|---|---|---|---|
|REST|Universal tooling, HTTP caching by URL, simple mental model, safe/idempotent verbs built in|Over-fetching and under-fetching, version bumps costly, many round trips for nested data|Public APIs, CRUD, partner integrations, browser consumption|Default for external APIs|
|GraphQL|Precise client-specified shapes, single endpoint, federation across services, avoids over/under-fetching|N+1 without DataLoader, query cost hard to predict, no URL-keyed HTTP cache|Many diverse clients reading the same data graph (mobile + web + partners)|When you have 50+ screens fetching varying shapes|
|gRPC|~3x smaller payloads, up to 10x throughput for large payloads, 4 streaming modes, generated typed stubs, HTTP/2 multiplexing|Binary wire format hard to inspect, browsers need gRPC-Web, HTTP/2 breaks naive L4 load balancers|Internal service-to-service, latency-sensitive RPCs, streaming|Default for internal services|
Common Pitfalls
- GraphQL N+1 without data loader
- gRPC load balancer sticky connections
- Idempotency key not stored atomically with response.
- Offset pagination race on mutation
- URL-based versioning forcing client rewriting
- Missing 429 with no Retry-After header

Key takeaways

- REST is the right default for public APIs. GraphQL is right when clients fetch varying shapes. gRPC is right for internal service-to-service.
- The paradigm choice (REST vs GraphQL vs gRPC) is smaller than the cross-cutting concerns: versioning, idempotency, and pagination.
- Every GraphQL server needs DataLoader from day one. The N+1 problem turns a "cheap" query into 101 database calls.
- gRPC delivers up to 10x throughput over REST for large payloads but requires L7 load balancing because HTTP/2 multiplexes on a single TCP connection.
- Always use cursor pagination for datasets that can exceed 10K rows. Offset pagination breaks under concurrent writes and degrades at depth.
- Always accept an `Idempotency-Key` header on POST endpoints that mutate money or trigger side effects. Store the key atomically with the side effect.
- Stripe's date-based rolling versioning lets them ship 100 breaking changes without forcing a single client to upgrade. Design for this from day one.
