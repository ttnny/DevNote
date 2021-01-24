# App Components



## Activities

* Subclass of `Activity` class
* Activities are declared as individual `<activity>` elements in the manifest file
* An activity is activated by an asynchronous message called an `intent`
* Example:
  * Start an activity by passing an `intent` to `startActivity()` or `startActivityForResult()`



## Services

* Started services & Bound services
* Services are declared as individual `<service>` elements in the manifest file
* Schedule a `JobService` to perform some work based on the event with `JobScheduler`
* A service is activated by an asynchronous message called an `intent`
* Example:
  * Start a service using the `JobScheduler` class to schedule actions



## Broadcast receivers

* Subclass of `BroadcastReceiver`
* Broadcast receivers are declared as individual `<receiver>` elements in the manifest file. They can also be created dynamically in code as `BroadcastReceiver` objects and registered with the system by calling `registerReceiver()`
* Each broadcast is delivered as an `intent` object
* A broadcast receiver is activated by an asynchronous message called an `intent`
* Example:
  * Initiate a broadcast by passing an `intent` to methods such as `sendBroadcast()`, `sendOrderedBroadcast()`, or `sendStickyBroadcast()`



## Content providers

* A subclass of `ContentProvider`
* Content providers are declared as individual `<provider>` elements in the manifest file
* A content provider is activated when targeted by a request from a `ContentResolver`
* Example:
  * Perform a query to a content provider by calling `query()` on a `ContentResolver`



## References

* [https://developer.android.com/guide/components/fundamentals](https://developer.android.com/guide/components/fundamentals)
* [https://developer.android.com/guide/components/activities](https://developer.android.com/guide/components/activities)
* [https://developer.android.com/guide/components/service](https://developer.android.com/guide/components/services)[s](https://developer.android.com/reference/android/content/BroadcastReceiver)
* [https://developer.android.com/guide/components/broadcasts](https://developer.android.com/guide/components/broadcasts)
* [https://developer.android.com/guide/topics/providers/content-providers](https://developer.android.com/guide/topics/providers/content-providers)
* [https://developer.android.com/guide/components/intents-filter](https://developer.android.com/guide/components/intents-filters)

