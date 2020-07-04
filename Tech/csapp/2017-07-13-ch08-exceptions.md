
# Exception Control Flow

## Control Flow
> Processors do only one thing:
- From startup to shutdown, a CPU simply reads and executes(interprets) a sequence of instructions, one at a time.
- This sequence is the CPU’s **control flow (or flow of control)**
![][image-1]

## Altering the Control Flow
There are tow mechanisms for changing control flow, both react to changes in **Program state**:
- Jumps and branches
- Calls and return

But this is insufficient for a useful system, it’s difficult to react to changes in **System state**:
- data arrives from a disk or a network adapter
- instruction divides by zero
- user hits `Ctrl-C` at the keyboard
- system timer expires

> **System needs mechanisms for “exceptional control flow”**

## Exceptional Control Flow
- Exists at all levels of a computer system
- Low level mechanisms
	- Exceptions: change in control flow in response to s system event(i.e., change in system state)
	- Combination of hardware and OS software
- Higher level mechanisms
	- Process context switch
	- Signals
	- Nonlocal jumps: `setjmp()`/`longjmp()`
	- Implemented by either:
		- OS software (context switch and signals)
		- C language runtime library (nonlocal jumps)

## Exceptions
- An exception is a transfer of control to the OS in response to some event (i.e., change in processor state)
![][image-2]

- Examples: div by 0, arithmetic overflow, page fault, I/O request completes, Ctrl-C

### Exception Handing
> Exceptions can be difficult to understand because handing then involves close cooperation between hardware and software.

![][image-3]

- Each type of event has a unique exception number `K`
- `K` = index into exception table
- Handler K is called each time exception k occurs

### Classes of Exceptions
Exception can be divided into four classes: **interrupts, traps, faults,** and **aborts**.

#### Asynchronous Exceptions (Interrupts)
- **Interrupts**: interrupts occur asynchronously as a result of signals from I/O devices that are external to the processor. 
	- Caused by events external to the processor
	- indicated by setting the processor’s interrupt pin
	- Handler returns to “next” instruction
- Examples:
	- I/O interrupts
		- hitting `Ctrl-C` at the keyboard
		- arrival of a packet from a network
		- arrival of data from a disk
	- Hard reset interrupt
		- hitting the reset button
	- Soft reset interrupt
		- hitting `Ctrl-Alt-Delete` on a PC

#### Synchronous Exceptions
- Caused by events that occur as a result of executing an instruction:
	- **Traps**:
		- Intentional
		- Examples: *system calls*, breakpoint traps, special instructions
		- Returns control to “next” instruction
	- **Faults**:
		- Unintentional but possibly recoverable
		- Examples: page faults (recoverable), protection faults (unrecoverable), floating point exceptions
		- Either re-executes faulting (“current”) instruction or aborts
	- **Aborts**:
		- unintentional and unrecoverable
		- Examples: parity error, machine check
		- Aborts current program


# Processes
## Definition
> When we run a program on a modern system, we are presented with the illusion that our program is the only one currently running in the system. Our program appears to **have exclusive use of both the processor and the memory**. The processor appears to execute the instructions in our program, one after the other, without interruption. Finally, **the code and data of our program appear to be the only objects in the system’s memory**. These illusions are provided to us by the notion of a process.

 A **process** is an instance of a running program.
- One of the most profound ideas in computer science
- Not the same as “program” or “processor”

### Process provides each program with two key abstractions:
- Logical control flow
	- Each program seems to have exclusive use of the CPU
- Private virtual address space
	- Each program seems to have exclusive use of main memory

### How are these illusions maintained?
- Process executions interleaved (multitasking) or run on separate cores
- Address spaces managed by virtual memory system

## Concurrent Processes
> A logical flow whose execution overlaps in time with another flow is called a **concurrent** flow, and the two flows are said to run concurrently.
> The general phenomenon of multiple flows executing concurrently is know as **concurrency**. The notation of a process taking turns with other processes is also known as **multitasking**. Each time period that a process executes a portion of its flow is called a **time slice**. Thus, multitasking is also referred to as **time slicing**.

- Two processes run concurrently (are concurrent) if their flows overlap in time
- Otherwise, they are **sequential**

### User View of Concurrent Processes
- Control flows for concurrent processes are physically disjoint in time
- However, we can think of concurrent processes are running in parallel with each other.
- If two flows are running concurrently on different processor cores or computers, then we say that they are **parallel flows**, that they are running in parallel, and have parallel execution.

### Private Address Space
>  A process provides each program with its own private address space.  This space is private in the sense that a byte of memory associated with a particular  address in the space cannot in general be read or written by any other process.

Although the contents of the memory associated with each private address space is different in general, each such space has the same general organization.

![][image-4]

### Context Switching
- Processes are managed by a shared chunk of OS code called the **kernel**
	- Important: **the kernel is not a separate process, but rather runs as part of some user process**
- Control flow passes from one process to another via a **content switch**





[image-1]:	https://user-images.githubusercontent.com/980216/28248942-b9996e16-6a7f-11e7-9b6e-c735fbbf1d6e.png "physical control flow"
[image-2]:	https://user-images.githubusercontent.com/980216/28253954-04459c9a-6adb-11e7-84d8-c410ac127264.png
[image-3]:	https://user-images.githubusercontent.com/980216/28253964-288c2fc4-6adb-11e7-8c5e-e670782a13d2.png
[image-4]:	https://user-images.githubusercontent.com/980216/28343288-2a24459a-6c4f-11e7-9a76-b02838d8bbe9.png