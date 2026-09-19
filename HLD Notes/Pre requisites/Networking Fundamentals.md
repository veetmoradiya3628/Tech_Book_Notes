
- TCP vs UDP
- HTTP generations
	- HTTP 1.1
	- HTTP/2
	- HTTP/3 - QUIC
- DNS
- TLS 1.3
- WebSocket vs. SSE vs. long polling

| Layer         | What It does                          | Examples              |
| ------------- | ------------------------------------- | --------------------- |
| 7 Application | The protocol your code speaks         | HTTP, gRPC, SMTP, DNS |
| 4 Transport   | End-to-end delivery between processes | TCP, UDP, QUIC        |
| 3 Network     | Routing packets between hosts         | IP, ICMP              |
| 2 Data Link   | Moving frames on a wire or radio      | Ethernet, Wi-Fi       |

- TCP vs. UDP
	- TCP gives you a reliable, ordered byte stream. It handshakes, retransmits lost packets, and paces itself to avoid congesting the network. UDP gives you a datagram: fire it and forget.
	- **Use TCP** when correctness matters more than speed: HTTP, database connections, file transfer, SSH.
	- **Use UDP** when loss is tolerable or you want to build your own reliability: DNS queries (one packet, retry if it drops), video calls (a dropped frame beats a stalled stream), game networking, and QUIC (which reinvents reliable transport on top of UDP).

- HTTP varieties
	- HTTP/1.1
		- It is text-based and serial: one request at a time per TCP connection. Browsers work around this by opening up to 6 parallel connections per origin.
		- You still see HTTP/1.1 everywhere: internal services, curl scripts, health checks.
	- HTTP/2
		- It multiplexes many requests over a single TCP connection using binary framing and HPACK header compression.
		- One connection, many parallel streams.
		- The catch: TCP head-of-line (HOL) blocking. If one TCP segment drops, every HTTP/2 stream stalls until the retransmit arrives, even streams that had nothing to do with the lost packet
	- HTTP/3
		- It runs on QUIC (RFC 9000), which runs on UDP. QUIC builds its own streams with independent loss recovery, so a lost packet only stalls the stream it belonged to.
		- QUIC also combines the transport and TLS handshakes into a single 1-RTT operation and supports 0-RTT resumption where a returning client sends data in the very first packet.

- DNS resolution
	- Its hierarchical cache walk, caches at every layer mean popular names resolve in under a millisecond from warm cache.
	- Runs over UDP on port 53
	- Famous name servers
		- 1.1.1.1 - cloud flare
		- 8.8.8.8 - google 
	- for queries within 512 bytes it is on UDP then switch to TCP for larger response, 
	- DNS-Over-HTTPS (DoH) and DNS-Over-TLS (DOT) add privacy by encrypting queries

- TLS 1.3 handshake
	- TLS 1.2 - 2 round trips to 1 round trip in TLS 1.3
	- 0-RTT resumption
	- Never use 0-RTT for state-changing requests

- Real-time transports 
	- WebSockets
		- upgrade an HTTP connection into a full-duplex binary channel. Low per-message overhead: 2 to 14 bytes of framing
		- Use for chat, multiplayer games, and collaborative editors where both sides push data.
	- SSE
		- uses a long-lived HTTP response with `Content-Type: text/event-stream`. The server streams text events. One-way (server to client), built-in reconnection via `Last-Event-ID`, works over HTTP/1.1 and HTTP/2. Under HTTP/1.1, browsers cap at 6 concurrent connections per domain, so SSE consumes one of those slots
		- Over HTTP/2 the limit becomes the negotiated `SETTINGS_MAX_CONCURRENT_STREAMS`; RFC 9113 sets no initial limit but recommends a value no smaller than 100; most servers (Kestrel, nghttp2, curl) advertise exactly 100.
	- Long polling
		- **Long polling** opens an HTTP request and the server holds it until data is ready or a timeout fires. Simple, works through any proxy. High overhead: every message is a fresh HTTP request with full headers.

- Tradeoffs

| Transport    | Pros                                                       | Cons                                                      | Best When                                                      | Our pick                                              |
| ------------ | ---------------------------------------------------------- | --------------------------------------------------------- | -------------------------------------------------------------- | ----------------------------------------------------- |
| TCP          | Reliable ordered byte stream, congestion control           | 1-RTT handshake, TCP head-of-line blocking on lossy links | Correctness-critical traffic (HTTP, database connections, SSH) | Default for APIs and data                             |
| UDP          | Zero setup, low per-packet overhead                        | No reliability, ordering, or congestion control           | Video, voice, games, DNS, QUIC/HTTP/3                          | When you build your own reliability on top            |
| HTTP/1.1     | Universal, debuggable with curl, trivial proxies           | One request at a time per connection, verbose headers     | Internal services, curl scripts, health checks                 | When simplicity and universal tooling beat throughput |
| HTTP/2       | Multiplexing on one connection, HPACK compression          | TCP head-of-line blocking on lossy links                  | Modern web over reliable networks                              | Default for most web traffic                          |
| HTTP/3       | Per-stream loss recovery over QUIC, 0-RTT resumption       | UDP blocked on some enterprise networks                   | Mobile-first traffic, global users                             | When you control the edge (CDN advertising `Alt-Svc`) |
| WebSocket    | Full-duplex binary channel, 2-14 byte frame overhead       | Sticky sessions, proxy quirks, custom reconnect logic     | Chat, multiplayer games, collaborative editing                 | When the client also needs to push data               |
| SSE          | Simple over plain HTTP, auto-reconnect via `Last-Event-ID` | One direction only, text-only payload                     | Dashboards, notifications, LLM token streams                   | Default for server push                               |
| Long polling | Works through any HTTP proxy or firewall                   | High overhead per message (full HTTP round-trip)          | Fallback behind middleboxes that block WebSocket and SSE       | When universal compatibility is required              |

- Common pitfalls
	- Forgetting DNS TTL during database failover - A 300 second TTL means up to 5 minute of errors, use 30 to 60 second TTLs for failover-targeted records
	- HTTP/2 multiplexing defeated by a chatty L7 proxy.
	- Use SSE in general and move to WebSocket when required only
	- TLS 1.3 0-RTT used for non-idempotent requests only
	- SSE stalled at the HTTP/1.1 6 connection browser limit
		- An SSE-heavy app opens multiple tabs; the 7th tab cannot open a stream because the browser hit its 6-connection-per-domain cap. This is marked "Won't fix" in Chrome and Firefox


