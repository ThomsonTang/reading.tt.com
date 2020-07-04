
## 反常识卡——关键字`synchronized`的作用
- 常识： 同步代码块和同步方法可以确保以原子的方式执行操作，所以有人认为关键字`synchronized`只能用于实现原子性或者「临界区」（Critical Section）。
- 反常识：同步其实还有另外一个重要的特性，即**内存可见性（Memory Visiblity）**。
	- 可见性：在多线程编程中，我们不仅希望防止某个线程在使用对象状态的同时被另一个线程修改该状态，而且希望确保当一个线程修改了对象状态后，其他线程能够看到这种状态的变化。
	- 重排序：在没有同步的情况下，编译器、处理器以及运行时等都可能对操作的执行顺序进行一些意想不到的调整，达到优化处理的目的。但是这就导致我们在判断内存操作的执行顺序时，很难得出正确的结论。
	- 不过幸运的是，有一种简单的方法可以避免这些复杂的问题：**只要有数据在多个线程之间共享，就使用正确的同步**。
- 例子：在没有同步的情况下共享变量：
	```java
	public class NoVisibility {
			private static boolean ready;
			private static int number;

			private static class ReaderThread extends Thread {
				public void run() {
					while(!ready) {
						Thread.yield();
					}
					System.out.println(number);
				}
			}
			
			public static void main(String[] args) {
				new ReaderThread().start();
				number = 42;
				ready = true;
			}
	}
	```
	上面的示例程序看起来会输出`42`，但事实上很可能输出`0`，或者根本无法终止。这是因为在代码中没有使用足够的同步机制，因此无法保证主线程写入的`ready`值和`number`值对于读线程来说是可见的。

## 术语卡——失效数据
- 术语：失效数据
- 印象：在缺乏同步的程序中可能会产生一种错误的结果：「失效数据」。这通常是因为访问变量时没有使用同步，导致某个线程获得的是该变量的一个已经失效的值。
- 例子：可变整数类：
	```java
	public class MutableInteger {
			private int value;
			
			public int get() { return value; }
			public void set(int value) {
				this.value = value;
			}
	}
	```
	上面实例中的`MutableInteger`类是非线程安全的类：**如果某个线程调用了`set`，那么另一个正在调用的`get`的线程可能会看到更新后的 `value` 值，也可能看不到。**
- 对 `set` 和 `get` 等方法进行同步，可以使 `MutableInteger` 成为一个线程安全的类，但仅对`set`进行同步是不够的，调用`get`的线程仍然会看见失效值。

## 反常识卡——最低安全性的适用范围
> 最低安全性：当线程在没有同步的情况下读取变量时，可能会得到一个失效值，但至少这个失效值是由之前的某个线程设置的值，而不是一个随机值。这种安全性保证被称为**最低安全性（out-of-thin-air-safety）**。

- 常识：最低安全性适用于绝大多数变量。
- 反常识：最低安全性存在一个例外：**非volatile类型的64位数值变量**。
- 例子：
	- Java内存模型要求：变量的读取和写入操作都必须是原子操作，但对于非volatile类型的long和double变量，JVM允许将64位的读操作或写操作分解为两个32位的操作。
	- 这就可能导致在读取一个这样的变量时，如果对该变量的读操作和写操作在不同的线程中执行，那么很可能对读取到某个值的高32位和另一个值的低32位。
	- 在多线程程序中使用共享且可变的long型和double等类型的变量也是不安全的，除非用关键字`volatile`来声明它们，或者用锁保护起来。

## 问答卡——加锁与可见性
- 问：为什么在访问某个共享且可变的变量时要求所有线程在同一个锁上进行？
- 答：
	- 这么做是为了确保某个线程写入该变量的值对于其他线程来说都是可见的。否则，如果一个线程在未持有正确锁的情况下读取某个变量，那么读到的可能是一个失效值。
	- **加锁并不只是为了互斥行为，它还有关于内存可见性**。为了保证其他线程能够看到共享可变变量的最新值，所有的读操作和写操作的线程都必须在同一个锁上同步。

## 术语卡——volatile变量
- 术语：volatile 变量
- 印象：
	- Java提供的一种与`synchronized`相比较弱的同步机制
	- `volatile`可以确保变量的更新可以及时通知给其他线程
	- 当一个变量声明为`volatile`后，编译器和运行时就会注意到该变量是共享的，并且对该变量的操作不会和其他内存操作一起重排序。
	- `volatile`变量不会被缓存在寄存器或者其他CPU不可见的地方，因此对`volatile`变量的读取始终获取的是被其他线程修改过的最新的值。
	- 访问`volatile`变量的时候是不会执行加锁操作的，因此不会引起执行线程的阻塞。这使得`volatile`是比`synchronized`更轻量级的原因。
- 例子：如何使用`volatile`变量？
	- 仅当`volatile`变量能简化代码的实现并且能够验证你的同步策略时，才应该使用它们。如果在验证正确性时需要对可见性进行复杂的判断，那么就不要使用`volatile`变量。`volatile`变量的正确使用方式包括：
		- 确保它们自身状态的可见性
		- 确保它们所引用对象的状态的可见性
		- 标志一些重要的程序生命周期事件的发生（例如，初始化或关闭）
	- `volatile`的典型用法：**检查某个状态标记以判断是否退出循环**。
		```java
		volatile boolean asleep;
		...
				while(!asleep) {
					countSomeSheep();
				}
		```
	- 加锁机制不仅能确保可见性还能保证原子性，但是`volatile`只能确保可见性。
	- 当且仅当满足以下所有条件时，才应该使用`volatile`变量：
		- 对变量的写入操作不依赖变量的当前值，或者你能确保只有单个线程更新变量的值。
		- 该变量不会与其他状态变量一起纳入不变性条件中。
		- 在访问变量时不需要加锁。

## 术语卡——发布与逸出
- 术语：
	- 发布（publish）: 发布一个对象的意思是指，使对象能够在当前作用域之外的代码中使用。
	- 逸出（escape）：当某个不应该发布的对象被发布时，这种情况就被成为逸出。
- 印象：
	- 许多情况下，我们要确保对象及其内部的状态不被发布。
	- 如果在对象构造之前就发布该对象，就会破坏线程安全性。
	- 当发布某个对象时，可能会间接的发布其他对象。
	- 当发布一个对象时，在该对象的非私有域中引用的所有对象同样会被发布。
	- 封装能够使得对程序的正确性进行分析变得可能，并使得无意中破坏设计约束条件变得更难。
- 例子：安全的对象构造过程：
	- **不要在构造过程中使`this`引用逸出**。如果`this`引用在构造过程中逸出，那么这种对象就被认为是不正确的构造。
	- 在构造函数中创建线程并没有错，但最好不要立即启动它，而是通过一个`start`或`initialize`方法来启动。
	- 如果想在构造函数中注册一个事件监听器或启动线程，那么可以使用一个私有的构造函数和一个公共的工厂方法，从而避免不正确的构造过程。
		```java
		public class SafeListener {
		    private final EventListener eventListener;

		    private SafeListener() {
		        this.eventListener = new EventListener() {
		            public void onEvent(Event event) {
		                doSomething(event);
		            }
		        };
		    }

		    public static SafeListener newInstance(EventSource source) {
		        SafeListener listener = new SafeListener();
		        source.registerListener(listener.eventListener);
		        return listener;
		    }
		```