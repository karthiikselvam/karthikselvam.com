---
title: "Fundamentals of Multithreading"
description: "2023"
date: "2023-01-23"
tags:
- fundamentals
---

**1. Thread creation** : In Java, you can create a thread by either extending the Thread class or implementing the Runnable interface. Here's how you can do it:
* Extending the Thread class:
```java
public class MyThread extends Thread {
    public void run() {
        // code to be executed in this thread
    }
}
```
You can create an instance of the MyThread class and start the thread using the start() method:
```java 
MyThread myThread = new MyThread();
myThread.start();
```
* Implementing the Runnable interface:
```java
public class MyRunnable implements Runnable {
    public void run() {
        // code to be executed in this thread
    }
}
```
You can create an instance of the MyRunnable class and pass it to a Thread object's constructor:
```java
MyRunnable myRunnable = new MyRunnable();
Thread thread = new Thread(myRunnable);
thread.start();

```
In both cases, the run() method contains the code that will be executed when the thread is started. You should not call the run() method directly, but rather use the start() method to start the thread.

---
**2. Thread Termination** : 
Following are reasons to terminate a thread:
* Threads in a computer consume various resources, such as memory, kernel resources, CPU cycles, and cache memory.

* If a thread finished its work, but the application is still running we want to clean up the thread's resources.

* If a thread is misbehaving, we want to stop it.

* The application will not stop as long as at least one thread is still running.

In Java, you can terminate a thread by calling the interrupt() method on the thread object. This method sets a flag on the thread to indicate that it should stop executing. However, it is up to the thread's code to check for the interrupt flag and terminate gracefully.

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        while (!Thread.currentThread().isInterrupted()) {
            // do some work
        }
        System.out.println("Thread is terminating.");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start();

        // Interrupt the thread
        thread.interrupt();
    }
}

```

In this example, the MyThread class extends the Thread class and overrides the run() method to do some work in a loop. Inside the loop, the thread checks if it has been interrupted using the isInterrupted() method.

In the Main class, we create an instance of MyThread and start it. When the interrupt() method is called, the isInterrupted() method in the MyThread class will return true, causing the thread to exit the loop and terminate gracefully.

---
**3. Daemon Threads** : 
In Java, a daemon thread is a type of thread that runs in the background and does not prevent the Java Virtual Machine (JVM) from exiting when the program finishes execution.

Daemon threads are typically used for tasks that need to run continuously in the background, such as garbage collection or other system-level tasks. They are also commonly used in server applications to perform tasks like cleaning up old connections or handling periodic maintenance tasks.

To create a daemon thread in Java, you can use the setDaemon() method on a Thread object. For example, the following code creates a new thread and sets it as a daemon thread:
```java
Thread myThread = new Thread(new Runnable() {
    public void run() {
        // Do some background task here
    }
});
myThread.setDaemon(true);
myThread.start();

```

Once a thread is set as a daemon thread, you cannot change it back to a non-daemon thread.

---
**4. Joining Threads** : 

Joining threads in Java is done using the join() method, which allows one thread to wait for another thread to complete before continuing. Here's a simple example of how to use the join() method to join two threads:

```java
public class JoinExample {

    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(() -> {
            System.out.println("Thread 1 is running");
            try {
                Thread.sleep(2000); // simulate some work being done
            } catch (InterruptedException e) {
                System.out.println("Thread 1 was interrupted");
            }
            System.out.println("Thread 1 is finished");
        });

        Thread t2 = new Thread(() -> {
            System.out.println("Thread 2 is running");
            try {
                Thread.sleep(1000); // simulate some work being done
            } catch (InterruptedException e) {
                System.out.println("Thread 2 was interrupted");
            }
            System.out.println("Thread 2 is finished");
        });

        t1.start();
        t2.start();

        // wait for both threads to finish
        t1.join();
        t2.join();

        System.out.println("Both threads have finished");
    }
}
```

In this example, we create two threads t1 and t2 and start them. We then use the join() method to wait for both threads to finish before printing out a message indicating that both threads have finished.

When t1.join() and t2.join() are called, the main thread will block and wait until both t1 and t2 have finished executing. Once both threads have completed, the main thread will continue executing and print out the final message.

---

**5. Resource Sharing in Threads** 

In Java, resource sharing can be achieved using the concept of synchronized monitors. A synchronized monitor is a mechanism that allows only one thread to access a shared resource at a time.

To use synchronized monitors in Java, you can use the synchronized keyword to define a block of code that needs to be executed by only one thread at a time. The synchronized keyword can be applied to a method or a block of code.

Here's an example of using synchronized monitors to share a resource in Java:

```java
public class SharedResource {
    private int value;

    public synchronized void increment() {
        value++;
    }

    public synchronized int getValue() {
        return value;
    }
}

public class ResourceSharingExample {
    public static void main(String[] args) {
        SharedResource sharedResource = new SharedResource();

        // create multiple threads to access the shared resource
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                sharedResource.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                sharedResource.increment();
            }
        });

        // start the threads
        t1.start();
        t2.start();

        // wait for the threads to finish
        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // print the final value of the shared resource
        System.out.println("Value: " + sharedResource.getValue());
    }
}
```

In this example, the SharedResource class represents a shared resource that contains a single integer value. The increment() method and getValue() method are both marked as synchronized, which means that only one thread can access them at a time.

The ResourceSharingExample class creates two threads that access the shared resource using the increment() method. The threads run concurrently, but because of the synchronized monitors, only one thread can execute the increment() method at a time. The final value of the shared resource is printed at the end of the program, which should be 2000 in this case.

We also achieve some result using Object lock instead of synchronized monitors in Java.
```java
public class SharedResource {
    private int value;
    private final Object lock = new Object();

    public void increment() {
        synchronized (lock) {
            value++;
        }
    }

    public int getValue() {
        synchronized (lock) {
            return value;
        }
    }
}

public class ResourceSharingExample {
    public static void main(String[] args) {
        SharedResource sharedResource = new SharedResource();

        // create multiple threads to access the shared resource
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                sharedResource.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                sharedResource.increment();
            }
        });

        // start the threads
        t1.start();
        t2.start();

        // wait for the threads to finish
        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // print the final value of the shared resource
        System.out.println("Value: " + sharedResource.getValue());
    }
}
```

Using mutiple object locks threads can access the shared resources concurrently without interfering with each other.

```java
public class SharedResource {
    private int value1;
    private int value2;
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();

    public void incrementValue1() {
        synchronized (lock1) {
            value1++;
        }
    }

    public int getValue1() {
        synchronized (lock1) {
            return value1;
        }
    }

    public void incrementValue2() {
        synchronized (lock2) {
            value2++;
        }
    }

    public int getValue2() {
        synchronized (lock2) {
            return value2;
        }
    }
}

public class ResourceSharingExample {
    public static void main(String[] args) {
        SharedResource sharedResource = new SharedResource();

        // create multiple threads to access the shared resource
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                sharedResource.incrementValue1();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                sharedResource.incrementValue2();
            }
        });

        // start the threads
        t1.start();
        t2.start();

        // wait for the threads to finish
        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // print the final values of the shared resources
        System.out.println("Value1: " + sharedResource.getValue1());
        System.out.println("Value2: " + sharedResource.getValue2());
    }
}

```
In this example, the SharedResource class contains two object locks, lock1 and lock2, and two methods for each value, incrementValue1(), getValue1(), incrementValue2(), and getValue2(). The incrementValue1() method and getValue1() method use lock1 to synchronize access to value1, while the incrementValue2() method and getValue2() method use lock2 to synchronize access to value2.

The ResourceSharingExample class creates two threads that access the shared resources using incrementValue1() and incrementValue2() methods. Because two different object locks are used to synchronize access to two different values, the **threads can access the shared resources concurrently without interfering with each other**. The final values of the shared resources are printed at the end of the program.

---

**6. Reentrant lock**

A ReentrantLock is a synchronization mechanism in Java that provides a way to protect shared resources from simultaneous access by multiple threads. It is called "reentrant" because a thread that already holds the lock can acquire it again without blocking, unlike traditional synchronization using the synchronized keyword.

```java
import java.util.concurrent.locks.ReentrantLock;

public class SharedResource {
    private int value;
    private ReentrantLock lock = new ReentrantLock();

    public void incrementValue() {
        lock.lock();
        try {
            value++;
        } finally {
            lock.unlock();
        }
    }

    public int getValue() {
        lock.lock();
        try {
            return value;
        } finally {
            lock.unlock();
        }
    }
}

public class ResourceSharingExample {
    public static void main(String[] args) {
        SharedResource sharedResource = new SharedResource();

        // create multiple threads to access the shared resource
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                sharedResource.incrementValue();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                sharedResource.incrementValue();
            }
        });

        // start the threads
        t1.start();
        t2.start();

        // wait for the threads to finish
        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // print the final value of the shared resource
        System.out.println("Value: " + sharedResource.getValue());
    }
}
```
In this example, the SharedResource class contains a single ReentrantLock and two methods for incrementing and retrieving the value variable. The lock object is used to synchronize access to the value variable.

The ResourceSharingExample class creates two threads that access the shared resource using the incrementValue() method. Because a ReentrantLock is used to synchronize access to the shared resource, the threads can access the shared resource concurrently without interfering with each other.

ReentrantLock is like a special key that allows threads to take turns accessing a shared resource. And if a thread already has the key, it can use it again without waiting in line. It provides query methods for testing lock's internal state.

Example of using ReentrantLock and tryLock() method in Java:
```java
import java.util.concurrent.locks.ReentrantLock;

public class Example {
    public static void main(String[] args) {
        ReentrantLock lock = new ReentrantLock();

        // acquire the lock using tryLock() method
        boolean isLockAcquired = lock.tryLock();

        if (isLockAcquired) {
            try {
                // perform critical section operations here
                System.out.println("Lock acquired and critical section entered.");
            } finally {
                // release the lock
                lock.unlock();
                System.out.println("Lock released.");
            }
        } else {
            System.out.println("Could not acquire lock. Another thread is holding the lock.");
        }
    }
}
```

Example of using ReentrantReadWriteLock and tryLock() method in Java:

```java
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class Example {
    private Map<String, String> map = new HashMap<>();
    private ReadWriteLock lock = new ReentrantReadWriteLock();

    public void put(String key, String value) {
        lock.writeLock().lock(); // acquire write lock
        try {
            map.put(key, value); // perform write operation
        } finally {
            lock.writeLock().unlock(); // release write lock
        }
    }

    public String get(String key) {
        lock.readLock().lock(); // acquire read lock
        try {
            return map.get(key); // perform read operation
        } finally {
            lock.readLock().unlock(); // release read lock
        }
    }
}
```

In this example, we have a Map object that is accessed by multiple threads. To ensure thread safety and prevent race conditions, we use a ReadWriteLock to control access to the map.

The put() method acquires a write lock using the writeLock() method, performs a write operation on the map, and then releases the write lock using the unlock() method.

The get() method acquires a read lock using the readLock() method, performs a read operation on the map, and then releases the read lock using the unlock() method.

With this approach, multiple threads can simultaneously read from the map, but only one thread can write to the map at a time. This can significantly improve performance in scenarios where reads are much more frequent than writes.

---

**7. Volatile Keyword**

In Java, the volatile keyword is used as a modifier to indicate that a variable's value may be modified by multiple threads at the same time.

When a variable is marked as volatile, the JVM ensures that any write to that variable is immediately visible to other threads that may access it. This means that changes made to the variable by one thread will be immediately reflected in the value seen by other threads. Without the volatile keyword, there is no guarantee that changes made by one thread will be immediately visible to another thread, which can lead to hard-to-detect bugs.

The volatile keyword can be used with any primitive type, as well as with references to objects. However, it's important to note that using volatile does not provide atomicity, which means that if multiple threads try to modify the same variable at the same time, race conditions and inconsistencies can still occur.

In general, the volatile keyword should only be used when there is a specific need for multiple threads to access the same variable, and the programmer is sure that the volatile variable will be accessed in a safe and consistent way.

Here's an example to illustrate the use of volatile keyword in Java:

```java
public class Counter {
    private volatile int count;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

In this example, we have a Counter class with a private volatile integer field called count. The increment() method is used to increment the value of count by one, and the getCount() method returns the current value of count.

Without the volatile keyword, there is no guarantee that changes made to count by one thread will be immediately visible to another thread. However, since count is marked as volatile, the JVM ensures that any write to count is immediately visible to other threads that may access it.

Consider the following example usage of the Counter class by multiple threads:

```java
public class Main {
    public static void main(String[] args) {
        Counter counter = new Counter();

        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                counter.increment();
            }
        });

        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                counter.increment();
            }
        });

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Final count: " + counter.getCount());
    }
}
```

In this example, we create two threads, thread1 and thread2, which both increment the Counter object's count field 10,000 times. We then wait for both threads to complete using the join() method, and output the final value of count.

Without the volatile keyword on the count field in the Counter class, there is no guarantee that the final value of count will be 20,000. However, with count marked as volatile, we can be sure that changes made by one thread will be immediately visible to the other thread, and the final value of count will be 20,000.

--- 

**8. Deadlocks**
In Java, a deadlock occurs when two or more threads are blocked, waiting for each other to release the locks they hold. As a result, none of the threads can make progress and the program hangs.

Here's an example to illustrate how a deadlock can occur in Java:
```java
public class DeadlockExample {
    private Object lock1 = new Object();
    private Object lock2 = new Object();

    public void method1() {
        synchronized (lock1) {
            System.out.println("Acquired lock1 in method1");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {}
            synchronized (lock2) {
                System.out.println("Acquired lock2 in method1");
            }
        }
    }

    public void method2() {
        synchronized (lock2) {
            System.out.println("Acquired lock2 in method2");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {}
            synchronized (lock1) {
                System.out.println("Acquired lock1 in method2");
            }
        }
    }

    public static void main(String[] args) {
        final DeadlockExample example = new DeadlockExample();
        Thread thread1 = new Thread(new Runnable() {
            public void run() {
                example.method1();
            }
        });
        Thread thread2 = new Thread(new Runnable() {
            public void run() {
                example.method2();
            }
        });
        thread1.start();
        thread2.start();
    }
}
```

In this example, there are two methods, method1() and method2(), which each synchronize on a different lock. The main() method creates two threads that each call one of these methods.

Now, suppose that thread1 acquires lock1 and then calls method2(), which tries to acquire lock2. At the same time, thread2 has already acquired lock2 and is trying to acquire lock1. Both threads are blocked waiting for the other thread to release its lock, causing a deadlock.

Deadlock can occur in a concurrent system when the following conditions are met:
- Mutual Exclusion: At least one resource is held in a mutually exclusive mode, meaning only one thread can use it at a time.

- Hold and Wait: A thread is holding at least one resource and is waiting to acquire additional resources that are currently held by other threads.

- No Preemption: Resources cannot be preempted or taken away from threads that are holding them. The only way to release a resource is for the thread to voluntarily release it.

- Circular Wait: A circular chain of threads exists, where each thread is waiting for a resource that is held by the next thread in the chain. In other words, there is a cycle in the resource allocation graph.


To avoid deadlocks, you can use some techniques like:

- Acquire locks in a fixed order.

- Use timeouts when acquiring locks to avoid indefinitely waiting for a lock.

- Use tryLock() instead of synchronized blocks to acquire locks in a non-blocking way.

- Use higher-level concurrency utilities like java.util.concurrent classes, which handle synchronization and - locking automatically.

