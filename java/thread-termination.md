# Thread Termination

When `run()` method returns.

- After a successful execution.

  ```java
  new Thread(new Runnable() {
  	@Override
  	public void run() {
  		// doing something
  	}
  }).start();
  ```

- In response to internal error.

  ```java
  new Thread(new Runnable() {
  	@Override
  	public void run() {
  		try {
  			// doing something
  		} catch (IOException e) {
  			return;
  		}
      // continue if no error
  	}
  }).start();
  ```

- In response to external set flag (**preferred** over thread interruption).

  ```java
  private AtomicBoolean aFlag = new AtomicBoolean(false);
  
  public void aMethod() {
    new Thread(new Runnable() {
      @Override
      public void run() {
        if (aFlag.get()) {
          return;
        }
        // doing something
       	// aFlag.set(true) doesn't guarantee thread termination beyond this point
      }
    }).start();
  }
  ```

- In response to thread interruption (similar to external set flag in some cases).

  ```java
  private Thread aThread;
  
  public void aMethod() {
    aThread = new Thread(new Runnable() {
      @Override
      public void run() {
        if (Thread.interrupted()) {
          return;
        }
        // doing something
        // aThread.interrupt() doesn't guarantee thread termination beyond this point
      }
    });
    aThread.start();
  }
  ```



------

https://www.udemy.com/course/android-multithreading/