# Java-Multi-Threading
# Why we need Threads?

## 1. Responsiveness

Example of poor responsiveness - waiting for customer support

## 2. Performance

We can create an illusion that multiple tasks are executing concurrently, for high scale service (parallelism)

## Single-Threaded vs. Multi-Threaded Applications

In a single-threaded application, there's only one stack and instruction pointer for the entire process.

In a multi-threaded application, each thread has its own instruction pointer (which points to the address of the next instruction to be executed) and stack (a memory region where local variables are stored and passed into functions).

## OS fundamentals

context switching - scheduling thread, saving the state of one thread and load another thread to the CPU. scheduling for CPU.

## Too many Threads?

thrashing - spending more time in management (context switching) than real productive work

- thread consumes relatively less resources than a process
- context switch btw thread of same process in easier than context switch btw threads of different process
- algorithms like FIFO, Shortest Job First, Epochs (giving time for thread to execute), etc. are used for the context switch
- dynamic priority = static priority + bonus
    - static priority is assigned by the programmer
    - bonus assigned by the OS, can be negative used for handling starvation
    - this is used for giving time in epoch

## When to prefer multi-threaded architecture?

1. if the tasks share a lot of data
2. threads are much faster to create and destroy
3. shorter context switching in case of same process

## When to prefer multi-process architecture?

1. security and stability are of highest importance
2. tasks are unrelated to each other.

## Thread creation with -  java.lang.Runnable

```java
public class Main {
    public static void main(String[] args) {
        // Create thread using an object of a class implementing Runnable
        ThreadRunnable threadRunnable = new ThreadRunnable();
        Thread thread1 = new Thread(threadRunnable);
        
        // Create thread using an anonymous class
        Thread thread2 = new Thread(() -> {
            System.out.println("Running Thread! " + Thread.currentThread().getName());
        });
        
        // Start both threads
        thread1.start();
        thread2.start();
    }
}

// Class implementing Runnable interface
class ThreadRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Running Thread! " + Thread.currentThread().getName());
    }}
}
```

## Exception Handling inside thread

when there is an exception inside thread that we don’t want to handle with try catch we can use

UnCaughtExceptionHandler.

```java
public class Main {
	public static void main(String args[]) {
	
		// creating thread that throws a runtime exception
		Thread thread = new Thread(() -> {
			throw new RuntimeException();
		});
		
		// setting exception handler for that thread
		thread.setUncaughtExceptionHandler(new Thread.UncaughtExceptionHandler() {
			@Override
			public void uncaughtException(Thread t, Throwable e) {
				System.out.println(t.getName() + " error Occured " + e.getMessage());
			}
		});
		
		// starting thread
		thread.start();

		// creating another thread which also throws runtime exception
		Thread thread2 = new Thread(() -> {
			throw new RuntimeException();
		});
		
		// setting exception handler for the thread
		thread2.setUncaughtExceptionHandler(new ExceptionHandler());
		
		// starting thread
		thread2.start();
		
		// staic method - that will set exception handler for all threads
		Thread.setDefaultUncaughtExceptionHandler(new ExceptionHandler());
		
		// creating new thread
		Thread thread3 = new Thread(()->{
			throw new RuntimeException();
		});
		
		// creating new thread
		Thread thread4 = new Thread(()->{
			throw new RuntimeException();
		});
		
		// starting both thread
		thread3.start();
		thread4.start();
	}
}

// creating exception handler with uncaughtExceptionHandler interface
class ExceptionHandler implements Thread.UncaughtExceptionHandler {
	@Override
	public void uncaughtException(Thread t, Throwable e) {
		System.out.println(t.getName() + " error occured " + e.getMessage());
	}
}

```

## Thread creation with - java.lang.Thread

```java
public class Main{

		public static void main(String arg[]){
				// creating thread anonymous class
				Thread thread1 = new Thread(()->{
						System.out.println("Thread started "+Thread.currentThread().getName());
				});
				
				// creating thread
				Thread thread2 = new TempThread();
				
				// starting thread
				thread1.start();
				thread2.start();
		}
}

// class extending Thread
class TempThread extends Thread{
			@Override
			public void run(){
					System.out.println("Thread started "+Thread.currentThread().getName());
			}
}
```

## Thread termination & daemon thread

Threads consume resources

- memory and kernel resources
- CPU cycles and cache memory

we want to stop a thread

- if a thread finished its work
- if a thread is misbehaving

by default, application won't stop as long as at least one thread is still running

```java
public class Main{
		public static void main(String args[]){
				// creating a thread that expect an interrupt
				Thread thread = new Thread(()->{
						try{
							   System.out.println("Thread running "+Thread.currentThread().getName());
								 Thread.sleep(1000);
						}catch(InterruptedException e){
								 System.out.println("Thread got Interrupted "+e.getMessage());
						}
				});
				
				// creating a thread that doesnt expect an interrupt
				Thread thread2 = new Thread(()->{
							System.out.println("Thread running "+Thread.currentThread().getName());
				});
				
				// starting and interrupting thread - catch block get executed
				thread.start();
				thread.interrupt();
				
				// starting and interrupting thread  - no effect
				thread2.start();
				thread2.interrupt();
		}
}
```

Daemon Thread

1. background task, that should not block our application from terminating
2. code in a worker thread is not under our control and we do not want it to block our application from terminating.

```java
public class Main{
		public static void main(String args[]){
				// creating thread
				Thread thread1 = new Thread(()->{
						System.out.println("Thread running");
				});
				thread1.start();
				System.out.println(thread1.isDaemon());
				// setting to a daemon thread after starting throw IllegalThreadStateException
				try{
					thread1.setDaemon(true);
				}catch(IllegalThreadStateException e){
					System.out.println("Exception "+e.getMessage());
				}
				
				// creating  a thread
				Thread thread2 = new Thread(()->{
						System.out.println("Thread running");
				});
				thread2.setDaemon(true);
				System.out.println(thread2.isDaemon());
				thread2.start();
		}
}
```

## Thread co-ordination with join ()

usually, thread co-ordination is unpredictable.

when we want to wait a thread for another thread, we can make use of join method

if I call threadA.join() on threadB - threadB will wait for threadA to complete.

```java
public class Main {
	public static void main(String args[]) {
		// creating thread
		Thread thread = new Thread(() -> {
			try {
				System.out.println("Thread Started " + Thread.currentThread().getName());
				Thread.sleep(1000);
				System.out.println("Thread Completed " + Thread.currentThread().getName());
			} catch (InterruptedException e) {
				System.out.println("Exception " + e.getMessage());
			}
		});
		// starting thread
		thread.start();

		// main thread will wait for this thread to complete
		// join(long millis) and join(long millis, int nanos)
		try {
			thread.join();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		// creating second thread
		Thread thread1 = new Thread(() -> {
			try {
				System.out.println("Thread Started " + Thread.currentThread().getName());
				Thread.sleep(1000);
				System.out.println("Thread Completed " + Thread.currentThread().getName());
			} catch (InterruptedException e) {
				System.out.println("Exception " + e.getMessage());
			}
		});

		thread1.start();	
	}
}
```

## Performance & optimizing for latency

1. High speed trading systems - performance measured in units of time
2. YouTube video player - performance measured as precision and accuracy of frame rate
3. Machine learning - performance measured is through put.

- Latency - Time required for completion of a task, measured in units of time
- Throughput - The number of tasks completed in a given period, measured tasks/time unit

when a single task taking latency of T is reduced to N sub task, the latency is reduced to T/N that is 

performance increased by a factor of N

## What is N, when dividing a task to N sub tasks?

On a general-purpose computer, N typically equals the number of cores. This assumes that:

1. All threads are in a runnable state at all times.
2. There are no other processes running that consume significant CPU resources.

Hyper-Threading

- Allows a single physical core to act as two logical cores
- Improves multitasking and parallel processing

Virtual cores versus physical cores

- Physical cores are actual hardware components
- Virtual cores are logical processors created through Hyper-Threading

Hardware units shared among virtual cores

- Cache memory
- Execution units
- Memory bus

## Inherent cost of parallelization and aggregation

1. Break task into multiple task
2. Thread creation, passing tasks to Thread
3. Time btw thread.start() to thread get scheduled
4. Time until the last thread finishes and signals
5. Time until the aggregating thread runs
6. Aggregation of the sub results into a single artifact

Points to consider regarding the inherent costs of parallelization and aggregation:

- Overhead of synchronization mechanisms (e.g., locks, semaphores) when threads need to coordinate
- Potential for resource contention, such as multiple threads competing for CPU time or memory access
- Increased complexity in debugging and maintaining parallel code
- Potential for race conditions or deadlocks if not properly managed
- Memory overhead for creating and managing multiple threads
- Potential diminishing returns as the number of threads increases beyond the available CPU cores

Tasks can be categorized based on their parallelizability:

1. Fully Parallelizable Tasks: Can be divided into independent subtasks that can be executed concurrently.
2. Sequential Tasks: Cannot be broken down and must be executed in a specific order.
3. Partially Parallelizable Tasks: Contain both parallel and sequential components.
4. Data-Dependent Tasks: Parallelizability depends on the nature and structure of the data being processed.

## Throughput

Throughput is the number of tasks completed in a given period, measured in tasks per time unit. It's a critical metric for assessing system performance, especially in high-volume processing scenarios.

To improve throughput:

- Run tasks in parallel to process more simultaneously
- Optimize individual task performance to reduce processing time
- Balance workload across available resources efficiently
- Minimize bottlenecks and wait times in the system

## Thread Pooling

Thread pooling is a technique where a fixed number of threads are created and reused for executing multiple tasks, instead of creating new threads for each task. This approach offers several advantages:

- Improved performance by reducing thread creation overhead
- Better resource management and control over concurrent execution
- Prevents thread explosion in high-load scenarios

While implementing a thread pool from scratch can be complex, Java provides built-in implementations through the java.util.concurrent package:

- ExecutorService: An interface that represents a thread pool
- Executors: A utility class with factory methods for creating different types of thread pools

Common thread pool implementations in Java include:

- Fixed Thread Pool: Maintains a constant number of threads
- Cached Thread Pool: Creates new threads as needed, but reuses idle threads when available
- Scheduled Thread Pool: Allows scheduling of tasks to run after a delay or periodically
- Single Thread Executor: Uses a single worker thread to execute tasks sequentially

Example of creating and using a thread pool:

```java
// Import necessary classes for thread pool and concurrency
import java.util.concurrent.*;
import java.util.List;
import java.util.ArrayList;
import java.util.Random;

public class Main {

    private static final int POOL_SIZE = 4;
    private static final int TASK_COUNT = 5;

    public static void main(String[] args) {
        // Create lists to hold our tasks
        List<Runnable> runnableTasks = createRunnableTasks();
        List<Callable<Integer>> callableTasks = createCallableTasks();

        // Create a fixed-size thread pool
        ExecutorService fixedThreadPool = Executors.newFixedThreadPool(POOL_SIZE);

        try {
            // Execute Runnable tasks
            for (Runnable task : runnableTasks) {
                fixedThreadPool.execute(task);
            }

            // Execute Callable tasks and get Future objects
            List<Future<Integer>> futures = fixedThreadPool.invokeAll(callableTasks);

            // Process results from Callable tasks
            for (Future<Integer> future : futures) {
                try {
                    System.out.println("Callable result: " + future.get());
                } catch (ExecutionException e) {
                    System.out.println("Callable task threw an exception: " + e.getCause());
                }
            }

        } catch (InterruptedException e) {
            System.out.println("Main thread was interrupted: " + e.getMessage());
        } finally {
            // Shutdown the thread pool
            shutdownAndAwaitTermination(fixedThreadPool);
        }

        // Demonstrate other types of ExecutorServices
        ExecutorService virtualThreadPool_1 = Executors.newVirtualThreadPerTaskExecutor();
        ExecutorService virtualThreadPool = Executors.newThreadPerTaskExecutor(Thread.ofVirtual().factory());
        ExecutorService cachedThreadPool = Executors.newCachedThreadPool();
        ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(POOL_SIZE);

        // Use ScheduledExecutorService to schedule a task
        scheduledThreadPool.schedule(() -> System.out.println("Scheduled task executed"), 2, TimeUnit.SECONDS);

        // Shutdown additional thread pools
        shutdownAndAwaitTermination(cachedThreadPool);
        shutdownAndAwaitTermination(scheduledThreadPool);
    }

    private static List<Runnable> createRunnableTasks() {
        List<Runnable> tasks = new ArrayList<>();
        for (int i = 0; i < TASK_COUNT; i++) {
            final int taskId = i;
            tasks.add(() -> {
                try {
                    System.out.println("Runnable task " + taskId + " started: " + Thread.currentThread().getName());
                    Thread.sleep(1000);
                    System.out.println("Runnable task " + taskId + " completed: " + Thread.currentThread().getName());
                } catch (InterruptedException e) {
                    System.out.println("Runnable task " + taskId + " was interrupted");
                }
            });
        }
        return tasks;
    }

    private static List<Callable<Integer>> createCallableTasks() {
        List<Callable<Integer>> tasks = new ArrayList<>();
        for (int i = 0; i < TASK_COUNT; i++) {
            final int taskId = i;
            tasks.add(() -> {
                try {
                    System.out.println("Callable task " + taskId + " started: " + Thread.currentThread().getName());
                    Thread.sleep(1000);
                    int result = new Random().nextInt(100);
                    System.out.println("Callable task " + taskId + " completed: " + Thread.currentThread().getName());
                    return result;
                } catch (InterruptedException e) {
                    System.out.println("Callable task " + taskId + " was interrupted");
                    throw e;
                }
            });
        }
        return tasks;
    }

    // Utility method to shutdown an ExecutorService
    private static void shutdownAndAwaitTermination(ExecutorService pool) {
        pool.shutdown(); // Disable new tasks from being submitted
        try {
            // Wait a while for existing tasks to terminate
            if (!pool.awaitTermination(5, TimeUnit.SECONDS)) {
                List<Runnable> droppedTasks = pool.shutdownNow(); // Cancel currently executing tasks
                System.out.println("Pool did not terminate, dropped tasks: " + droppedTasks.size());
                // Wait a while for tasks to respond to being cancelled
                if (!pool.awaitTermination(5, TimeUnit.SECONDS)) {
                    System.out.println("Pool did not terminate");
                }
            }
        } catch (InterruptedException ie) {
            // (Re-)Cancel if current thread also interrupted
            pool.shutdownNow();
            // Preserve interrupt status
            Thread.currentThread().interrupt();
        }
    }
}
```

This enhanced version of the thread pool example demonstrates several important concepts and functionalities:

- Use of both Runnable and Callable tasks to showcase different ways of submitting work to a thread pool.
- Implementation of a fixed-size thread pool using ExecutorService.
- Proper shutdown procedure for ExecutorService to ensure all tasks are completed or cancelled.
- Error handling for interrupted threads and exceptions thrown by tasks.
- Demonstration of different types of thread pools: FixedThreadPool, CachedThreadPool, and ScheduledThreadPool.
- Use of Future objects to retrieve results from Callable tasks.
- Implementation of a utility method for graceful shutdown of thread pools.

## Apache JMeter

A tool used for measuring performance of application

- java platform automation tool
- doesn't require to write code
- it will calculate the throughput by sending request

## Data Sharing Between Threads

In Java multi-threading, there are two main memory regions: stack and heap. Understanding these is crucial for effective thread management and performance optimization.

### Stack

- Method calls are stored
- Arguments are passed
- Local variables are stored
- Each thread has its own stack

The combination of stack and instruction pointer represents the state of each thread's execution.

```java
void main(String args[]){
    int x = 1;
    int y = 2;
    int result = sum(x,y);
}
int sum(int x,int y){
    int s = x+y;
    return s;
}
```

Here's a visualization of the stack call for the provided code:

```jsx
main()
  |
  |--> int x = 1
  |--> int y = 2
  |--> sum(x, y)
       |
       |--> int s = x + y
       |--> return s
  |
  |--> int result = [returned value]
```

This diagram illustrates the stack's evolution during code execution:

- The `main()` method is called first, creating a new frame on the stack.
- Local variables `x` and `y` are initialized in the `main()` frame.
- The `sum()` method is called, creating a new frame on top of the stack.
- Parameters `x` and `y` are passed to `sum()`, and `s` is calculated.
- The `sum()` method returns, and its frame is popped off the stack.
- The returned value is stored in `result` in the `main()` frame.

This stack call representation demonstrates how local variables are stored and method calls are managed on the stack.

- All variables on the stack belong to the thread executing on that stack
- Stack memory is statically allocated when a thread is created (stack size is fixed and relatively small - platform specific)
- A StackOverflowError may occur if the calling hierarchy is too deep

### Heap

- Shared memory region that belongs to the process
- All objects are stored in the heap (created with the `new` keyword - e.g., String, ArrayList)
- Class members are stored here
- Static variables are stored in the heap
- Managed and governed by the garbage collector
- Objects persist as long as there's a reference to them
- Instance members exist as long as their parent object exists
- Static variables persist for the lifetime of the process
- Heap memory is shared among all threads, which can lead to concurrency issues

Note: A reference is not the same as the object itself. Understanding this distinction is crucial for effective memory management and avoiding memory leaks.

## Resource Sharing Between Threads

Resources shared between threads can include variables, data structures, files, connection handles, messages, work queues, or any other objects accessible by multiple threads.

### Why We Need to Share Resources

Resource sharing is essential for efficient multi-threaded applications. For example, in a text editor, a document saver thread and a UI thread might need to access the same document data.

### Benefits of Resource Sharing

- Improved performance through parallel processing
- Efficient use of system resources
- Enhanced responsiveness in user interfaces

### Problems in Sharing Resources

- Data inconsistency due to race conditions
- Integrity issues caused by concurrent modifications
- Deadlocks and livelocks
- Increased complexity in program design and debugging

### Strategies for Safe Resource Sharing

- Use of synchronization mechanisms (e.g., locks, semaphores)
- Implementing thread-safe data structures
- Utilizing atomic operations for simple shared variables
- Employing higher-level concurrency utilities (e.g., java.util.concurrent package)

```java
import java.util.Random;

/**
 * This class demonstrates a simple inventory management system with concurrent operations.
 * It highlights potential issues with shared resources in a multi-threaded environment.
 */
public class InventoryCounter {
    // Maximum value for initial inventory
    private final static Integer MAX_VALUE = 10000;
    // Number of operations to perform
    private final static Integer OP_COUNT = 1000;

    public static void main(String[] args) {
        Random random = new Random();
        // Initialize inventory with a random value
        Inventory inventory = new Inventory(random.nextInt(MAX_VALUE));

        System.out.println("Before : " + inventory);

        // Create threads for incrementing and decrementing inventory
        Thread managerDecrement = new ManagerDecrement(inventory, OP_COUNT);
        Thread managerIncrement = new ManagerIncrement(inventory, OP_COUNT);

        // Start both threads
        managerIncrement.start();
        managerDecrement.start();

        try {
            // Wait for both threads to complete
            managerIncrement.join();
            managerDecrement.join();
        } catch(InterruptedException ie) {
            System.out.println("Thread interrupted: " + ie.getMessage());
        }
        System.out.println("After : " + inventory);
    }

    /**
     * Represents the inventory with methods to modify its value.
     * Note: This class is not thread-safe, which may lead to race conditions.
     */
    public static class Inventory {
        private Integer value;

        public Inventory(Integer value) {
            this.value = value;
        }

        public void increment() {
            value++;  // This operation is not atomic
        }

        public void decrement() {
            value--;  // This operation is not atomic
        }

        @Override
        public String toString() {
            return "Inventory [value=" + value + "]";
        }
    }

    /**
     * Thread class for incrementing the inventory.
     */
    public static class ManagerIncrement extends Thread {
        private Inventory inventory;
        private Integer count;

        public ManagerIncrement(Inventory inventory, Integer count) {
            this.inventory = inventory;
            this.count = count;
        }

        @Override
        public void run() {
            for (int i = 0; i < count; i++) {
                inventory.increment();
            }
        }
    }

    /**
     * Thread class for decrementing the inventory.
     */
    public static class ManagerDecrement extends Thread {
        private Inventory inventory;
        private Integer count;

        public ManagerDecrement(Inventory inventory, Integer count) {
            this.inventory = inventory;
            this.count = count;
        }

        @Override
        public void run() {
            for (int i = 0; i < count; i++) {
                inventory.decrement();
            }
        }
    }
}
	}
}
```

The above code shows inconsistency in the output refer below topic

### Atomic Operations

An atomic operation is one that appears to the rest of the system as if it occurred instantaneously and indivisibly. Key characteristics include:

- Executed as a single, indivisible unit
- "All-or-nothing" execution - either completes entirely or not at all
- No observable intermediate states
- Cannot be interrupted by other concurrent operations

Example of a non-atomic operation:

```java
item++; // This is not atomic
// It involves three steps:
// 1. Read the current value of item
// 2. Increment the value
// 3. Write the new value back to item
```

### Atomic Operations in Java

- Most operations are not atomic
- All reference assignments are atomic
- Getter and setter methods for references are atomic
- Assignments to all primitive types are atomic, except for long and double
- The java.util.concurrent.atomic package provides classes for atomic operations on single variables

Understanding atomic operations is crucial for writing thread-safe code and avoiding race conditions in concurrent programming.

## Concurrency challenges and solutions

### Critical Section

A critical section is a part of a program where shared resources are accessed. It's crucial for maintaining data integrity in concurrent programming.

- Characteristics of critical sections:
    - Only one thread can execute in the critical section at a time
    - Ensures mutual exclusion to prevent race conditions
    - Should be as short as possible to improve concurrency

Proper management of critical sections is essential for thread safety and preventing data corruption in multi-threaded applications.

### Synchronized - monitor/lock

- Locking mechanism to restrict access to a critical section or entire method to a single thread at a time
- Types of synchronization:
    - Method-level: Entire method is synchronized
    - Block-level: Only a specific block of code is synchronized
- Intrinsic locks (monitor locks) are used by default
- Can use different objects for locking to improve granularity
- Static synchronized methods use class-level locks
- Considerations:
    - Can impact performance due to thread contention
    - Potential for deadlocks if not used carefully
    - Ensures visibility of shared data between threads

```java
public class SynchronizedMonitor {
	public static void main(String[] args) {
		// Main method implementation
	}

	public static class Inventory {
		private Integer value;
		private Integer empValue;
		private Integer managerValue;
		private static Integer staticValue;

		private final Object lockEmployee = new Object();
		private final Object lockManager = new Object();

		public Inventory(Integer value) {
			this.value = value;
			staticValue = 0;
			managerValue = 0;
			empValue = 0;
		}

		// Method-level synchronization
		public synchronized void increment() {
			value++;
		}

		// Block-level synchronization using 'this' as lock
		public void decrement() {
			synchronized (this) {
				value--;
			}
		}

		// Using a specific object for locking
		public void managerIncrement() {
			synchronized (lockManager) {
				managerValue++;
			}
		}

		// Using a specific object for locking
		public void managerDecrement() {
			synchronized (lockManager) {
				managerValue--;
			}
		}

		// Using a different object for locking
		public void employeeIncrement() {
			synchronized (lockEmployee) {
				empValue++;
			}
		}

		// Using a different object for locking
		public void employeeDecrement() {
			synchronized (lockEmployee) {
				empValue--;
			}
		}

		// Static synchronized method (class-level lock)
		public static synchronized void staticValueIncrement() {
			staticValue++;
		}

		// Equivalent to static synchronized method
		public static void staticValueDecrement() {
			synchronized (SynchronizedMonitor.class) {
				staticValue--;
			}
		}
	}
}

```

This example demonstrates various ways to use synchronization in Java, including method-level, block-level, static, and object-specific synchronization.

- synchronized lock is reentrant

A reentrant lock means that a thread can acquire the same lock multiple times without deadlocking itself. This is particularly useful when a synchronized method calls another synchronized method on the same object.

### Volatile

```java
package com.litmus7.thread;

public class Volatile {
	public static void main(String[] args) {
		SharedClass sharedClass = new SharedClass();
		Thread thread1 = new Thread(() -> {
			for (int i = 0; i < Integer.MAX_VALUE; i++) {
				sharedClass.increment();
			}
		});

		Thread thread2 = new Thread(() -> {
			for (int i = 0; i < Integer.MAX_VALUE; i++) {
				sharedClass.checkForDataRace();
			}

		});

		thread1.start();
		thread2.start();
	}

	public static class SharedClass {
		private int x = 0;
		private int y = 0;

		public void increment() {
			x++;
			y++;
		}

		public void checkForDataRace() {
			if (y > x) {
				System.out.println("y > x - Data Race is detected");
			}
		}
	}
}
```

Here are some notes and comments on the volatile keyword in Java:

- The volatile keyword is used to ensure visibility of shared variables across threads
- It guarantees that any thread reading a volatile variable will see the most recently written value
- Volatile variables are not cached in registers or processor-specific caches
- Reading and writing to volatile variables causes a memory barrier, forcing cache coherence
- Volatile is useful for flag variables that indicate status changes across threads
- It does not provide atomicity for operations like increment (i++)
- Volatile is lighter weight than synchronization but provides weaker guarantees
- It's commonly used in double-checked locking patterns for lazy initialization

Example usage:

```java
public class SharedFlag {
    private volatile boolean flag = false;
    
    public void setFlag() {
        flag = true;
    }
    
    public boolean isSet() {
        return flag;
    }
}
```

In this example, changes to the flag variable in one thread are immediately visible to other threads.

## Race Conditions and Data Races

### Race Condition

- Occurs when multiple threads access a shared resource concurrently
- At least one thread is modifying the resource
- The timing of thread scheduling may lead to incorrect results
- Often involves non-atomic operations on shared resources
- Can lead to data inconsistency and unpredictable behavior

Solution: Use synchronization mechanisms like locks or monitors

### Data Race

- Occurs when compiler or CPU reorders instructions for optimization
- Reordering maintains logical correctness for single-threaded execution
- Out-of-order execution is crucial for improving performance
- Can result in unexpected, paradoxical, and incorrect outcomes in multi-threaded scenarios
- Difficult to reproduce and debug due to their non-deterministic nature

### Differences and Relationships

- Race conditions are a higher-level concurrency issue, while data races are low-level memory access conflicts
- Not all race conditions involve data races, and not all data races lead to race conditions
- Both can be mitigated through proper synchronization and use of thread-safe constructs

### Prevention Strategies

- Use atomic operations for simple shared variables
- Implement thread-safe data structures
- Utilize higher-level concurrency utilities from java.util.concurrent package
- Apply the principle of confinement: limit shared mutable state

```java
// Example of a potential race condition
class Counter {
    private int count = 0;
    
    public void increment() {
        count++; // This is not an atomic operation
    }
    
    public int getCount() {
        return count;
    }
}

// Thread-safe version using AtomicInteger
import java.util.concurrent.atomic.AtomicInteger;

class SafeCounter {
    private AtomicInteger count = new AtomicInteger(0);
    
    public void increment() {
        count.incrementAndGet(); // This is an atomic operation
    }
    
    public int getCount() {
        return count.get();
    }
}
```

Understanding and addressing race conditions and data races is crucial for developing robust and reliable multi-threaded applications.

### Rule of thumb for shared variables

- Every shared variable should either:
    - Be guarded by a synchronized block or any type of lock
    - Be declared volatile
    - Use atomic classes from java.util.concurrent.atomic package

Additional considerations:

- Understand the trade-offs between synchronization, volatile, and atomic operations
- Use immutable objects when possible to avoid synchronization issues
- Consider using thread-local variables for thread-specific data
- Be aware of the limitations of volatile (e.g., not suitable for compound operations)

## Locking Strategies & dead lock

1. Fine grained locking - every method has individual lock accordingly, chance of deadlock, increased overhead.
2. Coarse grained locking - a single lock for entire thing, all operations are locked when one thread is executing an operation and CPU idle time is the disadvantage.

## Deadlock

Deadlock is a situation where each process waits for a resource held by another process, resulting in no process execution

### Condition

1. Mutual exclusion - only one thread can have exclusive access to a resource
2. Hold & wait - at least one thread is holding a resource & is waiting for another resource
3. Non - Preemptive - a resource is released only after the thread is done using it
4. Circular wait - one is waiting for resource held by other thread & vice versa.

### Solution

Avoiding circular wait is the simplest technique for avoiding deadlock situation

- Enforce a strict order in lock acquisition

```java
// Thread 1                                     Thread 2
lock(A);                                        lock(B);      
						          // critical section
lock(B);                                        lock(A);
                      // critical section   
unlock(B);                                      unlock(A);
unlock(A);                                      unlock(B);

// there is a chance of deadlock in above code

// Thread 1                                     Thread 2
lock(A);                                        lock(A);      
						          // critical section
lock(B);                                        lock(B);
                      // critical section   
unlock(B);                                      unlock(B);
unlock(A);                                      unlock(A);

// by keeping the order in locking we can avoid a dead lock situation
```

we can implement a watch dog for dead lock detection

- Thread interruption (not possible with synchronized)
- tryLock () (not possible with synchronized)

## Reentrant Lock

package: java.util.concurrent.locks.ReentrantLock
