# Data Binding



## Setup

Modify `build.gradle` \(app-level\) to enable data binding and sync when prompted

```text
buildFeatures {
    dataBinding = true
​
    // For view binding:
    // viewBinding = true
}
```

In **your\_fragment.xml**, wrap existing XML layout into a &lt;layout&gt; tag. Also, cut and paste any namcespace declarations into the new &lt;layout&gt; tag.

```text
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
​
    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".ui.home.HomeFragment">
    </androidx.constraintlayout.widget.ConstraintLayout>
​
</layout>
```

In **YourFragment.kt**:

```text
override fun onCreateView(
    inflater: LayoutInflater, container: ViewGroup?,
    savedInstanceState: Bundle?
): View? {
    val binding: YourFragmentBinding =
        DataBindingUtil.inflate(inflater, R.layout.your_fragment, container, false)
    
    // ...
}
```

Use data binding as a type-safe way to get views. For e.g.:

```text
binding.getDataButton.setOnClickListener {
    // ...
}
```



## Implementations

### Data Binding with ViewModel

_\(Above setup is required.\)_

Taking data and binding it to a view object without going through UI Controller, which acting as a relay between Views and ViewModel.

Modify **your\_fragment.xml** then rebuild project right after that so all of the generated data binding code is regenerated.

```text
<layout>
    <data>
​
        <variable
            name="yourViewModel"
            type="com.example.android.app.YourViewModel" />
    </data>
</layout>
```

In **YourFragment.kt**, pass the YourViewModel into the data binding:

```text
override fun onCreateView(...) {
    binding.yourViewModel = viewModel
}
```

Back to **your\_fragment.xml**, add a data binding expression to the desired view, for e.g. buttons. This step will setup a onClickListener right in the .xml so we can remove the _setOnClickListener\(\)_ in YourFragment.kt. Now, relay or intermediary is no longer needed.

```text
android:onClick="@{() -> yourViewModel.yourMethod()}"
```

### Data Binding with LiveData

