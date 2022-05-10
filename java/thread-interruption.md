# Thread Interruption

- The interrupted thread must support its own interruption. For e.g., if the thread is frequently invoking methods that throw `InterruptedException`, it simply returns from the `run` method after it catches that exception.

- One thread can request another thread to stop what it is doing early. However, interruption is a cooperative mechanism. The interrupted thread does not necessarily stop what it is doing immediately. It can ignore or will stop at its convenience.

- Every thread has a non-static `isInterrupted()` boolean property that is `false` by default and will be set to `true` when `Thread.interrupt()` is invoked. If that interrupted thread is executing a low-level interruptible blocking method like `Thread.sleep()`, `Thread.join()`, or `Object.wait()`, it unblocks and throws `InterruptedException`.

- Most blocking methods respond to interruption by throwing `InterruptedException`.

  ```java
  if (Thread.interrupted()) {
      throw new InterruptedException();
  }
  ```

- Best practice to preserve interruption status is to re-interrupt after handling the `InterruptedException` is done.

  ```java
  try {
  	// ...
  } catch (InterruptedException e) {
  	Thread.currentThread().interrupt();
    return; // or break;
  }
  ```

  Before a blocking code (on the interrupted thread) throws an `InterruptedException`, it marks the `isInterrupted` as `false`. Thus, when handling of the `InterruptedException` is done, we should also consider preserving the interruption status by calling `Thread.currentThread().interrupt()`.

- Methods should but are not required to pay attention to interruption. Methods that do not block but that still may take a long time to execute can respect requests for interruption by polling the interrupted status and return early if interrupted.



## Interruption with Thread

**One thread cannot stop the other thread. A thread can only request the other thread to stop. The request is made in the form of an interruption.**

- Calling the `interrupt()` method on an instance of a thread sets the interrupt status state, `isInterrupted`, to `true`.
- The interrupted thread responds to the interruption request by handling `InterruptedException`.

```java
public static void main(String[] args) throws InterruptedException {
    Thread countingThread = new Thread(countingThread());
    countingThread.start();			// start countingThread on a background thread
    Thread.sleep(3_000);     		// main thread sleeps for 3s
    countingThread.interrupt(); // interrupt countingThread
    countingThread.join(1_000); // main thread waits for 1s before shutdown program
}

private static Runnable countingThread() {
    return () -> {
        for (int i = 0; i < 10; i++) {
            System.out.print(i);
            try {
                Thread.sleep(1_000);
            } catch (InterruptedException e) {
                break;
            }
        }
    };
}
```

*Note: Calls to sleep() and join() methods in main() method are blocking and may also throw InterruptedException upon interruption. Handling of the exception here has been omitted for brevity.*



## Interruption with Executor

Executor framework is a complete asynchronous task execution framework. Executor framework is preferred over Threads as it provides separation of task execution from the thread management.

```java
public static void main(String[] args) throws InterruptedException {
    ExecutorService executor = Executors.newSingleThreadExecutor();
    executor.submit(countingThread());  						// ExecutorService runs the submitted task
    Thread.sleep(3_000);														// main thread sleeps for 3s
    executor.shutdownNow();													// running task is interrupted
    executor.awaitTermination(1, TimeUnit.SECONDS);	// awaits for the service to shutdown
}

private static Runnable countingThread() {
    return () -> {
        for (int i = 0; i < 10; i++) {
            System.out.print(i);
            try {
                Thread.sleep(1_000);
            } catch (InterruptedException e) {
                break;
            }
        }
    };
}
```

Specific task can be interrupted without shutting down the `ExecutorService`. On submitting a task to the service an instance of `Future<?>` is returned by the service. You may call the `cancel()` method on that instance to interrupt the task. In situations when you service a web request by running parallel tasks, this method of cancelling tasks and not shutting down the service helps in re-using the service across multiple requests. In such situations you may want to shutdown the service only on shutdown of your web application. Calling the `cancel()` with `true` causes the task to be interrupted.

```java
Future<?> submittedTask = executor.submit(someTask());
// ...
submittedTask.cancel(true) // if conditions to cancel the task have been met
```



## References

- https://dzone.com/articles/understanding-thread-interruption-in-java
- https://docs.oracle.com/javase/tutorial/essential/concurrency/interrupt.html
- https://stackoverflow.com/a/3976377
- https://www.ibm.com/developerworks/library/j-jtp05236/index.html