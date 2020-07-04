
## 术语卡——Critical Section

## 反常识卡——Lock机制的好处
- 常识：Java中最常用的同步方式就是使用`synchronized`同步代码块，因为这种方式使用起来比较简单。
- 反常识：`synchronized`使用起来确实很简单，但是Java还提供了另一种通过机制，可以使用`Lock`接口和它的实现类（常用的是`ReentrantLock`）。使用`Lock`机制有着比`synchronized`方式明显的好处。
- 例子：
	- 同步代码块的组织结构更灵活。使用`synchronzed`块时，其结构相对比较固定，但是使用`Lock`接口则要灵活的多，而且还可以实现一些比较复杂临界区操作。
	- `Lock`接口提供了一些额外的函数，这些函数的功能是`synchronized`所不具有的。比如`tryLock()`方法。这个方法会尝试获取锁，返回的结果是一个`Boolean`类型的值，表示是否有别的线程正在执行这个锁保护的代码。`synchronized`则不具有这样的功能，因为线程在尝试进入同步代码块的时候，如果获取不到内置锁，就会阻塞并一直等待，知道获取到内置锁。
	- `Lock`接口还允许读写操作相互分离，允许多个读操作和一个写操作同时存在。
	- `Lock`接口相比`synchronized`拥有更好的性能。
	- `ReentrantLock`还允许**递归调用**。
	- 使用`Lock`的时候一定要格外小心避免**死锁**的发生。因为经常会发生因忘记执行`unlock()`方法而导致死锁。所以最好的记得在`finally`块中执行`unlock()`方法。

## 术语卡——读写锁
- 术语：ReadWriteLock、ReentrantReadWriteLock
- 印象：
	- 该类拥有两个锁，一个用于写操作，一个用于读操作
	- 写操作允许多个线程同时执行，但是读操作只能有一个线程执行
	- 当一个线程执行写操作时，其他线程不能执行读操作
- 例子：

## 术语卡—— Condition
- 术语：Condition
- 印象：
	- `Condition`接口对`java.lang.Object`类中的监视器方法（`wait()/notify()/notifyAll()`）进行了提炼和扩展，使得每个对象拥有了更多的类似等待的操作方法，并且这些方法可以和任意的`Lock`接口的实现类组合使用。
	- `Lock`是`synchronized`的替代者，`Condition`是`Object`监控方法的替代者。
	- 一个锁通常可能会和一个或者多个condition相关。这些condition都声明在`Condition`接口中。这些condition的目的是允许线程拥有对锁的控制并且能够检查这些条件的正确与否，如果条件是false的那么线程将会阻塞直到被其他线程唤醒。