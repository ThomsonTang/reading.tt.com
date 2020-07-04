# Processes

## Process Concept

A system consists of a collection of processes:

- operating-system processes executing system code
- user processes executing user code.

> job == user programs == tasks == processes

## Operations on Process

### Process Creation

- the tree of processes
- process identifier (or _pid_)

When a process create a new process, two possibilities for execution exist:

1. The parent continues to execute concurrently with its children.
2. The parent waits until some or all its children have terminated.

There are also two address-space possibilities for the new process:

1. The child process is a duplicate of the parent process (it has same program or data as parent.)
2. The child process has a new program loaded into it.

### Process Termination

A parent process may terminate the execution of one of its children for a variety of reasons, such as these:

- The child has exceeded its usage of some of the resources that it has been allocated. (To determine whether this has occurred, the parent must have a mechanism to inspect the state of its children.)
- The task assigned to the child is no longer required.
- The parent is existing, and the operating system does not allow a child to continue if its parent terminates.

Some systems do not allow a child to exist if its parent has terminated.  
A parent process may wait for the termination of a child process by using the `wait()` system call.  
When a process terminates, its resource are deallocated by the operating system.

## Interprocess Communication

Cooperating processes require an **interprocess communication** mechanism that will allow them to exchange data and information. There are two fundamental models of interprocess communication:

- shared memory
- message passing

![interprocess-communication][image-1]

Message passing is useful for exchanging smaller amounts of data, because no conflicts need be avoided. Message passing is apse easier to implement in a distributed system than shared memory.  
Shared memory can be faster than message passing, since message-passing systems are typically implemented using system calls and thus require the more time-consuming task of kernel intervention. In shared-memory systems, system calls are required only to establish shared-memory regions. Once shared-memory is established, all accesses are treated as routine memory accesses, and no assistance from the kernel is required.

### Shared-Memory Systems

## Words Explanation

1. contemporary: belonging to the present time; modern
2. compartmentalise: to divide things into separate groups, especially according to what type of things they are.

[image-1]: http://img1.ph.126.net/C7GTYYrn2lOc1ZAhFKtQ1Q==/6619238120095273249.png
