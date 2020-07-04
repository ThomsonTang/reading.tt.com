## Thread Management

### Create and running a thread

 - Extending *Thread* class and overriding the *run()* method.
 - Building a class that implements *Runnable* interface and then creating an object of the *Thread* class passing the *Runnable* object as a parameter.
 
 1. Main thread: Every java program has at least one execution thread. When you run the program, the JVM runs this execution thread that calls the *main()* method in the program. 
 2. Another thread: When call the *start()* method of a *Thread* object, we are creating another execution thread.
 3. End: A java program ends when all its threads finish (more specifically, when all its no-daemon thread finish). If the initial thread(the "main thread") ends, the rest of the threads will continue their execution until they finish. If one of the threads use *System.exit()* instruction to end the execution of the program, all the thread end their execution.
 
> Only calling the start() method creates a new execution thread.

### Getting and setting thread information

There are some attributes:

- ID: store unique identifier for each Thread.
- Name: store the name of the Thread.
- Priority: store the priority of the Thread objects. priority between [1, 10]. Change the priority of thread is not recommended.
- Status: store the status of thread. In java, six states: [new, runnable, blocked, waiting, timed_waiting, terminated]

*You can't modify the ID or status of a thread. The Thread class doesn't implement the setId() and setStatus() method to allow their modification.*

### Interrupting a thread

The *Thread* class has an attribution that stores a boolean value indicating whether the thread has been interrupted or not.
When you call the *interrupt()* method of a thread, you set that attribute to true. The *isInterrupted()* only return the value of that attribution.

The static method, *interrupted()*, checks whether the current executing thread has been interrupted or not.

### Controlling the interruption of a thread

If the thread implements a complex algorithm divided into some methods, or it has method recursive calls, we can use a better mechanism to control the interruption of the thread. Java provides **InterruptedException** exception for this purpose.

### Sleeping and resuming a thread

Use the *sleep()* method of the *Thread* class to suspend the execution of the thread. When the sleeping time ends, the thread continues with its execution in the isntruction, after the *sleep()* method calls, when the JVM assigns them CPU time.   

Another possiblity is to use the *sleep()* method of an element of the *TimeUnit* enumeration. This method uses the *sleep()* method of the *Thread* class to put the current thread to sleep, but it receives the parameter in the unit that it  represents and converts it to milliseconds.

### Waiting for the finalization of a thread

In some situations, we will have to wait for the finalization of a thread. For this purpose, we can use the *join()* method of the *Thread* class. When we call this method using a thread object, it suspends the execution of the calling thread until the object called finishes its execution.

- join(long milliseconds)
- join(long milliseconds, long nanos)


### Creating and running a daemon thread

Java has a specifal kind of thread called *daemon* thread. These kinds of threads have very low priority and normally only executes when no other thread of the same program is running. When daemon threads are the only threads running in a program, the JVM ends the program finishing these threads.

### Processing uncontrolled exception in a thread
> **Checked Exception**: must be specified in the throws clause of a method or caught inside them. For example, *IOException*, *ClassNotFoundException*.

If this exception is thrown inside the `run()` method of a Thread object, we have to catch and treat, because the `run()` method doesn't accept a throws clause.

> **Unchecked Exception**: don't have to be specified or caught. For example, *NumberFormatException*.

If this exception is thrown inside the `run()` method of a *Thread* object, the default behavior is to write stack trace in console and exit the program. Fortunately, java provides a mechanism to catch and treat the unchecked exceptions thrown in a *Thread* object to avoid the program ending. There are steps below:

* Define an exception handler class to treat the unchecked exception. The class must implement **Thread.UncaughtExceptionHandler** and override the **uncaughtException()** method.

```
public class ExceptionHandler implements Thread.UncaughtExceptionHandler {
	public void uncaughtException(Thread t, Throwable e) {
    		//handle the exceptions
    	}
}
```

* Set the exception handler to the thread. 

```
 Thread thread = new Thread(task);
 thread.setUncaughtExceptionHandler(new ExceptionHandler());
```

When an uncaught exception is thrown in **Thread**, the JVM looks for three possible handlers for this exception:
- First, it looks for the handler set to the thread as the above example.
- If the handler doesn't exist, it then looks for **ThreadGroup** of the **Thread** objects.
- If it also doesn't exist, the JVM looks for the default uncaught exception handler.
If none of the handlers exist, the JVM writes the stack trace of the exception in the console and exit the program.