# Thread Safety

- Visibility
- Atomicity
- Happens-Before



## Visibility

JVM or compiler can perform vrious optimizations to the code, a consumer thread may not see changes in a variable.

```java
private static int sCount = 0;

static class Consumer extends Thread {
    @Override
    public void run() {
        int localValue = -1;
        while (true) {
            if (localValue != sCount) {
                System.out.println("Consumer: detected count change " + sCount);
                localValue = sCount;
              	// Changes made to sCount from Producer thread may not be seen here
              	// due to JVM or compiler can perform code optimizations.
              	// The local cached version of sCount might be used instead.
              	// Bad case: this thread will run forever.
            }
            if (sCount >= 5) {
                break;
            }
        }
        System.out.println("Consumer: terminating");
    }
}
```

Making `sCount` `volatile` resolves the issue.

```java
private volatile int sCount = 0;
```

`volatile` makes sure that changes made in one thread are immediately reflect in other thread. `sCount` value will be saved/retreived directly from memory without caches. 



## Atomicity

Race condition occurs when multiple processes or threads perform read/write operations on a set of data.

```java
new Thread(() -> {
  for (int i = 0; i < 10; i++) {
    sCount++;
    // When multiple threads 
  }
}).start();
```





## Happens-Before





------

https://www.udemy.com/course/android-multithreading/

https://www.baeldung.com/java-volatile-variables-thread-safety