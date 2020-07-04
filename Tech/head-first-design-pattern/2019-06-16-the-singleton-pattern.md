# the Singleton Pattern

## What use

There are many objects we only need one of: **thread pools**, **caches**, **dialog boxes**, **objects that handle preferences and registry settings**, **objects used for logging**, **objects that act as device drivers to devices like printers and graphics cards**.

## Global Variable vs Singleton Pattern

global variable object might be created when application begins, and if the object is resource intensive and the application never ends up using it. With the singleton pattern, we can create our objects only when they are needed.

## Single Pattern defined

the **Singleton Pattern** ensures a class has only one instance, and provides a global point of access to it.

## Dealing with multithreading

```java
public class Singleton {
        private static Singleton uniqueInstance;

        //other fields

        private Singleton() {}

        public static synchronized Singleton getInstance() {
                if (null == uniqueInstance) {
                    uniqueInstance = new Singleton();
                }
        }

        //other methods
}
```

## synchronization is expensive, a few options to improve multithreading

- Do nothing if the performance of getInstance() isn't critical to application

- Move to an eagerly created instance rather than a lazily created one

```java
public class Singleton {
        private static Singleton uniqueInstance = new Singleton();
        //other fields
        private Singleton() {}
        public static synchronized Singleton getInstance() {
            return uniqueInstance;
        }
        //other methods
}
```

the JVM guarantees that the instance will be created before any thread accesses the static uniqueInstance variable.

- Use "double-checked locking" to reduce the use of synchronization in getInstance()

```java
public class Singleton {
    // the volatile keyword ensures that multiple threads handle the uniqueInstance variable
    // correctly when it is being initialized to the Singleton instance.
    private volatile static Singleton uniqueInstance;

    //other fields

    private Singleton() {}

    public static Singleton getInstance() {
        if (null == uniqueInstance) {
            synchronized (Singleton.class) {
                if (null == uniqueInstance) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }

    //other methods
}
```