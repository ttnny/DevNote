## Shared Preferences



## Overview

- One of the [options](https://developer.android.com/training/data-storage) to save app data in Android.

- SharedPreferences APIs are used to save a relatively small collection of key-values.
- A `SharedPreferences` object points to a file which is managed by Android and can be private or shared.



## Get

- `getSharedPreferences()` to access shared preference files identified by name (specify with the first parameter). This can be called from any `Context` in your app.

  ```kotlin
  val sharedPref = activity?.getSharedPreferences(
          getString(R.string.preference_file_key), Context.MODE_PRIVATE)
  ```

- `getPreferences()` to access the default shared preference file that belongs to an activity. This should be called from an `Activity`.

  ```kotlin
  val sharedPref = activity?.getPreferences(Context.MODE_PRIVATE)
  ```

- `getDefaultSharedPreferences()` to get the default shared preference file for the entire app to save app [settings](https://developer.android.com/guide/topics/ui/settings).



## Write to

- Create a `SharedPreferences.Editor` by calling `edit()` on the `SharedPreferences` object.

- Pass the keys and values to the methods such as `putInt()` and `putString()`. Then call `apply()` or `commit()` to save the changes.

  ```kotlin
  val sharedPref = activity?.getPreferences(Context.MODE_PRIVATE) ?: return
  with (sharedPref.edit()) {
      putInt(getString(R.string.saved_high_score_key), newHighScore)
      apply()
  }
  ```

- Notes:

  - `apply()` writes update to disk **async** and `commit()` is **sync**, so advoid `commit()` in UI thread.
  - A **more secure** way is to call `edit()` on an `EncryptedSharedPreferences` object instead of `SharedPreferences` object. [[more](https://developer.android.com/topic/security/data)]



## Read from

- Call methods such as `getInt()` or `getString()`, providing the key and optionally a default value to return if the key isn't present.

  ```kotlin
  val sharedPref = activity?.getPreferences(Context.MODE_PRIVATE) ?: return
  val defaultValue = resources.getInteger(R.integer.saved_high_score_default_key)
  val highScore = sharedPref.getInt(getString(R.string.saved_high_score_key), defaultValue)
  ```

  

## References

- https://developer.android.com/training/data-storage/shared-preferences
- https://developer.android.com/reference/androidx/security/crypto/EncryptedSharedPreferences
- https://developer.android.com/topic/security/data
- https://developer.android.com/training/data-storage
- https://developer.android.com/guide/topics/ui/settings