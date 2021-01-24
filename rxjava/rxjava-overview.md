# RxJava



## Intro

**Functional programming** is the process of building software by composing pure functions, avoiding shared state, mutable data, and side-effects.

**Reactive programming** is an asynchronous programming paradigm concerned with data streams and the propagation of change.

Together, **functional reactive programming** forms a combination of functional and reactive techniques that can represent an elegant approach to event-driven programming – with values that change over time and where the consumer reacts to the data as it comes in.

Reactive Systems are:

* Responsive – systems should respond in a timely manner
* Message Driven – systems should use async message-passing between components to ensure loose coupling
* Elastic – systems should stay responsive under high load
* Resilient – systems should stay responsive when some components fail



## Observables

Observable __represents any object that can get data from a data source and whose state may be of interest in a way that other objects may register an interest.

Types of observables:

* Observable
* Flowable
* Single
* Maybe
* Completable

Observables can actually be categorized into two groups:

* _Non-Blocking –_ asynchronous execution is supported and is allowed to unsubscribe at any point in the event stream.
* _Blocking –_ all _onNext_ observer calls will be synchronous, and it is not possible to unsubscribe in the middle of an event stream. We can always convert an _Observable_ into a _Blocking Observable_, using the method _toBlocking_

Observable sources don't support _backpressure_. Because of that, we should use it for sources that we merely consume and can't influence. From RxJava 2, we can use [Flowable](https://www.baeldung.com/rxjava-2-flowable) to handle backpressure-aware sources.



## Observers

Observer is an object that wishes to be notified when the state of another object changes.

There are three methods on the _observer_ interface that we want to know about:

* _OnNext_ is called on our _observer_ each time a new event is published to the attached _Observable_. This is the method where we'll perform some action on each event
* _OnCompleted_ is called when the sequence of events associated with an _Observable_ is complete, indicating that we should not expect any more _onNext_ calls on our observer
* _OnError_ is called when an unhandled exception is thrown during the _RxJava_ framework code or our event handling code



## Operators

An operator is a function that takes one Observable \(or data source, depending on the type of operator\) as its first argument and returns another Observable \(the destination\). Then for every item that the source observable emits, it will apply a function to that item, and then emit the result on the destination Observable.

Operators can be chained together to create complex data flows that filter event based on certain criteria. Multiple operators can be applied to the same observable.

 It is not difficult to get into a situation in which an _Observable_ is emitting items faster than an _operator_ or _observer_ can consume them. You can read more about back-pressure [here.](https://www.baeldung.com/rxjava-backpressure)

Types of operators:

* Creating: `just()` `defer()` `from()` `interval()` `empty()` `never()` `throw()`
* Transforming: `map()` `flatMap()` `buffer()` `scan()`
* Filtering: `filter()` `first()` `last()` `take()` `skip()` `elementAt()` `distinct()`
* Combining: `and()` `then()` `when()` `join()` `merge()` `switch()`
* Error Handling: `catch()` `retry()`
* Observable Utility: `observeOn()` `subscribeOn()` `subscribe()` `delay()`
* Conditional & Boolean: `all()` `contains()` `defaultIfEmpty()` `skipUntil()`
* Mathematical & Aggregate: `average()` `concat()` `count()` `max()` `sum()`
* Backpressure: more [here](http://reactivex.io/documentation/operators/backpressure.html), [here](https://www.baeldung.com/rxjava-backpressure), as well as the [Flowable](https://www.baeldung.com/rxjava-2-flowable)
* Connectable Observable: `connect()` `publish()` `replay()`
* Converting: `to()`



## Subjects

A Subject can be simultaneously a subscriber and an observable. As a subscriber, a subject can be used to publish the events coming from more than one observable. And because it's also observable, the events from multiple subscribers can be reemitted as its events to anyone observing it.



## References

[https://www.baeldung.com/rx-java](https://www.baeldung.com/rx-java)  
[http://reactivex.io/documentation/operators.html](http://reactivex.io/documentation/operators.html)  
[https://www.baeldung.com/rxjava-2-flowable](https://www.baeldung.com/rxjava-2-flowable)