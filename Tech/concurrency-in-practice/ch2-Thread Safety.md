Writing thread-safe code is, at its core, about managing access to *state*, and in particular to *shared, mutable state*.

Whether an object needs to be thread-safe depends on whether it will be accessed from multiple threads. This is a property of how the object is **used** in a program, not what it **does**. Making an object thread-safe requires using synchronization to coordinate access to its mutable state, failing to do so could result in data corruption and other undesirable consequences.

> If multiple threads  access the same mutable state variable without appropriate synchronization, your program is broken. There are three ways to fix it:
> - Don't share the state variable across threads;
> - Make the state variable **immutable**;
> - Use **synchronization** whenever accessing the sate variable.

The less code that has access to a particular variable, the easier it is to ensure that all of it uses the proper synchronization, and the easier it is to reason about the conditions under which a given variable might be accessed.

> When designing thread-safe classes, good object-oriented techniques -- encapsulation, immutability, and clear specification of invariants -- are your best friends. 

It is always a good practice first to make your code right, and then make it fast.

## 2.1 What is Thread Safety?

> A class is thread-safe if it behaves correctly when accessed from multiple threads, regardless of the scheduling or interleaving of the execution of those threads by the runtime environment, and with no additional synchronization or other coordination on the part of the calling code.

No set of operations performed sequentially or concurrently on instances of a thread-safe class can cause an instance to be in an invalid state.

> Thread-safe classes encapsulate any needed synchronization so that clients need not provide their own.

> Stateless objects are always thread-safe.

It is only when servlets want to remember things from one request to another that the thread safety requirement becomes an issue.

## 2.2 Atomicity

The possibility of incorrect results in the presence of unlucky timing is so important in concurrent programming that it has a name: **a race condition**.

### Race Conditions

The most common type of race condition is **check-then-act**, where a potentially stale observation is used to make a decision on what to do next.

The common idiom that uses **check-then-act** is **lazy initialization**.

**Read-modify-write** operations, like incrementing a counter, define a transformation of an object's state in terms of its previously state.

Like most concurrency errors, race conditions don't always result in failure: some unlucky timing is also required. But race conditions can cause serious problems.

### Compound Actions

> Operation A and B are atomic with respect to each other if, from the perspective of a thread executing A, when another thread execute B, either all of B has executed or none of it has. An *atomic operation* is one that is atomic with respect to all operations, including itself, that operate on the some state.

We refer collectively to **check-then-act** and **read-modify-write** sequences as **compound actions**: sequences of operations that must be executed atomically in order to remain thread-safe.

> Where practical, use existing thread-safe objects, like AtomicLong, to manage your class's state. It is simpler to reason about the possible states and sate transitions for existing thread-safe objects than it is for arbitrary state variables, and this makes it easier for maintain and verify thread safety.

## Locking

The definition of thread safety requires that invariants be preserved regardless of timing or interleaving of operations in multiple threads.




