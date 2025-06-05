

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
	- What is Memory ?
		- Stores data
		- Volatile
			- RAM - Random access memory
		- Non-Volatile
			- ROM - Read Only Memory
	- SRAM 
		- Static RAM, Complex, expensive but fast
		- 1 bit -> 1 flip flop -> 6 transistors
		- constant power
		- Used it CPU caches, SSDs
	- DRAM
		- Dynamic RAM, cheaper, slower
		- 1 bit -> 1 capacitor, 1 transistor
		- Capacitors, lose their state
		- need to be refreshed
	- Double Data Rate RAMs
	- DDR4 vs DDR5 SDRAM
	- burst
	- prefetch buffer io
	- channels
	- DRAM internals
		- DIMM, Bank, Rows, Columns, Cells (1 cell 1 bit)
	- MMU - Memory management unit
	- Read from memory to CPU cache / RAM will happen via 64 bytes block not as a single instruction
	- Sense amplifier is write ahead logging concept in RAM concept
	- Data types are aligned
		- 1, 4 or 8 bytes 
		- Certain sizes are placed in specific addresses
	- Memory access takes 50 - 100 ns
	- Virtual Memory
		- Limitations of physical memory
			- Fragmentation
				- One space, memory must be contiguous
				- External vs. Internal fragmentation
				- Fixed size block allocation
			- Shared Memory
				- Shared libraries
					- Most processes uses libraries
					- OS loads the library code once
					- Map the virtual page to the library physical code
					- Libc
					- /proc/[pid]/maps
				- CoW - copy on write concept
			- Isolation
			- Large programs
				- Swaps
		- fixed block size call it paging
		- page size is often 4kb
		- Virtual memory and fragmentation
		- Page Tables
		- MMU & TLB
	- DMA
		- Direct Memory Access
		- Data from network / disk must pass through CPU (Peripherals Read)
		- DMA allows direct access from network / disk to RAM, works with directly with physical address since there is no MMU available from CPU.
		- IOMMU (allows IO) with virtual address
	- Virtual Memory space is dedicated to process but threads share the virtual memory space.

- #### Inside the CPU
	- Basic components
		- ALU (Arithmetic logic unit)
		- CU (Control Unit)
		- MMU (Memory management Unit)
		- Registers
		- Caches (L1, L2, L3)
		- Bus
	- 



- #### Process Management
- #### Storage Management
- #### Socket Management
- #### More OS Concepts


#### Hands On
- Memory management and analysis with c code in Linux (http://geeksforgeeks.org/structure-member-alignment-padding-and-data-packing/)
- Linux file system and its architecture
- DMA signals & DMA Attacks
- 
