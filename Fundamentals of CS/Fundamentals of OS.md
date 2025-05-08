

- #### Why an OS?
	- The operating system is Software
	- Abstract layer over Hardware
	- Apps talk to the OS
	- Most OSs are general-purpose
    - Scheduling
	- CPU / Memory and IO
    - Idea: Job scheduling software that's all
    - OS APIs abstract the hardware
    - Can you build your app without an OS?
    - OS shouldn’t be a black box
    - Kernel manages resources
    - CPU
	    - Central Processing Unit
	    - Consists of cores
	    - Each core has a clock speed
	    - Executes machine-level instructions
	    - Fast caches
    - Memory
	    - RAM - Random Access Memory
	    - Fast but volatile
	    - Store processes, states, and data
	    - Limited
	    - Slower than CPU Cache
    - Storage
	    - Persisted storage
	    - HDD, SSD
	    - Slower than memory
	- Network
	    - TCP IP offload engine
	    - Communicates with other hosts
	    - NIC - Network Interface Controller
	    - Protocol implementations
    - Kernal
	    - The core of the OS
	    - Manages the resources
	    - The OS is more than the kernel
			- GUI, command line, UI, and tooling    
			- E.g., top is a tool that extracts and processes
			- Distros are all about this extra tooling   
	- File System
	    - Storage is primarily blocks of bytes
	    - We like to work on files
	    - The file system is an abstraction
	    - LBA - logical block addressing
	    - How are files stored on disk
	    - btrfs, ext4, fat32, NTFS, tmpfs
	- Program vs Process
	    - A program is the compiled executable
	    - Process is an instance of the program
	    - Process is a program in motion
	    - A program is a process at rest
	    - Execution file format
    - User Space vs Kernel Space
    	- User space
		    - Browser
		    - Postgres
	    - Kernel space
		    - Kernel code
		    - Device drivers
		    - TCP/IP stack
		- Isolated (io_uring kind of broke that rule)
	- Process management    
	- Device drivers
    - System calls
  

- #### Anatomy of a Process

- Program vs. Process
    - Process is a Program in motion
    - Code is compiled and linked for a CPU, and calls Program.
	- Static linking vs. Dynamic linking    
	- DLLs    
	- Only works on that CPU architecture
- Process
	   - When a program is run, we get a process
    - Process lives in memory
    - Uniquely identified with an id
    - Instruction pointer/program counter
    - Process control block (PCB)
- Code -> Assembly -> Machine Code
- Demo: Spin a process in Linux
	- gcc -S test.c test.s    
	- gcc -g test.c -o test
	 - gdb test
	    - Start
		- -n   

- Simple process execution

	- Machine code is often read from bottom to top, from low address to high address
	- Program counter steps, line-by-line instruction execution from memory and registry.
    - PC points to the current/next instructions
    - Fetch, Load, Execute cycle!

- The stack
	   - The stack is used for function calls
    - Stack pointer
    - Base pointer (Frame pointer)
    - Program counter
    - Previous base pointer
    - Return address 
	- Link register (LR)
- Process execution with Stack
- Data Section
	- Fixed size, Global and static variables, all functions share
    - Read and Write
    - Watch out for concurrency
- The Heap
    - Large, Dynamic place for memory allocations
	- Stores large data
	- Remain until explicitly removed
	- All functions can access
	- Dynamic, grows low to high
	- malloc, free, new
	- Pointers
		- Points to a memory address in the heap
		- A pointer can in stack, data or heap
		- Stores the address of first byte
		- Pointer type helps determine size
		- Heap is random
		- Stack space limited
		- Cache locality in stack
	- What is escape analysis ??
		- compiler optimization technique
	- Program break
		- where the process ends
		- Points to the top of the heap
		- Not recommended now its replaced with mmap
	- Slab memory allocation 

- Process demo :-

```
cat /proc/<pid>/maps 

sudo cat /proc/204/maps
```

- #### Memory Management

	- SRAM, DRAM, SDRAM, DDR Ram, Virtual Memory
	- 

- #### Inside the CPU
- #### Process Management
- #### Storage Management
- #### Socket Management
- #### More OS Concepts
