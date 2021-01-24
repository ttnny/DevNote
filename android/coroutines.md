# Coroutines



## **Pattern**

1. Launch a coroutine that runs on the main or UI thread, because the result affects the UI. You can access the `CoroutineScope` of a ViewModel through the `viewModelScope` property of the ViewModel, as shown in the following example:
2. Call a suspend function to do the long-running work, so that you don't block the UI thread while waiting for the result.
3. The long-running work has nothing to do with the UI. Switch to the I/O dispatcher,\(if using Room, this code is generated for you\) so that the work can run in a thread pool that's optimized and set aside for these kinds of operations.
4. Then call the long running function to do the work.



## **Pseudo code**

```text
fun someWorkNeedsToBeDone {
   viewModelScope.launch {
        suspendFunction()
   }
}
​
suspend fun suspendFunction() {
   withContext(Dispatchers.IO) {
       longrunningWork()
   }
}
```



## **Coroutines and Room**

Similar to the pattern above but there is no need to specify the dispatcher since Room uses Dispatchers.IO

```text
// Using Room
fun someWorkNeedsToBeDone {
   viewModelScope.launch {
        suspendDAOFunction()
   }
}
​
suspend fun suspendDAOFunction() {
   // No need to specify the Dispatcher, Room uses Dispatchers.IO.
   longrunningDatabaseWork()
}
```



## References

* [https://developer.android.com/topic/libraries/architecture/coroutines?authuser=1\#viewmodelscope](https://developer.android.com/topic/libraries/architecture/coroutines?authuser=1#viewmodelscope)

