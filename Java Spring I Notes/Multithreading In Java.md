
- Process is an instance of a program that is getting executed
- It has its own resources like memory, thread etc. OS allocate these resources to process when its created
- `java -Xms256m -Xmx2g MainClassName`
	- -Xms - set the initial heap size
	- -Xmx - Max heap size that program can get
- Thread
	- lightweight process
	- 1 process can have multiple threads
	- When program process is created, 1 main thread is created and we can create more threads when we needed
- code segment
	- contains compiled bytecode
	- all threads within the same process, share the same code segment
- data segment
	- contains global and static variables
	- all threads within the same process can read and write on this data
	- synchronization required
- heap
	- objects created at runtime using new keyword are allowed in heap
	- heap is shared among all the threads within the same process
	- threads can read and modify the heap data
	- synchronization required
- Stack
	- each thread has its own stack
	- It manages method calls, local variables
- Register
	- when JIT (Just in time) compliers converts the bytecode into native machine code, its uses register to optimized the generated machine code
	- also helps in context switching
	- each thread has its own register
- Counter
	- also known as PC program counter, points to the instructor getting executed
	- Increments its counter after successfully execution of the instructor
- Multithreading
	- Allows a program to perform multiple task at the same time
	- Multiple threads share the same resource such as memory space but still can perform task independently
	- benefits
		- improved performance by task parallelism
		- responsiveness
		- resource sharing
	- challenges
		- concurrency issue like deadlock, data inconsistency etc
		- synchronized overhead
		- testing and debugging difficult
- Multitasking vs. Multithreading

- Thread creation ways
	- implementing runnable interface
		- implement run() method
		- create thead class with providing runnable interface implementor class and start
	- extending thread class
		- create a class that extends Thread class
		- override the `run()` method to tell the task which thread has to do
		- create instance of the class and call the start method
- A class can implement more than 1 interface but a class can extend only 1 class

- Thread Lifecycle

![[Pasted image 20260718110145.png]]


| state         | description                                                                                |
| ------------- | ------------------------------------------------------------------------------------------ |
| New           | Thread has been created but not started, its just an object in memory                      |
| Runnable      | Thread is ready to run, waiting for CPU time                                               |
| Running       | When thread starts executing its code                                                      |
| Blocked       | different scenario where runnable thread goes into the blocking state - I/O, Lock acquired |
| Waiting       | Thread goes into this state when we call wait() method, makes it non runnable              |
| Timed waiting | sleep() or join()                                                                          |
| Terminated    | life of thread is completed, it can not be started back again                              |

- Monitor lock
	- It helps to make sure only 1 thread goes inside the particular section of the code (a synchronized block or method)
- Implement producer consumer problem
- Why stop, resume, suspended method is deprecated ?
	- STOP - terminates thread abruptly, no lock release, no resource cleanup
	- SUSPEND - put the thread on hold for temporary
	- RESUME - used to resume the suspended thread
- Join
	- when join method is invoked on a thread object. current thread will be blocked and waits for the specific thread to finish
	- It is helpful when we want to coordinate between threads or to ensure we complete certain task before moving ahead
- Thread priority
	- 1 - low, 10 - high
	- its just hint to thread scheduler but its not always the case that it will follow this priority
	- setPriority(int number) can be used to set priority of thread
	- new thread inherits priority of their parent class
- Deamon thead

- Locks and Condition
- Lock vs. Monitor
- ReentrantLock
	- In Java, `ReentrantLock` is an explicit mutual exclusion lock implementing the `Lock` interface. It allows the same thread to acquire a lock multiple times without deadlocking itself, maintaining a hold count that increments on acquisition and decrements on release. It is released only when the count hits zero
	- useful in recursive function call stack
- ReadWriteLock
	- ReadLock - more than 1 thread can acquire read lock
	- WriteLock - Only 1 thread can acquire the write lock
- StampedLock
	- Support Read/Write functionality like ReadWriteLock
	- Support optimistic lock functionality too
- SemaphoreLock
- Condition
	- await() = wait()
	- signal() = notify()

- Lock Free Concurrency (CAS)
	- Lock Based Mechanism
		- Synchronized
		- Reentrant
		- Stamped
		- ReadWrite
		- Semaphores
	- CAS operation (Compare And Swap)
		- AtomicInteger
		- AtomicBoolean
		- AtomicLong
		- AtormicReference
	- Compare And Swap
		- It's low level operation
		- it's atomic
		- all modern processor supports it
	- involves 3 main parameters
		- Memory location
		- Expected Value
		- New Value
	- Atomic = single or nothing
	- Concurrent collections


| Collection      | Concurrent Collection                     | Lock          |
| --------------- | ----------------------------------------- | ------------- |
| Priority Queue  | PriorityBlockingQueue                     | ReentrantLock |
| LinkedList      | ConcurrentLinkedDeque                     | CAS operation |
| Array Deque     | ConcurrentLinkedDeque                     | CAS operation |
| ArrayList       | CopyOnWriteArrayList                      | ReentrantLock |
| HashSet         | netKeySet method inside concurrentHashMap | Synchronized  |
| TreeSet         | Collections.synchronizedSortedSet         | Synchronized  |
| LikedHashSet    | Collections.synchronizedSet               | Synchronized  |
| Queue Interface | ConcurrentLinkedQueue                     | CAS operation |

- Thread Pool and ThreadPoolExecutor
	- Its collection of thread or workers which are available to perform the submitted tasks
	- once task completed, worker thread get back to thread pool and wait for new task to assign
	- threads can be reused
	- advantages of threadpool
		- Thread creation time can be saved
		- Overhead of managing thread lifecycle can be removed
		- Increased the perform
	- package java.util.concurrent

![[Pasted image 20260718155747.png]]

- ThreadPoolExecutor
	- It helps to create a customizable ThreadPool
	```
	public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliceTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, ThreadFactory threadFactory, RejectedExecutionHandler handler)
	```
- corePoolSize - Number of threads are initially created and keep in the pool, even if they are idle
- each parameter has meaning

![[Pasted image 20260718160419.png]]

- why you have choose corePoolSize as 2 and not 10 or 15 ?
	- Generally thread pool min and max size are depend on various factor like
		- CPU cores
		- JVM memory
		- Task Nature (CPU Intensive or I/O intensive)
		- Concurrency Requirement
		- Memory required to process a request
		- Throughput
	- its an iterative process to update the min and max values based on monitoring
	```
	Max. no of threads = No. of CPU core * (1 + Request waiting time / processing time)
	```

- Future, CompletableFuture and Callable
	- Future
		- Interface which represents the result of the Async task
		- means, it allow you to check if 
			- computation is complete
			- get the result
			- take care of exception if any etc.
		- methods - cancel, isCancelled, isDone, get, get(timeout, unit) etc
	- Callable
		- Callable represents the task which needs to be executed just like Runnable
		- But difference is
			- Runnable do not have any return type
			- Callable has the capability to return the value
	- CompletableFuture
		- Introduced in Java8
		- To help in Async Programming
		- we can consider it as an advanced version of future provides additional capability like chaining
		- supplyAsync()
		- thenApply() & thenApplyAsync()
			- apply a function to the result of previous async computation
			- return a new CompletableFuture Object
		- thenCompose() & thenComposeAsync()
			- chain together dependent async operations
		- thenAccept() & thenAcceptAsync()
			- generally end stage, in the chain of async operations
			- it does not return anything
		- thenCombine() & thenCombineAsync()
			- used to combine the result of 2 comparable future

- Fork/Join Pool, Single, Fixed & CachedPool
	- FixedThreadPoolExecutor
		- newFixedThreadPool method creates a thread pool executor with a fixed no. of threads
	- CachedThreadPoolExecutor
		- newCachedThreadPool method creates a thread pool that creates a new thread as needed (dynamically)
	- SingleThreadExecutor
		- newSingleThreadExecutor creates executor with just single worker thread
- WorkStealing Pool Executor
	- It creates a Fork-Join Pool Executor
	- Number of threads depends upon the available processors or we can specify in the parameter
	- There are 2 queues
		- submission queue
		- work-stealing queue for each thread (It's a Deque)
	- RecursiveTask & RecursiveAction
	- we can create Fork-Join pool using `newWorkStealingPool` method in ExecutorService
	- or by calling ForkJoinPool.commonPool() method

- ScheduledThreadPoolExecutor
	- shutdown
		- Initiates orderly shutdown of the ExecutorService
		- After calling 'shutdown' executor will not accept new task submission
		- already submitted tasks, will continue to execute
	- AwaitTermination
		- Its an Optional functionality, Return true / false
		- It is used after calling `Shutdown` method
		- Blocks calling thread for specific timeout period, and wait for ExecutorService shutdown
		- Return true if ExecutorService gets shutdown within specific timeout else false
	- shutdownNow
		- Best effort attempt to stop/interrupt the actively executing tasks
		- Halt the processing of tasks which are waiting
		- Return the list of tasks which are waiting execution
	- ScheduleThreadPoolExecutor helps to schedule the tasks
		- schedule(Runnable command, long delay, TimeUnit unit)
		- scheduleAtFixedRate(Callable\<V> callable, long delay, TimeUnit unit)
		- scheduleWithFixedDelay(Runnable command, long initialDelay, long period, TimeUnit unit)
		- scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit)

- VirtualThreads and ThreadLocal
	- ThreadLocal
		- ThreadLocal class provide access to Thread-Local variables
		- This `Thread-Local` variable hold the value of particular thread
		- each thread has its own copy of Thread-Local variable
		- We need only 1 object of ThreadLocal class and each thread can use it to set and get its own Thread-variable variable
		- Remember to clean up, if reusing the thread
	- Types of threads
		- Platform Threads
		- Virtual Threads
	- Virtual Thread
		- To get higher throughput not latency
		- Use virtual threads for I/O bound and Network-bound operations
		- don't perform CPU-heavy tasks on virtual threads (unless managed carefully)

- Lombok
	- Java library, which helps to reduce boilerplate code using annotations 
	- during compilation, it process the annotation and inject code into our Java classes
	- Lombok is compatible with Java starting from Java 6 and supports all later versions
	1. val and var
		- Instead of actually writing the type, we can use these as the type of local variable declaration
		- type will be inferred from the initializer expression
			- val as const
			- var as var 
		- @NonNull
			- Generates a null check statement
			- can be used on parameters of a method or constructor
		- @Getters and @Setters
			- generates the default getter and setter methods
		- @ToString
			- Used to generate "toString()" method
			- class name followed by parentheses containing fields (non-static) separated by commas
		- @NoArgsConstructor, @RequiredArgsConstructor, @AllArgsConstructor
		- @EqualsAndHashCode
		- @Data
			- Shortcut for @ToString, @EqualsAndHashCode, @Getter on all fields, @Setter on all non-final fields, @RequiredArgsConstructor
		- @Value
			- Immutable version of @Data
		- @Builder
		- @Cleanup
			- It ensures that given resource is automatically cleaned up before execution path exists the current scope

- SequencedCollection, SequencedSet and SequencedMap
	- A collection whose elements have a defined order
	- Implemented by List, Deque, and LinkedHashSet
	- Standardize "first", "last", and "reverse" operations across ordered collections and maps, eliminating collection-specific APIs and making generic programming much cleaner.

- Sealed Classes
- **Sealed classes/interfaces** let you define a **closed hierarchy** by explicitly listing which classes may extend or implement them
- public sealed class Payment
    permits CreditCardPayment,
            UpiPayment,
            CashPayment {
	}

- A sealed class must explicitly list its permitted subclasses.
- Every permitted subclass **must declare how open it is**.
- Every permitted subclass must be declared as **`final`** (stop inheritance), **`sealed`** (continue restricting), or **`non-sealed`** (reopen inheritance).
- sealed interface

- Switch statement
- pattern matching

- Records
	- It helps us to create immutable class in a short way
	- It is mostly designed to reduce boiler plate code for data carrying classes (like POJO)
- Text blocks with `"""<multi line string>"""`

- Optional
	- Methods generally return "null" which indicate value is not present and many times client forgot to add "null" check, which led to NullPointerException.
	- Introduced in Java8, to solve the above discussed i.e. null-return problem. API or methods, now can express their intention for ex: Optional\<User> as return type, tells client that return value may or may not exist.
	- few methods
		- isPresent()
		- ifPresent()
		- orElse()
		- orElseGet()
		- etc...
