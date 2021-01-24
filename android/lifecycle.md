# Lifecycle



## Setup

\(for both **ViewModel** and **LiveData**\)

Add dependency to `build.gradle` \(app-level\)

```text
implementation 'androidx.lifecycle:lifecycle-extensions:2.2.0'
```



## Implementations

### ViewModel & ViewModel Factory

Create YourViewModel class, extending ViewModel

```text
class YourViewModel : ViewModel() { ... }
```

In YourViewModel class, override onCleared\(\)

```text
override fun onCleared() {
    super.onCleared()
}
```

In YourFragment class, create and initialize a YourViewModel using ViewModelProviders. Don't manually instantiate it.

```text
private lateinit var viewModel: YourViewModel
​
override fun onCreateView(...) {
    viewModel = ViewModelProvider(this)
            .get(YourViewModel::class.java)
}
```

\(optional\) **If data is needed to be passed** under YourViewModel initialization from YourFragment class, utilizing the factory design pattern by using **ViewModel Factory** as below.

Modify the defined YourViewModel class

```text
class YourViewModel(data: Int) : ViewModel() {
    init {
        ...
    }
}
```

And create a new YourViewModelFactory class which will create the ViewModel object for us

```text
class YourViewModelFactory(private val data: Int) : ViewModelProvider.Factory {
    override fun <T : ViewModel?> create(modelClass: Class<T>): T {
        if (modelClass.isAssignableFrom(YourViewModel::class.java)) {
            // TODO Construct and return the YourViewModel
            // e.g.
            // return YourViewModel(data) as T
        }
        throw IllegalArgumentException("Unknown ViewModel class")
    }
}
```

Back to YourFragment class

```text
private lateinit var viewModel: ScoreViewModel
private lateinit var viewModelFactory: ScoreViewModelFactory
​
override fun onCreateView(...): View? {
    viewModelFactory = YourViewModelFactory(<your-data-argument-here>)
    viewModel = ViewModelProvider(this, viewModelFactory)
            .get(YourViewModel::class.java)
}
```

Another way without using ViewModel Factory \(or without adding a constructor for the ViewModel\) is making a setter for the data variable in the ViewModel then call it from onCreateView\(\) in YourFragment.

### LiveData

* Observable \(LiveData\) is any subject in YourViewModel class
* Observers \(other objects\) is any methods in YourFragment class

Create a LiveData in YourViewModel \(LiveData always starts as null value, so init to set default value as needed\)

```text
val data = MutableLiveData<Int>()
​
// OR for better encapsulation and protecting YourViewModel,
// use backing property in Kotlin or similar in Java
​
private val _data = MutableLiveData<Int>()
val data : LiveData<Int>
    get() = _data
```

Setup the observer relationship in YourFragment

```text
viewModel.data.observe(viewLifecycleOwner, Observer { newData ->
    // e.g.
    // binding.dataText.text = viewModel.data.toString()
})
```

No need to override onDestroyView\(\) to clean-up the relationship/connection in YourFragment class because **LiveData is lifecycle-aware** and it will be automatically destroyed.

### Event vs State

LiveData keeps track of data states.

* State: a loading indicator, the most recent email received, etc.
* Event: a notification, navigating to a different screen, a sound playing when a button is pressed, etc.

