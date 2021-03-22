# Double-checked Locking

Double-checked locking optimization (a design pattern) reduces the overhead of acquiring a lock by testing the locking criterion (lock hint) before acquiring the lock. Locking occurs only if the locking criterion check indicates that locking is required.

2 ways to do double-checked locking optimization:

- Java `AtomicReference`
- Java `volatile` variables

One of  the usage examples of using this design pattern is to make a singleton thread-safe.



## AtomicReference

```java
class Sample<T> {
	// Implement the singleton using an AtomicReference static field.
	public static AtomicReference<Sample> singleton =
		new AtomicReference<>(null); // init value is null
	
	// getters, setters, etc.
	
    public static <T> Sample getInstance() {
    	// Get the current value of the singleton.
    	Sample<T> instance = singleton.get();
    	
    	// If the singleton is not yet initialized.
    	if (instance == null) {
    		// Create a new singleton object.
    		// NOTE: With this approach, the Sample() cosntructor may be
            //	called more than once because of multithread access,
            //	so make sure the constructor is designed to allow that.
    		instance = new Sample<>();
    		
    		// Atomically set the refernce's value to the
            //	singleton if the current value is null.
    		if (!singleton.compareAndSet(null, instance)) {
    			// If the return is NOT true, then this is not the
                //	first time in, so just return the singleton's value.
    			instance = singleton.get();
    		}
    	}
    	
    	// Return the singleton's current value.
    	return singleton;
    }
}
```



## Volatile variables

```java
class Sample<T> {
	// Implement the singleton using a volatile static field
	public static volatile Sample singleton = null;

    // getters, setters, etc.
    
    public static <T> Sample getInstance() {
    	// Assign the volatile to local variable.
    	Sample instance = singleton;
    	
    	// If the singleton is not yet initialized.
    	if (instance == null) {
    		synchronized(Sample.class) { // only sync if instance == null
    			instance = singleton;
    			if (instance == null) { // double-check
    				singleton = instance = new Sample();
    			}
    		}
    	}
    	
    	// Return the singleton's current value.
    	return singleton;
    }
}
```



## References

- https://en.wikipedia.org/wiki/Double-checked_locking
- https://www.baeldung.com/java-singleton-double-checked-locking
- https://www.youtube.com/watch?v=L1Pq9-bx7KY&ab_channel=DouglasSchmidt