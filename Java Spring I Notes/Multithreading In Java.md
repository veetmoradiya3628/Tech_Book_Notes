
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
