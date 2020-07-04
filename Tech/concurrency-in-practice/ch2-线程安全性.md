
## 术语卡
- 术语：对象的状态（the state of an object)
- 印象：我们常说的对象的状态一般是指**保存在状态变量（比如实例变量或静态域）中的数据**。当然了，某个对象的状态可能包含了其他对象的实例变量或静态域。而且一个对象的状态还包括那些**能影响它的外部可见行为的数据**。
- 例子：`HashMap`对象的状态不仅存储在`HashMap`对象本身，而且还存储在许多的`Map.Entry`对象中。
- 出处：《Java Concurrency in Practice》第二章

## 术语卡
- 术语：共享和可变（Shared and Immutable）
- 印象：共享意味着一个变量可以被多个线程同时访问；可变意味着变量的值在其生命周期内是可以变化的。这指的是在程序中使用对象的方式，而不是对象所实现的功能。
- 例子：
	- 类的静态属性（static fields）可以被该类的所有对象实例共享
	- Java中不可变一般都是通过关键字`final`来修饰的。Java中的`String`类的对象就是典型的不可变对象。
- 出处：《Java Concurrency in Practice》第二章

## 技巧卡
- 如果多个线程同时访问同一个可变状态的变量，但是没有使用合适的同步，那么程序就有可能出错。有三种办法可以解决：
	- 不要在多个线程之间共享这个状态变量。
	- 把状态变量修改为不可变的。
	- 访问这个状态变量时使用同步。
- 在设计线程安全的类时，良好的面向对象的技术——封装性、不可变性以及明确的不变性规范等都是很好的辅助手法。

## 术语卡
- 术语：线程安全性
- 印象：线程安全的关键在于**正确性**，具体讲就是，在多线程的环境下，类表现出的行为与其规范是完全一致的。当多个线程访问某个类的时候，这个类总能表现出正确的行为，那么它就是线程安全的。
	> 当多个线程访问某个类时，不管运行时环境采用何种调度方式或者这些线程将如何交替执行，并且在主调代码中不需要任何额外的同步或协同，这个类都能表现出正确的行为，那么就将这个类称之为线程安全的。
	如果某个类在单线程环境下都是不正确的，那么它肯定不是线程安全的。
	> 线程安全的类本身在内部封装了必要的同步机制，因此客户端无须进一步采取同步措施。
- 例子：一个无状态的Servlet

	```java
	@ThreadSafe
	public class StatelessFactorizer implements Servlet {
	    public void service(ServletRequest req, ServletResponse resp) {
	        BigInteger i = extractFromRequest(req);
	        BigInteger[] factors = factor(i);
	        encodeIntoResponse(resp, factors);
	    }
	}
	```
	> 无状态对象一定是线程安全的。大多数Servlet都是无状态的，这大大降低了在实现Servlet线程安全性时的复杂性，只有当Servlet在处理请求时需要保存一些信息，线程安全性才会成为一个问题。

## 术语卡
- 术语：竞态条件（Race Condition）
- 印象：在并发编程中，由于不恰当的执行时序而出现不正确的结果，称之为竞态条件。
- 例子：
	- 有状态的Servlet，其中的`++count`是一个`Read-Modify-Write`的操作序列，这个操作不是**原子性**的，是**非线程安全**的。

		```java
		@NotThreadSafe
		public class UnsafeCountingFactorizer implements Servlet {
		    private long count = 0;
		    public long getCount() {
		        return count;
		    }
		    public void service(ServletRequest req, ServletResponse resp) {
		        BigInteger i = extractFromRequest(req);
		        BigInteger[] factors = factor(i);
		        ++count;
		        encodeIntoResponse(resp, factors);
		    }
		}
		```
	- `Check-then-Act`，「先检查后执行」的操作序列，这是延迟初始化中常见的一种竞态条件。
		```java
		@NotThreadSafe
		public class LazyInitRace {
		        private ExpensiveObject instance = null;
		        public ExpensiveObject getInstance() {
		            if (instance == null) {
		                instance = new ExpensiveObject();
		            }
		            return instance;
		        }
		}
		```
	- 与大多数并发错误一样，竞态条件并不总是会产生错误，还需要某种不恰当的执行时序。
	- 生活中也有竞态条件，例如和朋友相约在星巴克相见。
- 出处：《Java Concurrency in Practice》第二章——线程安全性——原子性


## 术语卡
- 术语：复合操作
- 印象：「竞态条件」部分提到的`Read-Modify-Write`和`Check-then-Act`两种操作序列本质上都应该是**原子性**的，这种操作统称为**复合操作**，即**包含了一组以原子方式执行的操作以确保线程安全性**。
- 例子：
	- **加锁机制**：这是Java用于确保原子性的内置机制。
	- 使用现有的线程安全的类：比如原子操作类`AtomicInteger`、`AtomicLong`，或线程安全容器类`ConcurrentHashMap`等。
		```java
		@ThreadSafe
		public class CountingFactorizer implements Servlet {
		        private final AtomicLong count = new AtomicLong(0);
		        public long getCount() {
		            return count.get();
		        }
		        public viod service(ServletRequest req, ServletResponse resp) {
		            BigInteger[] i = extractFromRequest(req);
		            BigInteget[] factors = factor(i);
		            count.incrementAndGet();
		            encodeIntoResponse(resp,factors);
		        }
		}
		```
	- 在实际编程中，应尽可能地使用现有的线程安全对象，比如`AtomicLong`等。

## 术语卡
- 术语：内置锁
- 印象：
	- Java提供了一种内置锁的机制来实现原子操作，这就是我们熟悉的**同步代码块**，用关键字`synchronized`来修饰。
	- 每个Java对象都可以用作一个实现同步的锁，这些锁被称为**内置锁（intrinsic lock）**。
	- 获得内置锁的唯一方法就是**进入由这个锁保护的同步代码块或方法**。
	- 内置锁是一种互斥锁，这就意味着同一时刻最多只能有一个线程持有这个锁。当线程B试图获取一个由线程A持有的锁时，它必须等待或者阻塞，直到线程A释放了这个锁。
	- 受内置锁保护的代码块，因为同一时刻只能有一个线程访问，所以这种操作是原子性的，是线程安全的。
	- 内置锁虽然保证了线程安全性，但是有时候会带来性能问题。
- 例子：
	- 同步代码块：
		```java
		synchronized(lock) {
		        //访问或修改由锁保护的共享状态
		}
		```

- 同步方法：
	```java
	public synchronized void method() {
	        //访问或修改由锁保护的共享状态
	}
	```


## 术语卡
- 术语：可重入
- 印象：
	- 当某个线程视图访问一个其他线程持有的锁时，就会被阻塞。但内置锁具有可重入性，所以一个线程试图获取一个已经由他自己持有的锁时，就会成功获取到。这就是内置锁的**可重入性**。
	- 「重入」意味着获取锁的操作粒度是「线程」，而不是「调用」。
- 例子：如果内置锁不是可重入的，那么下面的代码将发生死锁。
	```java
	public class Widget {
	        public synchronized void doSomething() {...}
	}
	public class LoggingWidget extends Widget {
	        public synchronized void doSomething() {
	            System.out.println(toString() + ": calling doSomething...");
	            super.doSomething();
	        }
	}
	```

## 反常识卡
- 常识：加锁可以让受其保护的代码路径以串行的方式来访问。但一种常见的错误认为，只有在写入共享变量的时候才需要使用同步。
- 反常识：
	- 如果用同步来协调对某个变量的访问，那么在访问这个变量的所有位置上都需要使用同步。而且，当使用锁来协调对某个变量的访问时，在访问变量的所有位置上都要使用**同一个锁**。
	- 虽然大多数类都将内置锁用做一种有效的加锁机制，但对象的内置锁与其状态之间没有内在的关联，
	- 当获取与对象关联的锁时，并不能阻止其他线程访问该对象，某个线程在获得对象的锁之后，只能阻止其他线程获得同一个锁。
	- 一种常见的加锁约定是，**将所有的可变状态都封装在对象内部，并通过对象的内置锁对所有访问可变状态的代码路径进行同步，使得在该对象上不会放生并发访问**。许多线程安全类都使用了这种方法，比如`Vector`和其他的同步集合类。
	- 并非所有数据都需要锁的保护，只有被多个线程同时访问的可变数据才需要通过锁来保护。
	- 对于每个包含多个变量的不可变性条件，其中涉及的所有变量都需要由同一个锁来保护。
- 例子：
	- `Vector`类
	- `ConcurrentHashMap`

## 反常识卡
- 常识：既然同步可以避免竞态条件的问题，那么为什么不在每个方法声明时使用关键字`synchronized`？
- 反常识：
	- 试着想想，如果不加区别的滥用`synchronized`，那么有可能导致程序中出现过多的同步。
	- 有时候如果只是将方法申明为同步方法，并不足以确保对对象的复合操作及时原子的。**如果要把多个操作合并为一个复合操作，还是需要额外的加锁机制**。
	- 过多的同步又会导致**活跃性问题**和**性能问题**。
- 例子：如下代码使用`Vector`：
	```java
	if (!vector.contains(element)) {
	        vector.add(element);
	}
	```
	上述代码中，虽然`contains`和`add`两个方法都是原子方法，但是如上这个「如果不存在就添加」的操作，还是存在竞态条件的。

## 术语卡
- 术语：不良并发程序
- 印象：对于有的应用程序而言，可同时调用的数量，不仅受到可用处理资源的限制，还受到应用程序本身结构的限制。
- 例子：
	- `UnsafeCachingFactorizer`中将Servlet的`service`方法声明为`synchronized`方法，这样每次只有一个线程可以执行，从本质上来讲背离了`Servlet`框架的初衷，即Servlet需要能同时处理多个请求。
	- 不过好的一点在于，我们可以通过将同步代码块缩小范围，这样既能确保Servlet的并发性，又能维护线程安全性。
		查看示例代码：[CacheFactorizer.java][1]
	- 上述示例中，我们已经不再使用`AtomicLong`类型了，而是使用了简单类型`long`，这是因为**我们已经使用了同步代码块来构造原子操作，而如果两种机制都使用不仅让代码看上去很混乱，不统一，更重要的是在性能可能带来不良影响**。
	- 通常，在简单性和性能之间存在着相互制约的因素。当实现某个同步策略时，一定不要盲目地为了性能而牺牲简单性（这有可能破坏线程安全性）。
	- 当执行时间较长的计算或者可能无法快速完成的操作时（例如，网络I/O，控制台I/O），一定不要持有锁。


[1]:	https://raw.githubusercontent.com/ThomsonTang/Java-Review/master/concurrency-in-practice/src/main/java/threadsafety/CachedFactorizer.java