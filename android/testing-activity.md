# Testing an Activity

`ActivityScenario` provides APIs to start and drive an Activity's [lifecycle](https://developer.android.com/topic/libraries/architecture/lifecycle#lc) state for testing.



## Gradle dependencies

```groovy
// Kotlin
androidTestImplementation 'androidx.test:core-ktx:1.3.0' // ActivityScenario
androidTestImplementation 'androidx.test.ext:junit-ktx:1.1.2' // ActivityScenarioRule

// Java
androidTestImplementation 'androidx.test:core:1.3.0' // ActivityScenario
androidTestImplementation 'androidx.test.ext:junit:1.1.2' //// ActivityScenarioRule
```



## Examples

### ActivityScenario

`ActivityScenario` is a replacement of ActivityController in Robolectric and ActivityTestRule in ATSL.

#### Launch an activity with a class reference

```kotlin
val scenario = launchActivity<MyActivity>()
scenario.onActivity { activity ->  
    // Test code goes here.

```

#### Launch an activity with a custom intent

```kotlin
val intent = Intent(ApplicationProvider.getApplicationContext(), MyActivity::class.java)
    .putExtra("title", "test rules...")

val scenario = launchActivity<MyActivity>(intent)
scenario.onActivity { activity ->  
    // Test code goes here.
}
```

#### Cleanup or teardown

Optional but recommended.`ActivityScenario` doesn’t do a cleanup or teardown after the test completes so we need to either call `close()` manually or use *try-with-resources* statement in Java ([use function](https://www.baeldung.com/kotlin/try-with-resources) in Kotlin).

```java
// Using Java try-with-resources statement
try (ActivityScenario<MyActivity> scenario = ActivityScenario.launch(MyActivity.class)) {
	// Test code goes here. For example:
	scenario.onActivity(activity -> {
    	assertThat(activity.getSomething()).isEqualTo("something");
	});
}

// Or manually close() the launched activity
@Before setUp() {...}
@Test myTest() {...}
@After tearDown() {
	scenario.close();
}
```

#### Do extra setup before launching an activity

```kotlin
lateinit var scenario: ActivityScenario<MyActivity>

@After
fun tearDown() {
    scenario.close()
}

@Test
fun myTest1() {
    val intent = Intent(ApplicationProvider.getApplicationContext(), MyActivity::class.java)
        .putExtra("title", "test rules...")
    scenario = launchActivity(intent)
    // Test code goes here.
}

@Test
fun myTest2() {
    val intent = Intent(ApplicationProvider.getApplicationContext(), MyActivity::class.java)
        .putExtra("title", "other test rules...")
    scenario = launchActivity(intent)
    // Your test code goes here.
}
```



### ActivityScenarioRule

`ActivityScenarioRule` launches a given activity before a test starts and closes it after the test completes.

#### Launch an activity with default intent

```kotlin
@get:Rule
val rule = activityScenarioRule<MyActivity>()

@Test
fun myTest() {
    val scenario = rule.scenario
    // Test code goes here.
}
```

#### Launch an activity with a custom intent

```kotlin
val intent = Intent(ApplicationProvider.getApplicationContext(), MyActivity::class.java)
    .putExtra("title", "test rules...")

@get:Rule
val activityRule = activityScenarioRule<MyActivity>(intent)

@Test
fun myTest() {
    val scenario = rule.scenario
    // Test code goes here.
}
```



## References

- https://developer.android.com/guide/components/activities/testing
- https://developer.android.com/reference/androidx/test/core/app/ActivityScenario
- https://developer.android.com/reference/androidx/test/ext/junit/rules/ActivityScenarioRule
- https://developer.android.com/topic/libraries/architecture/lifecycle#lc
- https://medium.com/stepstone-tech/better-tests-with-androidxs-activityscenario-in-kotlin-part-1-6a6376b713ea