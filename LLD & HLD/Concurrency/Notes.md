
- Process vs. Threads
- Process vs. Threads memory and address space difference
- Process components
	- Address space
		- Divided into below segments
			- Text (Code)
			- Data
			- BSS
			- Heap
			- Stack
	- Resources
	- Execution state
	- Security context
- Thread
	- unit of execution within a process
	- Thread within same process share
		- Address space
		- Resources
		- Process ID
	- Each thread has its own
		- Stack 
		- Registers
		- Thread ID
		- Thread-local storage


| Mechanism     | Data Shape          | Speed    | Best for                                       |
| ------------- | ------------------- | -------- | ---------------------------------------------- |
| Pipe / FIFO   | Byte stream         | Fast     | Simple producer-to-consumer flow               |
| Message Queue | Discrete messages   | Fast     | Request-response, prioritized events           |
| Shared memory | Raw memory region   | Fastest  | Large data, high-frequency exchange            |
| Socket        | Byte/message stream | Moderate | Client-server, same host or across the network |
| File          | Bytes on disk       | Slowest  | Durable handoff, logs, checkpoints             |

- Context switch
	- Process vs. Thread context switch
- When to use process
	1. Fault isolation is critical
	2. You need strong security boundaries
	3. You want to scale across machines
	4. You need to work around language / runtime limits
	5. Tasks are simple and mostly independent
- When to use threads 
	- Tasks needs to share data frequently
	- You need very low-latency communication
	- Resource efficiency matters
	- You need fine-grained parallelism
	- Your language / runtime supports threading well
- Multi-Process with Multi-threading
- Process pool pattern
- Thread Pool within Process
- PCB - Process control block
- TCB - Thread control block
- Kernel Threads vs User Threads
	- 1:1 Model
	- N:1 Model
	- N:N Model (Hybrid)
- Race condition
	- A race condition exists when the program's correctness depends on the interleaving of operations from multiple threads, and at least one of those operations is a write.
- read-modify-write race
- check-then-act race
- A critical section is a region of code that reads or writes shared state.
- mutual exclusion - at any instant. at most one thread is allowed to execute that protected region. everyone else must wait until the current thread leaves.
	- mutex lock
	- synchronized section vs. lock
	- atomic operations
- Immutable data is thread safe by nature

- Java Threads
	- Ways to create threads
		- Extending Thread class & overriding run() method
		- Implementing Runnable interface and overriding run() interface
			- Lambda expression
		- Implementing Callable interface with executor service
	- Runnable vs. Callable
	- Thread configuration
		- Naming conventions
		- ThreadPriority (1 - 10)
	- Deamon threads
	- ExecutorService
	- run() vs. start() difference ?
		- run() - no new thread is created, the code executes synchronously in the calling thread
		- start() - the JVM creates new OS thread and schedules run() to execute on that new thread, running concurrently with the calling thread.
	- join() and timeouts
- Completable Future
- ThreadFactory
- Common Patterns
	- One-shot execution pattern
	- Worker Thread pattern
	- Background cleanup pattern
- Java Memory Model (JMM)
	- L1, L2, L3 cache
	- Store buffer
	- L2 cache shared across core
- Visibility problems
- Volatile read / write, Synchronized