# Thread Safety

- Visibility
- Atomicity
- Happens-Before



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

Race condition occurs when multiple processes or threads perform read/write operations on a set of data. These operations are non-atomic 

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





## Happens-Before





------

https://www.udemy.com/course/android-multithreading/

https://www.baeldung.com/java-volatile-variables-thread-safety