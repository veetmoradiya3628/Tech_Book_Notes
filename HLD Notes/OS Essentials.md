
- Process vs. Thread
	- A **process** is an isolated execution environment: its own virtual address space, page tables, file descriptor table, and memory protections. Two processes cannot read each other's memory without explicit IPC.
	- A **thread** is a flow of execution inside a process. Threads share heap, globals, and file descriptors. On Linux, both are created via `clone(2)` with different flags: threads share `CLONE_VM` and `CLONE_FILES`, which makes creation and switching cheaper because the TLB (translation lookaside buffer) stays warm
	- A **context switch** saves one task's registers and restores another's. Between threads in the same process this costs roughly 1 microsecond. Between processes it costs 3 to 5 microseconds because the TLB must flush

|Property|Process|Thread|
|---|---|---|
|Memory space|Isolated|Shared within process|
|Creation cost|~1 ms|~10 us|
|Context switch|~3-5 us (TLB flush)|~1 us (TLB warm)|
|Crash blast radius|Isolated|Takes whole process down|
|Shared-state bugs|None (IPC required)|Race conditions|

- If `vmstat` shows hundreds of thousands of context switches per second and CPU is mostly in system time, you have too many runnable tasks. Reduce concurrency or switch to an event loop.

- Numbers every programmer should know

| Operation                     | Latency     | Relative to L1     |
| ----------------------------- | ----------- | ------------------ |
| L1 cache reference            | 0.5 ns      | 1x                 |
| Branch mispredict             | 5 ns        | 10x                |
| L2 cache reference            | 7 ns        | 14x                |
| Mutex lock/unlock             | 25 ns       | 50x                |
| Main memory reference         | 100 ns      | 200x               |
| Send 1 KB over 1 Gbps network | 10 us       | 20,000x            |
| NVMe SSD random 4 KB read     | 20 to 70 us | 40,000 to 140,000x |
| Same-datacenter round trip    | 500 us      | 1,000,000x         |
| HDD disk seek                 | 10 ms       | 20,000,000x        |
| Cross-continent round trip    | 150 ms      | 300,000,000x       |

- keep hot data in RAM. If you cannot, keep it on NVMe. If you cannot, design around the 10 ms cost of a disk seek with batching, pipelining, and async I/O.
- Network calls are most expensive so never treat networks calls as cheap

I/O models
- how does one machine serve 10000 concurrent connections ?
- Blocking I/O 
	- Thread per connection
	- limited by no. of max thread in system based on memory, each thread reserved 8MB stack
	- context-switch overhead
- I/O multiplexing (event loop)
	- One thread watches many file descriptors. The kernel tells you which are ready, and you handle them in a loop. `select` is O(n) and capped at 1,024 fds. `poll` removes the cap but stays O(n). `epoll` (Linux 2.5.44+, October 2002) keeps the watch set in kernel state and returns only ready fds in O(ready) time
	- `kqueue` (FreeBSD, macOS) provides the same pattern on BSD systems.
- Async I/O (io_uring)
	- Linux `io_uring` (kernel 5.1, 2019) uses two shared ring buffers: a submission queue and a completion queue. Userspace and kernel pass operations without a syscall per op, achieving multi-x throughput over syscall-per-op patterns in microbenchmarks
```
Concurrent connections ?

if no. of connection < 1K:
	Thread-per-connection, simple code, fine here
	
if no. of connection 1K to 100K+:
	if Platform == BSD/MacOs :
 		kqueue event loop
	if Platform == Linux :
		epoll event loop or async runtime
		
if high throughput disk + network:
	if Linux 5.1+ :
		io_uring (trusted hosts only)
	if not Linux 5.1+ :
		epoll event loop or async runtime

if C10M:
	kernet bypass DPDK/XDP
```

- filesystem
	- A **file system** organizes bytes on a block device into files and directories. An **inode** holds a file's metadata and pointers to data blocks. Directories map names to inode numbers.
	- **Journaling** protects against corruption after a crash. Before modifying metadata, the FS writes a journal entry describing the intent. After a crash, recovery replays the journal. ext4 defaults to `data=ordered` mode: metadata is journaled, data is written before the metadata commit
- page cache
	- The **page cache** is the kernel's RAM cache of file contents. Reads hit it when warm (nanoseconds). Writes land in dirty pages that flush to disk later. This is why your second benchmark run is 10 to 100x faster than the first: the first run populated the cache from disk.
- Durability requirement
	- fsync vs. write
	- group commit 
	- forced fsync

- syscalls and kernel cost is not free

- Real world example
	- Redis - single threaded event loop serving 100K ops/sec
	- Async Event concept 
	- main loop is based on epoll_wait
	- Redis bottleneck is network or memory bandwidth, not CPU
	- A single Redis thread handles ~100K operations per second and tens of thousands of concurrent connections on commodity hardware
	- Since single thread no overhead for context switch, no thread pool etc
	- Redis processes commands sequentially on one thread; epoll tells it which connections have data, eliminating the need for thousands of threads.

| Approach                     | Pros                                                                                             | Cons                                                                 | Best when                                     | Our Pick                                  |
| ---------------------------- | ------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------- | --------------------------------------------- | ----------------------------------------- |
| Thread-per-connection        | Simple code, easy debugging                                                                      | ~8 MB stack/thread (virtual), high context-switch cost past 1K conns | < 1K connections, CPU-heavy per request       | Low-concurrency internal services         |
| epoll/kqueue event loop      | Scales to 100K+ conns, tiny memory per conn                                                      | Callback complexity, edge-triggered gotchas                          | Network-bound servers (Redis, Nginx, Node.js) | **Default for network services**          |
| io_uring                     | True async for disk + network; multi-x throughput over syscall-per-op in Axboe's microbenchmarks | Linux 5.1+ only, broad attack surface, API evolving                  | High-throughput storage on trusted hosts      | Databases, CDN caches on modern Linux     |
| Async runtime (Tokio, Netty) | Ergonomic async/await, good ecosystem                                                            | Runtime overhead, function coloring                                  | Mixed CPU and I/O workloads                   | Application-layer services                |
| Kernel bypass (DPDK, XDP)    | Line-rate 10M+ pps per core                                                                      | Reimplement TCP, operational complexity                              | C10M, telecom, 100GbE middleboxes             | Only when the kernel is proven bottleneck |

Common pitfalls
- Thinking `fsync` is free
	- Use group commit: buffer writes for a few ms, append as a batch, fsync once then ack the entire batch
- Thread pool exhaustion under blocking I/O
	- A 16-thread pool with a 500 ms downstream p99 stalls at 32 req/sec even with idle CPU. Use async clients so threads return to the pool during the wait, or dedicate an oversized pool for I/O-bound work.
- **Cold page cache benchmarks.** 
	- The first run reads from disk; the second reads from RAM. Your benchmark looks 100x faster on run two. Either drop caches before each run or pre-warm and report steady-state. Always say which you measured.

- 