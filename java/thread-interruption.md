# Thread Interruption

The interruption mechanism provided by `Thread` and supported by `Thread.sleep()` and `Object.wait()` is a cancellation mechanism; it allows one thread to request that another thread stop what it is doing early. When a method throws `InterruptedException`, it is telling you that if the thread executing the method is interrupted, it will make an attempt to stop what it is doing and return early and indicate its early return by throwing `InterruptedException`. Well-behaved blocking library methods should be responsive to interruption and throw `InterruptedException` so they can be used within cancelable activities without compromising responsiveness.

Every thread has a Boolean property associated with it that represents its *interrupted status*. The interrupted status is initially false; when a thread is interrupted by some other thread through a call to `Thread.interrupt()`, one of two things happens. If that thread is executing a low-level interruptible blocking method like `Thread.sleep()`, `Thread.join()`, or `Object.wait()`, it unblocks and throws `InterruptedException`. Otherwise, `interrupt()` merely sets the thread's interruption status. Code running in the interrupted thread can later poll the interrupted status to see if it has been requested to stop what it is doing; the interrupted status can be read with `Thread.isInterrupted()` and can be read and cleared in a single operation with the poorly named `Thread.interrupted()`.

Interruption is a cooperative mechanism. When one thread interrupts another, the interrupted thread does not necessarily stop what it is doing immediately. Instead, interruption is a way of politely asking another thread to stop what it is doing if it wants to, at its convenience. Some methods, like `Thread.sleep()`, take this request seriously, but methods are not required to pay attention to interruption. Methods that do not block but that still may take a long time to execute can respect requests for interruption by polling the interrupted status and return early if interrupted. You are free to ignore an interruption request, but doing so may compromise responsiveness.

One of the benefits of the cooperative nature of interruption is that it provides more flexibility for safely constructing cancelable activities. We rarely want an activity to stop immediately; program data structures could be left in an inconsistent state if the activity were canceled mid-update. Interruption allows a cancelable activity to clean up any work in progress, restore invariants, notify other activities of the cancellation, and then terminate.



## with Thread

* How to request a task, running on a separate thread, to finish early?
* How to make a task responsive to such a finish request?

**One thread cannot stop the other thread. A thread can only request the other thread to stop. The request is made in the form of an interruption.** Calling the `interrupt()` method on an instance of a`Thread` sets the interrupt status state as `true` on the instance.

Use interruption to request a task, running on a separate thread, to finish.

*Question arises that why did Thread.sleep() throw an InterruptedException?* As soon as the `taskThread` was interrupted by the main thread, the `Thread.sleep(1_000)` responded to the interruption by throwing the exception. ***In fact almost all blocking methods respond to interruption by throwing InterruptedException.\***The decision of what to do in the case of interruption is left to the implementing code, which in this example is breaking out of the for loop as per the requirement in the use case.

*Note: Calls to sleep() and join() methods in main() method are blocking and may also throw InterruptedException upon interruption. Handling of the exception here has been omitted for brevity.*

Handle interruption request, which in most cases is done by handling InterruptedException, in the task to make it responsive to a finish request.



## with Executor



## InterruptedException & Interruption Status

***what happens to a thread’s interruption status when a blocking code responds to interruption\*** by throwing `InterruptedException`

Before a blocking code throws an InterruptedException, it marks the interruption status as false. Thus, when handling of the InterruptedException is done, you should also preserve the interruption status by calling`Thread.currentThread().interrupt()`.



## References

- https://dzone.com/articles/understanding-thread-interruption-in-java
- https://docs.oracle.com/javase/tutorial/essential/concurrency/interrupt.html
- https://www.ibm.com/developerworks/library/j-jtp05236/index.html