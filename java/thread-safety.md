# Thread Safety

- Visibility
- Atomicity
- Happens-Before (lower level of Visibility)



## Visibility

JVM or compiler can perform vrious optimizations to the code, a consumer thread may not see changes in a variable.

```java
private int mCount = 0;

private void startCountThread() {
    new Thread(() -> {
        for (int i = 0; i < 10; i++) {
            mCount++;
          	// Changes made to mCount from other threads may not be seen here,
            // and vice versa, due to JVM/compiler can perform code optimizations.
            // The local cached version of mCount might be used in some operations
          	// of this thread or other theads.
        }
    }).start();
}
```

Making `mCount` `volatile` resolves the visibility issue.

```java
private volatile int mCount = 0;
```

`volatile` makes sure that changes made in one thread are immediately reflect in other thread. `mCount` value will be saved/retreived directly from memory without caches.

`volatile` itself may not make `mCount` thread-safe, atomicity should also be considered.



## Atomicity

Race condition occurs when multiple processes or threads perform read/write operations on a set of data. These operations are non-atomic. Making an operation atomic guarantees that no other threads can modify the set of data, which is being used, until the operation is complete.

```java
private volatile mCount = 0;

private void startCountThread() {
    new Thread(() -> {
        for (int i = 0; i < 10; i++) {
            mCount++;
          	// mCount is volatile and the visibility is secured.
          	// However, this increment operation is non-atomic and may not
    				// behave consistently in multi-threaded environment.
        }
    }).start();
}
```

#### Resolves using Atomic classes:

```java
private volatile AtomicInteger mCount = new AtomicInteger(0);
// We still need volatile (or final) to ensure that mCount is
// accessed from the same reference in all threads.

private void startCountThread() {
    new Thread(() -> {
        for (int i = 0; i < 10; i++) {
            mCount.getAndIncrement();
        }
    }).start();
}
```

Or using `final` depends on the case. Final variables are thread-safe.

```java
private final AtomicInteger mCount = new AtomicInteger(0);
// ...
```

#### Resolves using synchronization:

```java
private final Object LOCK = new Object();
private int mCount = 0;

private void startCountThread() {
    new Thread(() -> {
      	synchronized(LOCK) {
          	for (int i = 0; i < 10; i++) {
              	mCount.getAndIncrement();
          	}
        }
    }).start();
}
```



## Happens-Before

***hb(A, B)** indicates A happens-before B*

Happens-Before **rules**:

- A and B on the same thread: A comes before B in program order (as in the code).
- A synchronizes with a following B
- If hb(A, B) and hb(B, C), then hb(A, C).

- There is a happens-before from the end of constructor of an object to the start of a finalizer for that object.

Happens-Before **guarantees**:

- Unlock/release a monitor (lock object) happens-before every subsequent lock/acquire that monitor.
- Write to a `volatile` field happens-before read of that field.
- `Thread.start()` happens-before any actions in that started thread.
- Actions in a thread happens-before any other thread successfully returns from a `join()` on that thread.
- Default initialization of an object happens-before any other actions (other than default-writes) of a program.



------

https://docs.oracle.com/javase/specs/jls/se18/html/jls-17.html

https://www.udemy.com/course/android-multithreading/

https://www.baeldung.com/java-volatile-variables-thread-safety