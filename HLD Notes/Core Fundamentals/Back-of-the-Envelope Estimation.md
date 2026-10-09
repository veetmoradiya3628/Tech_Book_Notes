
- Estimations should be based on assumption and it need not be exact though it needs to be accurate around real figure

Powers of Ten

| Power | Bytes     | Decimal approx | Unit |
| ----- | --------- | -------------- | ---- |
| 2^10  | 1,024     | 10^3           | KB   |
| 2^20  | 1,048,576 | 10^6           | MB   |
| 2^30  | ~10^9     | 10^9           | GB   |
| 2^40  | ~10^12    | 10^12          | TB   |
| 2^50  | ~10^15    | 10^15          | PB   |
Time constants

- 1 day = 86,400 s. Round to **10^5** (15% error, always acceptable).
- 1 year = 31.5M s. Round to **3 x 10^7**.
- 1 month = 2.6M s. Round to **2.5 x 10^6**.

Population anchors

- World: 8 billion. Smartphone users: 5 billion.
- Facebook DAU: 2.1 billion. YouTube MAU: 2.7 billion.
- WhatsApp messages/day: 100 billion (2024)

The latency hierarchy

| Operation                 | Latency | Order |
| ------------------------- | ------- | ----- |
| L1 cache reference        | 0.5 ns  | 10^-9 |
| Main memory reference     | 100 ns  | 10^-7 |
| NVMe SSD random 4 KB read | 16 us   | 10^-5 |
| Same-datacenter RTT       | 500 us  | 10^-4 |
| Disk seek                 | 10 ms   | 10^-2 |
| CA to Netherlands RTT     | 150 ms  | 10^-1 |

QPS Estimation
- avg_QPS = DAU x request_per_user_per_day / 10^5
- peak_QPS = avg_QPS x peak_multiplier

| Service class                         | Peak/avg ratio | Evidence                                                                                       |
| ------------------------------------- | -------------- | ---------------------------------------------------------------------------------------------- |
| Global consumer (always-on)           | 2 to 3x        | Twitter daily pattern                                                                          |
| Regional consumer (one timezone)      | 3 to 5x        | Shopify BFCM 2024: 4.7M RPS edge                                                               |
| B2B (business hours)                  | 5 to 10x       | Monday 9-11am spike                                                                            |
| Event-driven (live sports, elections) | 10 to 30x      | Super Bowl ~10x network traffic; elections 20-50x for news sites; World Cup 2014 peak 618K TPM |
Assume 5x peak unless you know better. It is the geometric mean of the 2x to 10x range and rarely gets you into trouble.

Storage Estimation

- `records_per_day x bytes_per_record x retention_days x replication_factor x 1.3 (indexes)`

Per object size reference

| Object type             | Typical size  | Notes                          |
| ----------------------- | ------------- | ------------------------------ |
| Tweet/short message     | 200 B to 1 KB | Text + metadata + pointers     |
| Chat message (WhatsApp) | ~1 KB         | E2E encrypted payload          |
| User profile row        | 1 to 5 KB     | Name, email, prefs, avatar URL |
| GPS ping                | 50 B          | lat, lng, timestamp, trip_id   |
| Photo metadata          | 5 to 10 KB    | EXIF, tags, permissions        |
| Photo file (compressed) | 2 to 5 MB     | JPEG/WebP                      |
| 1 min video (720p)      | 10 to 15 MB   | H.264 encoded                  |

Bandwidth estimation
- `peak_QPS x avg_payload_size`
- Network engineers use bits, Storage engineers uses bytes
- 1 Gbps = 125 MB/s not 1 GB/s
- Confusing this can inflates capacity estimation by 8x

Memory and CPU sizing
- Redis per-key overhead
- Cache sizing rule
- Thread count heuristic

Twitter timeline hybrid fanout model
```
Incoming tweet 
- if poster has 20M+ tweets ?
  	Yes
	  - fanout-on-read merge at read time from celebrity list
	No
	  - fanout-on-write insert into each follower's redis timeline list
  Redis fetches pre-built timeline O(1) latency
```

- fanout-on-write vs. fanout-on-read

Common pitfalls
- Ignoring peak vs average - always multiply by a peak factor
- Forgetting replication factor
- Forgetting backups
- Confusing Mbps with MBps
- Assuming uniform access pattern
- Forgetting index overhead

