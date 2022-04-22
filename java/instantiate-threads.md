# Instantiating New Threads

### Inheritance

```java
public class MyThread extends Thread {
  private final int x;
  
  public MyThread(int x) {
    this.x = x;
  }
  
  @Override
  public void run() {
    // Your implementation.
  }
}
```

```java
Thread myThread = new MyThread(8);
```



### Composition

with `Runnable` interface *(preferred, composition over inheritance)*

```java
Runnable runnable = new Runnable() {
  @Override
  public void run() {
    int x = 8;
    // Your implementation.
  }
}
```

```java
Thread myThread = new Thread(runnable);
```