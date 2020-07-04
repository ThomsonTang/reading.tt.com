
# Virtual Memory

## 术语卡：Virtual Memory

> **Virtual Memory** is an elegant interaction of hardware exceptions, hardware address translation, main memory, disk files, and kernel software that **provides each process with a large, uniform, and private address space**.

### 印象
**VM** provides three important capabilities:
- It uses main memory efficiently by treating it as **a cache for an address space stored on disk, keeping only the active areas in main memory, and transferring data back and forth between disk and memory as needed**.
- It simplifies memory management by **providing each process with an uniform address space**.
- It protects the address space of each process from corruption by other processes.

## Physical and  Virtual Addressing
- **Physical Addressing**:

![][image-1]

> Used in “simple” systems like embedded micro-controllers in devices like cars, elevators, and digital picture frames.

- **Virtual Addressing**:

![][image-2]

> - Used in all modern servers, desktops, and laptops
> - One of the great ideas in computer science
> - **Address translation**: the task of converting a virtual address to a physical one. Dedicated hardware on the CPU chip called the `memory management unit(MMU)` translates virtual addresses on the fly, using a look-up table stored in main memory whose contents are managed by the operating system.

## Address Spaces
- **Linear address space**: Ordered set of contiguous non-negative integer addresses: 
	`{0, 1, 2, ...}`
- **Virtual address space**: In a system with virtual memory, the CPU generates virtual addresses from an address space of `N = 2^n` addresses.
	`{0, 1, 2, ..., N-1}`
- **Physical address space**: A system also has physical address space that corresponds to the `M` bytes of physical memory in the system:
	`{0, 1, 2, ..., M-1}`

> - Clean distinction between data (bytes) and their attributes (address)
> - Each object can now have multiple addresses
> - Every byte in main memory: **one physical address, one (or more) virtual address**

### Why Virtual Memory?
- **Uses main memory efficiently**: use DRAM as a cache for the parts of a virtual address space
- **Simplifies memory management**: Each process gets the same uniform linear address space
- **Isolates address space**: 
	- one process can’t interfere with another’s memory
	- User program cannot access privileged kernel information

## VM as a Tool for Caching
- **Virtual memory** is an array of N contiguous bytes stored on disk.
- The contents of the array on disk are cached in **physical memory(DRAM cache)**.
	- These cache blocks are called **pages (size is P = 2^p bytes)**
	> VM systems handle this by partitioning the virtual memory into fixed-sized blocks called **virtual pages (VPs)**. Each virtual page is P=2^p bytes in size. Similarly, physical memory is partitioned into **physical pages (PPs)**, also P bytes in size. (Physical pages are also referred to as page frames.)

![][image-3]

Three disjoint subsets of virtual pages:
- **Unallocated**: Pages that have not yet been allocated (or created) by the VM system.
- **Uncached**: Allocated pages that are not cached in physical memory.
- **Cached**: Allocated pages that are currently cached in physical memory.

### DRAM Cache Organization
> the bottom line is that the organization of the DRAM cache is driven entirely by the enormous cost of misses.(归根结底，DRAM缓存的组织结构完全是由巨大的不命中开销驱动的）
- DRAM cache organization driven by the enormous miss penalty: 
	- DRAM is about `10x` slower than SRAM(the L1, L2 and L3 cache)
	- Disk is about `10,000x` slower than DRAM

- Consequences
	- Large page (block) size: typically `4-8 KB`, sometimes `4MB`
	- Fully associative:
		- Any VP can be placed in any PP
		- Requires a “large” mapping function - different from CPU caches
	- Highly sophisticated, expensive replacement algorithms
		- Too complicated and open-ended to be implemented in hardware
	- Write-back rather than write-through （总是使用写回，而不是直接写）

### Page tables
> As with any cache, the VM system must have some way to determine if a virtual page is cached somewhere in DRAM. These capabilities are provided by **a combination of operating system software, address translation hardware in the MMU(memory management unit), and a data structure stored in physical memory known as a page table that maps virtual pages to physical pages**.  
> A page table is an array of page table entries(PTEs) that maps virtual pages to physical pages.

![][image-4]

The indications of each PTE which consists of **a valid bit** and **an n-bit address field**:
- Valid bit is set, the address field indicates the start of the corresponding physical page in DRAM where the virtual page is cached.
- Valid bit is not set, then a null address indicates that the virtual page has not yet been allocated.
- Valid bit is not set, the address points to the start of the virtual page on disk.

### Page Hits
> Page hit: reference to VM word that is in physical memory (DRAM cache hit).

![][image-5]

### Page Faults
In virtual memory parlance, a DRAM cache miss is known as a **page fault**. 
> Page fault: reference to VM word that is not in physical memory (DRAM cache miss).

- Page miss causes page fault (an exception)
- Page fault handler selects a victim to be evicted (here VP4)
- Offending instruction is restarted: page hit !

![][image-6]

### Locality to the Rescue Again !
- Virtual memory works because of **locality**
- At any point in time, programs tend to access a set of active virtual pages called the **working set**.
	- Program with better temporal locality will have smaller working sets
- If (working set size \< main memory size)
	- Good performance for one process after compulsory misses
- If (SUM(working set sizes) \> main memory size)
	- Thrashing: Performance meltdown where pages are swapped (copied) in and out continuously

## VM as a Tool for Memory Management
> Virtual memory was still a useful mechanism because it greatly simplified memory management and provided a natural way to protected memory.
> In fact, operating system provide a separate page table, and thus a separate virtual address space, for each process.

In particular, VM simplifies linking and loading, the sharing of code and data, and allocating memory to applications.
- **Simplifying linking**: A separate address space allows each process to use the same basic format for its memory image, regardless of where the code and data actually reside in physical memory.
- **Simplifying loading**: Virtual memory also makes it easy to load executable and shared object files into memory.
- **Simplifying sharing**: Separate address spaces provide the operating system with a consistent mechanism for managing sharing between user processes and the operating system itself.
- **Simplifying memory allocation**: Virtual memory provides a simple mechanism for allocating additional memory to user processes.

### Key idea: each process has its own virtual address space
- it can view memory as a simple linear array
- Mapping function scatters addresses through physical memory
	- Well chosen mapping simplify memory allocation and management

![][image-7]

### Memory allocation
- Each virtual page can be mapped to any physical page
- A virtual page can be stored in different physical pages at different times

### Sharing code and data among processes
- Map virtual pages to the same physical page

### Simplifying Linking and Loading
- Liking
	- Each program has similar virtual address space
	- Code, stack, and shared libraries always start at the same address

- Loading
	- `execve()` allocates virtual pages for **.text** and **.data** sections = creates PTEs marked as invalid
	- The **.text** and **.data** sections are copied, page by page, on demand by the virtual memory system

![][image-8]





[image-1]:	https://user-images.githubusercontent.com/980216/27769332-fd160220-5f59-11e7-8053-c4ba88a00817.png
[image-2]:	https://user-images.githubusercontent.com/980216/27769349-3609574e-5f5a-11e7-92b7-dc86966ea54d.png
[image-3]:	https://user-images.githubusercontent.com/980216/28491808-c950e6d2-6f29-11e7-8cee-faaafed7d464.png "virtual pages and physical pages"
[image-4]:	https://user-images.githubusercontent.com/980216/28491825-1550ed84-6f2a-11e7-9b8d-6e02ae56ae37.png "Page table"
[image-5]:	https://user-images.githubusercontent.com/980216/28491837-36c0db3c-6f2a-11e7-98c3-9b7a9b153d5e.png "Page hit"
[image-6]:	https://user-images.githubusercontent.com/980216/28491847-5033c520-6f2a-11e7-9941-fce183b4b943.png
[image-7]:	https://user-images.githubusercontent.com/980216/28499674-bf2468f4-6fed-11e7-8950-e1b903c8ae28.png
[image-8]:	https://user-images.githubusercontent.com/980216/28499678-d63cbb4a-6fed-11e7-9fa0-0fe815e18487.png