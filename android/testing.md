# Testing



## Overview

### Testing Pyramid:

* Unit tests \(@SmallTest, 70%\): testing individual methods of a single class at a time \(**local unit tests**\).
* Integration tests \(@MediumTest, 20%\): testing a specific feature or an interaction of several classes or modules \(**local or instrumented unit tests**\).
* End-to-end tests \(E2E\) \(@LargeTest, 10%\): testing a combination of features working together to see whether the application actually works as a whole \(**instrumented unit tests**\).

### Android test directories:

* `test` contains **local unit tests**
* `androidTest` contains **instrumented unit tests** such as integration and end-to-end



## Local & Instrumented Unit Tests

### Recommended libraries and tools

* Should be written as a **JUnit 4** test class.
* [**AndroidX Test**](https://developer.android.com/training/testing/set-up-project) provides JUnit, Core APIs, Espresso, Truth, etc.
* **Robolectric** if tests depend on objects in Android framework without using an emulator or real device. Robolectric supports component lifecycles, event loops, and all resources.
* [**Android Builder**](https://developer.android.com/training/testing/unit-testing/local-unit-tests#android-builders) classes.
* [**Parcelables utility**](https://developer.android.com/training/testing/unit-testing/local-unit-tests#android-parcelables-util) class.
* **Mockito** if tests have minimal dependencies on the Android framework, or if the tests depend only on your own objects.
* **Truth** library and classes from **Android Assertions.**
* [**Espresso** ](https://developer.android.com/training/testing/espresso)and [**UI Automator**](https://developer.android.com/training/testing/ui-automator) for functional UI tests.
* **Hamcrest matcher** APIs.
* [**App Crawler**](https://developer.android.com/training/testing/crawler) tool \(part of Jetpack\) for test automation.
* [**JUnit4 rules**](https://developer.android.com/training/testing/junit-rules) with [**AndroidJUnitRunner**](https://developer.android.com/training/testing/junit-runner)**.**

### Best practices

#### General

* Pure view model tests can usually go in the `test` source set because their code doesn't usually require Android.
* **AndroidX Test APIs** are built to work both for local tests and instrumented tests.
* Use **AndroidX Test library** to get test versions of components like Applications and Activities. Depending on whether the test is being run as a local or instrumented test, different versions will be used. For example with `ApplicationProvider.getApplicationContext()`:
  * If `test`, a simulated Android env will be used.
  * If `androidTest`, an actual Application context provided when it boots up an emulator or connects to a real device.
* The **AndroidJUnit4** test runner allows for AndroidX Test to run tests differently depending on whether they are instrumented or local tests.
* If need to run simulated Android code in `test` source set, add the Robolectric dependency and the `@RunWith(AndroidJUnit4::class)` annotation.
* Alternatives: **junit.Assert** methods or **Hamcrest matchers** \(such as the `is()` and `equalTo()` methods\). _Hamcrest is still the preferred library to use when constructing matchers, such as for Espresso's_ [_`ViewMatcher`_](https://developer.android.com/reference/androidx/test/espresso/matcher/ViewMatchers) _class._

#### Instrumented tests

* \(?!?\) If creating an instrumented JUnit 4 test class to run on a device or emulator, the test class must be prefixed with the `@RunWith(AndroidJUnit4::class)` annotation.
* \(?!?\)  `@RunWith(AndroidJUnit4::class)` - Used in any class using AndroidX Test
* `ApplicationProvider` provides `getApplicationContext()` to get application context for the application under test. `@RunWith(AndroidJUnit4::class)` annotation is required.
* `ActivityScenario` allows to test app's activities transition through different states in their lifecycles.
* `IntentSubject` in Truth allows to verify intent values and provides readable error messages.

#### ViewModel and LiveData

* To test `LiveData`, use [`InstantTaskExecutorRule`](https://developer.android.com/codelabs/advanced-android-kotlin-training-testing-basics#8) and ensure `LiveData` observation with [`observeForever()`](https://developer.android.com/reference/kotlin/androidx/lifecycle/LiveData#observeforever) and [`removeObserver()`](https://developer.android.com/reference/kotlin/androidx/lifecycle/LiveData#removeobserver) when done. Doing so, we can observe the `LiveData` without the need of `LifecycleOwner` \(activity or fragment\). **OR** use this helper [LiveDataTestUtil.kt](https://gist.github.com/ttnny/5f0a91f8ae4432012e0fa68422a4a9f4) to get rid of the boilerplate code.

#### Coroutines

* To test couroutines \(suspend functions\), use `runBlockingTest` function from the `kotlinx-coroutines-test` library. This function makes a coroutine run like a non-coroutine. It executes the coroutine in a special coroutine context which runs synchronously and immediately, meaning actions will occur in a deterministic order. [\[more\]](https://developer.android.com/codelabs/advanced-android-kotlin-training-testing-test-doubles/#4)
* Use `runBlocking` to have a closer simulation of what a "real" implementation of the repository would do. This is preferable for **fakes**. _Example: use `runBlocking` in the FakeRepository class, then use `runBlockingTest` in the test class \(class with @Test functions\) to get deterministic behavior._ [\[more\]](https://developer.android.com/codelabs/advanced-android-kotlin-training-testing-test-doubles/#5)

#### Fragment and Activity

* Test Activity and its lifecycle using **[ActivityScenario](https://developer.android.com/reference/androidx/test/core/app/ActivityScenario)** and [**ActivityScenarioRule**](https://developer.android.com/reference/androidx/test/ext/junit/rules/ActivityScenarioRule). Read more at [testing activities](https://developer.android.com/guide/components/activities/testing), [Android Lifecycle](https://developer.android.com/topic/libraries/architecture/lifecycle#lc), and also [examples](testing-activity.md).
* To launch a fragment from a test, using [`launchFragmentInContainer`](https://developer.android.com/codelabs/advanced-android-kotlin-training-testing-test-doubles#7) to create a `FragmentScenario`, a class from AndroidX Test that wraps around a fragment and gives direct control over the fragment's lifecycle for testing. The fragment will be launched inside a generic empty activity so that it's properly isolated from activity code \(we test the fragment code, not the associated activity\).
* To test fragment and view-model interactions, use **ServiceLocator pattern** and the **Espresso** and **Mockito** libraries. [[more]](https://developer.android.com/codelabs/advanced-android-kotlin-training-testing-test-doubles/#7)



## [Service Testing](https://developer.android.com/training/testing/integration-testing/service-testing)

updating...



## [Content Provider Testing](https://developer.android.com/training/testing/integration-testing/content-provider-testing)

updating...



## References

* [https://developer.android.com/training/testing/](https://developer.android.com/training/testing/)
* [https://medium.com/better-programming/exploring-androidx-for-testing-6350100b4711](https://medium.com/better-programming/exploring-androidx-for-testing-6350100b4711)
* [https://developer.android.com/codelabs/advanced-android-kotlin-training-testing-basics](https://developer.android.com/codelabs/advanced-android-kotlin-training-testing-basics/)
* [https://developer.android.com/codelabs/advanced-android-kotlin-training-testing-test-doubles](https://developer.android.com/codelabs/advanced-android-kotlin-training-testing-test-doubles)
* [https://developer.android.com/codelabs/advanced-android-kotlin-training-testing-survey](https://developer.android.com/codelabs/advanced-android-kotlin-training-testing-survey)

