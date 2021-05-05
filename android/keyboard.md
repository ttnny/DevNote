# Keyboard



Activity

```kotlin
val imm = getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
imm.showSoftInput(editText, 0)
```



Fragment

```kotlin
val imm = context?.getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
imm.hideSoftInputFromWindow(editText, 0)
```

