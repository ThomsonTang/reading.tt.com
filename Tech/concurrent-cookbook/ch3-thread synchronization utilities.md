
## 术语卡——thread synchronization
- 术语：thread synchroziation
- 印象：
	- 线程同步的基本机制：Basic Thread Synchronization
		- the `synchronized` keyword
		- the `Lock` interface and its implementations: `ReentrantLock`、`ReentrantReadWriteLock.ReadLock`、`ReentrantReadWriteLock.WriteLock`
	- 线程同步工具：Thread Synchronization Utilities
		- `Semaphores`: 信号量是一种计数器，用于控制对一个或多个共享资源的访问。
		- `CountDownLatch`: 闭锁机制允许一个线程一直等待至若干操作结束。
		- `CyclicBarrier`: 栅栏允许多个线程在一个共同点上的同步。
		- `Phaser`: Phaser将并发任务分为多个阶段并控制其执行，所有线程必须在进入下一阶段时完成当前阶段的工作。
		- `Exchanger`: Exchanger在两个线程之间提供了一个数据交换的点。


## 术语卡——闭锁
- 术语：CountDownLatch
- 印象：`CountDownLatch`主要用来实现让线程等待直到若干事件或操作发生以后再执行。
	- `CountDownLatch`类有三个基本的元素：
		- 初始值，这个用来表示CountDownLatch类要等待的事件或操作的数量。
		- `await()`方法，这个方法是由等待操作事件结束后再执行的线程来调用的。
		- `countDown()`方法，这个方法是操作事件在完成它们的执行后调用的。
	- `CountDownLatch`一旦创建就无法再次初始化或者修改其内部的计数器，唯一能做的就是调用`countDown()`方法来改变它的值。当这个计数器的值达到`0`的时候，所有调用了`await`的方法都将立即返回，此后再调用`countDown()`将不会有任何作用。
	- 相对于其他同步方法，`CountDownLatch`有两点区别需要注意：
		- `CountDownLatch`机制并不是用来保护共享资源或临界区的。它是用来同步那些有不同任务要执行的一个或多个线程。
		- `CountDownLatch`只能使用一次，一旦用完就不能再使用了，只能重新创建一个。
- 例子：「视频会议参会」的例子，`VideoConference.java`

## 术语卡——栅栏
- 术语：栅栏CyclicBarrier
- 印象：
	- `CyclicBarrier`初始化时会传入一个整数，表示将要在一个特定点上进行同步的线程数。当其中一个线程到达这个特定点时，就会调用`await()`方法来等待其他线程，此时`CyclicBarrier`类将会阻塞该线程直到其他线程到达。当最后一个线程调用了`await()`方法后，它将会唤醒所有等待的线程，并开始执行后续的任务。
	- `CyclicBarrier`有一个很有意思的优点，你可以传入一个额外的`Runnable`对象作为初始化参数，`CyclicBarrier`会在所有线程都已到达公共点时将其作为一个线程来执行。这个特性使得`CyclicBarrier`非常适合使用「分而治之」的手段来实现并行任务的处理。
	- 重置一个`CyclicBarrier`对象：`CyclicBarrier`和`CountDownLatch`有着许多的共同点，不过`CyclicBarrier`对象可以重置，但`CountDownLatch`却不行。我们可以通过`reset()`方法对`CyclicBarrier`对象进行重置。一旦执行了重置操作，那么所有在`await()`方法中等待的线程将会收到一个`BrokenBarrierException`异常，程序可以捕获这个异常，并在捕获后执行一些额外的操作，比如重新执行或从中断点恢复等。
- 例子：「分组统计矩阵中随机数出现次数」的例子。

## 术语卡——Phaser
- 术语：Phaser
- 印象：